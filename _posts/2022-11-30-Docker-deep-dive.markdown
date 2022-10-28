---
layout: post
title: Docker Deep Dive
date: 2022-09-30 13:37:00 +0100
description: My experience while taking a deep dive into Docker
img: header/docker-deep-dive.png
tags: [Docker, Linux]
#published: false
---

# Why

# Installing Docker Desktop on Ubuntu-distro Desktop


# Creating a docker image from a docker file

First rule; experiment as much as needed as this is a itirative process as mentioned be a [friend](https://github.com/wezzynl) of me. First experiment and later create the final image.

## dockerfile
---
FROM ubuntu:latest

ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes
RUN apt-get update

RUN apt install software-properties-common
RUN add-apt-repository --yes --update ppa:ansible/ansible

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    jq \
    git \
    iputils-ping \
    libcurl4 \
    libunwind8 \
    netcat \
    libssl1.0 \
    python3 \
    python3-pip \
    ansible \
    wget \
    apt-transport-https \
  && rm -rf /var/lib/apt/lists/*

# Azure CLI
RUN curl -LsS https://aka.ms/InstallAzureCLIDeb | bash \
  && rm -rf /var/lib/apt/lists/*

# WinRM for Ansible
# RUN pip install "pywinrm>=0.3.0"
RUN pip install pywinrm

# Ansible and the azure requirements
RUN curl https://raw.githubusercontent.com/ansible-collections/azure/dev/requirements-azure.txt --output requirements-azure.txt
RUN pip install -r requirements-azure.txt
RUN ansible-galaxy collection install azure.azcollection --force

# Hashicorp products (Packer & Terraform)

RUN curl -fsSL https://apt.releases.hashicorp.com/gpg | apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
RUN apt-get update
RUN apt-get install packer terraform

# Microsoft PowerShell
RUN wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
RUN dpkg -i packages-microsoft-prod.deb

RUN apt-get update
RUN apt-get install powershell

# Clean up

RUN apt autoremove --purge
RUN apt clean

RUN df -h

# Azure DevOps Agent
ARG TARGETARCH=amd64
ARG AGENT_VERSION=2.194.0

WORKDIR /azp

RUN if [ "$TARGETARCH" = "amd64" ]; then \
      AZP_AGENTPACKAGE_URL=https://vstsagentpackage.azureedge.net/agent/${AGENT_VERSION}/vsts-agent-linux-x64-${AGENT_VERSION}.tar.gz; \
    else \
      AZP_AGENTPACKAGE_URL=https://vstsagentpackage.azureedge.net/agent/${AGENT_VERSION}/vsts-agent-linux-${TARGETARCH}-${AGENT_VERSION}.tar.gz; \
    fi; \
    curl -LsS "$AZP_AGENTPACKAGE_URL" | tar -xz

COPY ./start.sh .
RUN chmod +x start.sh

ENTRYPOINT [ "./start.sh" ]
---

## startup.sh

--- 
#!/bin/bash
set -e

if [ -z "$AZP_URL" ]; then
  echo 1>&2 "error: missing AZP_URL environment variable"
  exit 1
fi

if [ -z "$AZP_TOKEN_FILE" ]; then
  if [ -z "$AZP_TOKEN" ]; then
    echo 1>&2 "error: missing AZP_TOKEN environment variable"
    exit 1
  fi

  AZP_TOKEN_FILE=/azp/.token
  echo -n $AZP_TOKEN > "$AZP_TOKEN_FILE"
fi

unset AZP_TOKEN

if [ -n "$AZP_WORK" ]; then
  mkdir -p "$AZP_WORK"
fi

export AGENT_ALLOW_RUNASROOT="1"

cleanup() {
  if [ -e config.sh ]; then
    print_header "Cleanup. Removing Azure Pipelines agent..."

    # If the agent has some running jobs, the configuration removal process will fail.
    # So, give it some time to finish the job.
    while true; do
      ./config.sh remove --unattended --auth PAT --token $(cat "$AZP_TOKEN_FILE") && break

      echo "Retrying in 30 seconds..."
      sleep 30
    done
  fi
}

print_header() {
  lightcyan='\033[1;36m'
  nocolor='\033[0m'
  echo -e "${lightcyan}$1${nocolor}"
}

# Let the agent ignore the token env variables
export VSO_AGENT_IGNORE=AZP_TOKEN,AZP_TOKEN_FILE

source ./env.sh

print_header "1. Configuring Azure Pipelines agent..."

./config.sh --unattended \
  --agent "${AZP_AGENT_NAME:-$(hostname)}" \
  --url "$AZP_URL" \
  --auth PAT \
  --token $(cat "$AZP_TOKEN_FILE") \
  --pool "${AZP_POOL:-Default}" \
  --work "${AZP_WORK:-_work}" \
  --replace \
  --acceptTeeEula & wait $!

print_header "2. Running Azure Pipelines agent..."

trap 'cleanup; exit 0' EXIT
trap 'cleanup; exit 130' INT
trap 'cleanup; exit 143' TERM

# To be aware of TERM and INT signals call run.sh
# Running it with the --once flag at the end will shut down the agent after the build is executed
./run.sh "$@" &

wait $!
---

## Iterating
While iterating I use the following command line. 
`docker build --progress=plain -t myimage:latest .  2>&1 | tee myimage-build.log`

## Final image run
This makes use of the `--no-cache` to ensure that everything is 100% freshly loaded.

`docker build --no-cache --progress=plain -t myimage:latest . 2>&1 | tee myimage-build.log`

## Inspecting our create image

https://github.com/wagoodman/dive