---
layout: post
title: DevOps using a Commodore_64
date: 2021-12-31 13:37:00 +0100
description: This blog describes the use case of compiling a .ASM source file into a .PRG file while using GitHub Workflow. The GitHub Workflow Runner runs, in my case, on a `Pi Zero 2 W`. The Pi is mounted internally into the Commodore casing. The CI workflow runs after compiling the binary .PRG on the Ultimate64. 
img: header/devops64.png
tags: [DevOps, GitHub, Commodore 64, C64, Ultimate64, 1541U2, 1541U2+, KickAssembler]
---
# Why DevOps using a Commodore 64?

Exploring the possibilities of current and vintage technology is always fascinating. Combining the technology of 1982 with the 'new world' technologies makes me smile. It shows that the old technology is [still relevant to this day](https://csdb.dk/latestreleases.php).

My childhood was filled with hexadecimal numbers, monochrome green screens, buzzing transformers and rattling disk drives and I absolutely loved it. Days went by really fast and nights were getting too short.

Playing games wasn't part of my routine. Although the internet wasn't a thing yet, [we](https://en.wikipedia.org/wiki/Demoscene) had to travel to the copy-parties or swap disks using postal services. Games and demos went 'viral' on those parties, the 0-day warez had another meaning; getting a cracked game or demo on the day of the release. Crackers put an intro in front of the game to sign their crack. 

That [crack intro](https://csdb.dk/release/?id=53390) or [demo](https://csdb.dk/release/?id=4986) was for me the most interesting thing. How does that FLD-effect work? How does that beam go in front and on the back of the text? How to make a smooth scroller with colors flowing over it? I wanted to learn assembly (machine language) programming on the Commodore 64. Having a book on my lap, I started to dig my way through the Bible of the 6510 religion; [Programmers Reference Guide](assets/pdf/C64PRG.pdf).

Many years later now I am still getting sparkly joy from the C64. Combining this with the currerent technology is even more fun!

# What do you need?

Next to the interest in retro-computing you need to have some hardware, which is not very common in this cloud/virtual realm we live in.

1.  Commodore 64
2.  1541 Ultimate2(+) or Ultimate64 device 
3.  Raspberry Pi
4.  GitHub account

![](/assets/img/ultimate64_rpi-zero-2-w.png)

# How to use the setup?

The [READme.md](https://github.com/6510nl/DevOps64/blob/main/README.md) on the [DevOps64 repository](https://github.com/6510nl/DevOps64) contains the requirements and the instructions on how to create a similar setup to get things going.

Essentially;

*   Fork the repo
*   Install pre-configured Raspberry Pi OS on a SD-card.
*   Insert the card in the RPi
*   Boot the RPi
*   Setup the GitHub Runner on the RPI
*   Start up your 'Ultimate' C64
*   Start the Workflow from the GitHub website.

Have fun!