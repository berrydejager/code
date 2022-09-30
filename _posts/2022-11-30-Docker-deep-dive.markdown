---
layout: post
title: Docker Deep Dive
date: 2022-10-30 13:37:00 +0100
description: My experience while taking a deep dive into Docker
img: header/docker-deep-dive.png
tags: [Docker, Linux]
---

# Why

# Installing Docker Desktop on Ubuntu-distro Desktop


# Creating a docker image from a docker file

First rule; experiment as much as needed as this is a itirative process as mentioned be a [friend](https://github.com/wezzynl) of me. First experiment and later create the final image.

## Iterating
While iterating I use the following command line. 
`docker build --progress=plain -t myimage:latest .  2>&1 | tee myimage-build.log`

## Final image run
This makes use of the `--no-cache` to ensure that everything is 100% freshly loaded.

`docker build --no-cache --progress=plain -t myimage:latest . 2>&1 | tee myimage-build.log`

## Inspecting our create image

https://github.com/wagoodman/dive