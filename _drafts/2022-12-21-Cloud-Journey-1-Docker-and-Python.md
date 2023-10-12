---
layout: post
title: "Docker and Python"
subtitle: "Cloud Journey 1"
date: 2022-12-21
readtime: true
tags: [Docker, Python, VS Code, Cloud Journey]
cover-img: ["/assets/img/Cloud-Journey-1-Docker-and-Python/2022-12-21-Cloud-Journey-1-Docker-and-Python.jpg" : "by [https://pixabay.com/users/coltsfan-5936078] via Pixabay"]
thumbnail-img: /assets/img/Cloud-Journey-1-Docker-and-Python/2022-12-21-Cloud-Journey-1-Docker-and-Python.jpg
share-img: /assets/img/Cloud-Journey-1-Docker-and-Python/2022-12-21-Cloud-Journey-1-Docker-and-Python.jpg
---

<!--more-->

# Contents

* TOC
{:toc}

**Note:** *Beginning in December of 2022, I started focusing my learning on Cloud technologies. This series will highlight things I've learned and (hopefully) helpful tips for anyone else looking to expand their skill set.*

# Scenario
Advancements in Infrastrucutre as Code products have driven a lot of the automation an efficienices on various cloud platforms. These IaC products are largely Linux-based and frequently use Python. Python is one of the most popular programming languages and learning (or re-learning) this language will help my cloud journey moving forward.

In order to kill two birds with one stone, I will setup a Linux-based Python envrionment using containers. I will get an easily created, light-weight Python environment and gain familiarity with Docker containers. I'm an unabashed Microsoft Windows fan, so I looked at using Docker Desktop for Windows to help in creating this Python enviromnet. 

# Requirements
Before you get started, Docker needs the following [**requirements**](https://docs.docker.com/desktop/install/windows-install/#system-requirements){:target="_blank"} in place. These vary depending on whether you have Windows Pro or another version. With **Home and Pro** can use the Windows Subsytem for Linux (WSL), but With Windows **Pro**, you can install the Hyper-V feature.

## Hardware Requirements
1. Windows 11 64-bit: Home or Pro version 21H2 or higher, or Enterprise or Education version 21H2 or higher
2. Windows 10 64-bit: Home or Pro 21H1 (build 19043) or higher, or Enterprise or Education 20H2 (build 19042) or higher.
3. 64-bit processor with [**SLAT**](https://en.wikipedia.org/wiki/Second_Level_Address_Translation){:target="_blank"}
4. 4GB RAM
5. BIOS-level hardware [**virtualization**](https://docs.docker.com/desktop/troubleshoot/topics/#virtualization){:target="_blank"} support

## Enabling the WSL2 feature on Windows
1. Run PowerShell or Windows Command Prompt as an administrator
2. Install the wsl feature with the following command: `wsl --install`
3. Reboot your system
4. The default Linux distribution installed is Ubuntu

## Enable the Hyper-V feature on windows
1. Run PowerShell or Windows Command Prompt as an administrator
2. Install the the Hyper-V role with the following command:  
    `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All`
3. Reboot your system

## Software Requirements
Now that the hardware requirements are understood, we can review the software requirements. We only need a few components. 
1. [**Visual Studio Code**](https://code.visualstudio.com/download){:target="_blank"}
2. [**Docker VS Code Extension**](https://code.visualstudio.com/docs/containers/overview){:target="_blank"}
3. [**Docker Desktop**](https://docs.docker.com/desktop/){:target="_blank"}
4. A [**Docker Hub**](https://hub.docker.com/signup){:target="_blank"} account. This is free for Personal use.  

# Choices

So which way to go? It depends. If you are familar with Linux or just want a full Linux environment to play and learn with, you should use the Windows Linux Subsystem. 

In my case, I will use the Hyper-V feature as I have Windows 10 Pro. I've used the WLS2 in the past and since I'm not a full time Linux admin, it was not well maintained. Using Docker allows me to create a standarized Linux-based container and I can easily recreate it if any issue occurs. Creating the WLS2 is not a trivial procedure.

# Creating a Docker Container 

I'll assume you have these components installed. If not, you can follow this [**guide from Microsoft**](https://code.visualstudio.com/docs/setup/windows){:target="_blank"} on installing Visual Studio Code on Windows. You can also read about VS Code Extensions [**here**](https://code.visualstudio.com/docs/editor/extension-marketplace){:target="_blank"}. Finally, here's a guide on installing [**Docker Desktop on Windows**](https://docs.docker.com/desktop/install/windows-install/){:target="_blank"}.

Start Docker Desktop and sign into your Docker hub account
![Docker Desktop for Windows](/assets/img/Cloud-Journey-1-Docker-and-Python/dockerdesktop.png "Docker Desktop for Windows")

Open Visual Studio Code and click on the Docker VS Code Extension
![VS Studio Code](/assets/img/Cloud-Journey-1-Docker-and-Python/vscode.png "VS Studio Code")
Under the Help and Feedback section you'll find additional material to learn more about Docker.

The simplest way to create a container to create a DEV Container.
1. 


# Conculsion


# Learning More
* [Install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install){:target="_blank"}
* [Install Hyper-V on Windows 10](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v){:target="_blank"}
* [Docker Desktop](https://docs.docker.com/desktop/){:target="_blank"}
* [Docker on Windows Requirements](https://docs.docker.com/desktop/install/windows-install/#system-requirements){:target="_blank"}



### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for Reading,*  
*Alain Assaf*