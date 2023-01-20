---
layout: post
title: 
date: 2021-02-17 13:37:00 +0100
description:  
img: header/making-the-azure-devops-transition.gif
tags: [DevOps, Everything-as-Code, Code]
---
How to create world-class agility, reliability, and security in technology organizations using another way of working?

### Why? 

While working, together with Siebrand, on a project he planted the DevOps seed/mind bomb in the storm of thoughts in my brain. This opened up a whole new world for me; Everything and everywhere is code!

Siebrand also introduced me to The Phoenix Project and The Unicorn Project novels by [Gene Kim/IT Revolution](https://itrevolution.com/). These are must-reads for everyone, also outside of the IT industry.

Reading these novels brought to my attention that a lot of organizations, which I met over the past years, struggle with very similar challenges. I see that the main struggle is that the IT organization within a company is on the wrong side of the general ledger and needs to become a profit center instead of a cost center. IT development and operations will be the way forward to keep up or even ahead of the game of the business its competitors.

*	[The Phoenix Project](https://www.amazon.com/Phoenix-Project-DevOps-Helping-Business/dp/0988262592) - A novel about IT, DevOps, and helping your business win.
*	[The Unicorn Project](https://www.amazon.com/Unicorn-Project-Developers-Disruption-Thriving/dp/1942788762) - A novel about developers, digital disruption and thriving in the age of data.
*	[The DevOps Handbook](https://www.amazon.com/DevOps-Handbook-World-Class-Reliability-Organizations/dp/1942788002) - How to create world-class agility, reliability, and security in technology organizations. 


For the lazy reading, with your eyes closed, some excerpts are available on SoundCloud;

*	[The Phoenix Project, Part 1](https://soundcloud.com/itrevolution/sets/the-phoenix-project-part-2) on SoundCloud.com
*	[Beyond the Phoenix Project](https://soundcloud.com/itrevolution/sets/beyond-the-phoenix-project) on SoundCloud.com
*	[The Unicorn Project](https://soundcloud.com/itrevolution/sets/the-unicorn-project) on SoundCloud.com
*	[The Devops Handbook](https://soundcloud.com/itrevolution/sets/the-devops-handbook) on SoundCloud.com

You might also want to subscribe to [The Idealcast](https://itrevolution.com/the-idealcast-podcast/) for more in-depth insights.

### The Journey

While getting acquainted with this new way of working I must admit I felt some internal resistance; I just want to code! Over time, while diving into The Phoenix Project, I understood that “I just want to code!” isn’t going to cut it. There has to be a higher purpose to the script that I am creating. All scripts should have a common denominator. The reason that I am making a tiny script should address a common goal. Having this in mind, Siebrand and I, started working on assembling scripts into a library of functions. Each function has its purpose and can be easily reused over multiple scripts. Well, doesn’t it make it more complex? Actually there is a thin layer of complexity over this, however, this opens the gates of uniformity. Now all subjacent scripts will make use of common functions. For instance, creating a log file and rotating those log files to prevent the server to clog up. Now all scripts will make use of this function and the log files are created in the same syntax and have a configurable rotation scheme.

Having this concept in mind we needed to make sure that our library will remain available and that a new function in our library wouldn’t jeopardize our other coding. It is time for the versioning of our code. We opted for getting the code checked into the Microsoft Azure (GIT) repository and start using another editor. Siebrand and most other IT co-workers are used to PowerShell ISE. However, the ISE is shipped with Windows the PowerShell ISE is no longer in active feature development and there is no support for the ISE in PowerShell v6 and beyond. I was happily content with using PowerGUI from Quest Dell Quest Software is not on the product list nor being developed anymore. This is certainly not future proof so we head out for a complete IDE environment and we discovered that Microsoft Visual Studio Code was the answer. It has everything we need; the multi-platform, community-driven, open-source, vast majority of extensions, version control, debugging, and code linting. Heading out to Microsoft Visual Studio Code!

### The four types of work

1.	#### Business projects
	Projects that come from the business side of the organization. By facilitating promptly and accurately these projects we help the business and the IT organization will adapt to a profit-center as opposed to being historically viewed as a cost-center.
	
2.	#### Internal IT projects
	Keeping up to today's standards when it comes to infrastructure. a fleet of new network devices, decommissioning a data center, and any number of other internally focused system-based activities. The problem with many of these projects is that all too often internal teams are left to manage them independently. They progress with little oversight or visibility and consume untold amounts of resources, which will often adversely affect progress on Business Projects. 
	
3.	#### Changes
	Every day IT operations will be registering, planning, assessing, building, testing, and deploying changes. This may also include managing the process to deploy a change that relates to the other types of projects.
	
4.	#### Unplanned work / Recovery work
	Unplanned work is recovery work, which almost always takes you away from meeting your goals.

### The three ways

1.	System thinking.
	Improving the flow of work, more efficient working means faster delivery.
	Work always follows the DTAP conveyor belts towards the terminus; from DEV, TST, ACC to PRD.
	Lower the amount of WIP (work in progress) by eliminating the bottlenecks
	Reverting back is the last thing we will do as it is slowing down the betterment.
	Visualize the work, make the pile of work visible in chunks so it’s still a pile but it’s defined in achievable tasks. 

2.	Amplify feedback loops.
	While having a short feedback loop with the customer/business the solution can quickly adapt to the ever-changing business. This ensures that the work is effective and efficient at the same time.
	
3.	Culture of continual experimentation and learning.
	Exploring new ways means that things also will fail. Without failures, there is no progress. Keep striving for the most optimal solution.

### The five ideals

1. Locality and Simplicity.
	Making changes that only affect the local instance so it doesn't end up snowballing on other components. Making a change on one end should break something on the other end. The combination of locality and simplicity allows making changes easy to be overseen and keeps complexity on a low level.
	
2. Focus, Flow, and Joy.
	

3. Improvement of Daily Work

4. Psychological Safety

5. Customer Focus

### How do I get this in practice?

### My VScode toolchain

Microsoft Visual Studio Code (a.k.a. VScode), available on multiple platforms, is a rich (cross-platform) source editor. It sports support for debugging, embedded Git control and GitHub, syntax highlighting, intelligent code completion, snippet, and even code refactoring. Also, it comes with built-in support for JavaScript, TypeScript, and Node.js and has a rich ecosystem of extensions for other languages (such as C++, C#, Java, Python, PHP, Power-Shell, and even Commodore 64 Assembly) and run-times, such as; .NET and Unity.

VScode also has seamless integration with Microsoft Azure DevOps Services. This enables you to check in code, based on GIT, into the cloud.

It’s also possible to use the widely used GitHub, which has been cleverly acquired by Microsoft recently. GitHub is a smart concatenation of two syllables; Git and Hub.

Git is a distributed version control system for tracking changes in source code during software development. It is designed for coordinating work among programmers, but it can be used to track changes in any set of files. Its goals include speed, data integrity, and support for distributed, non-linear workflows.

'Hub' in GitHub represents the HQ for people to share their work, interact with each other and ultimately create better software together. However, GitHub can be used by anyone, not only by software developers!

When gluing the concepts together a foundation has been formed for source code versioning and collaboration, either publicly or privately.
Getting things done (more efficiently)

From all the daily tasks that I have to complete during the day I need to focus on the things that bring to most value to the customer. This isn’t necessarily always adding features to current solutions but certainly also means shortening the processing time for activities. This can be achieved either by creating an automated workflow or by optimizing the current workflow when it comes to getting (more) things done. Ideally, the work we put into automating will pay down the technical debt and thus increasing the overall efficiency of the operation/business.

Altogether this makes me look at work differently. It’s not about creating a fix for a small problem; we are working towards a common goal while keeping our vision in mind.

### Getting my gear up

#### Required software

*	Git for Windows
*	Github Desktop
*	Visual Studio Code
*	Visual Studio Code extensions for business;
	*	Azure Repos – Microsoft
    *	Git Graph – mhutchie
    *	GitLens – Eric Amodio
    *	PowerShell – Microsoft
*	Visual Studio Code extensions for leisure;
    *	KickAss (C64) – Captain JiNX)

#### Setting up the environment