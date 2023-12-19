---
layout: post
title: "Docker and Python"
subtitle: "Cloud Journey"
date: 2023-12-19
readtime: true
tags: [Docker, Python, VS Code, Cloud Journey]
cover-img: ["/assets/img/Cloud-Journey-1-Docker-and-Python/2023-12-19-Cloud-Journey-1-Docker-and-Python.jpg" : "by [https://pixabay.com/users/coltsfan-5936078] via Pixabay"]
thumbnail-img: /assets/img/Cloud-Journey-1-Docker-and-Python/2023-12-19-Cloud-Journey-1-Docker-and-Python.jpg
share-img: /assets/img/Cloud-Journey-1-Docker-and-Python/2023-12-19-Cloud-Journey-1-Docker-and-Python.jpg
---

<!--more-->

# Contents

* TOC
{:toc}

# Scenario
Advancements in Infrastructure as Code products have driven a lot of the automation efficiency on cloud platforms. These IaC products are largely Linux-based and frequently use Python. Python is one of the most popular programming languages and learning (or re-learning) this language will help my cloud journey moving forward.

In order to kill two birds with one stone, I will setup a Linux-based Python environment using containers. I will get an easily created, light-weight Python environment and gain familiarity with Docker containers. I'm an unabashed Microsoft Windows fan, so I looked at using Docker Desktop for Windows to help in creating this Python environment. 

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
2. Install the Hyper-V role with the following command:  
    `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All`
3. Reboot your system

## Software Requirements
Now that the hardware requirements are understood, we can review the software requirements. We only need a few components. 
1. [**Visual Studio Code**](https://code.visualstudio.com/download){:target="_blank"}
2. [**Docker VS Code Extension**](https://code.visualstudio.com/docs/containers/overview){:target="_blank"}
3. [**Docker Desktop**](https://docs.docker.com/desktop/){:target="_blank"}
4. A [**Docker Hub**](https://hub.docker.com/signup){:target="_blank"} account. This is free for Personal use.  

# Choices

So which way to go? It depends. If you are familiar with Linux or just want a full Linux environment to play and learn with, you should use the Windows Linux Subsystem. 

In my case, I will use the Hyper-V feature as I have Windows 10 Pro. I've used the WLS2 in the past and since I'm not a full time Linux admin, it was not well maintained. Using Docker allows me to create a standardized Linux-based container and I can easily recreate it if any issue occurs. Recreating the WLS2 is not a trivial procedure.

# Creating a Docker Container 

I'll assume you have these components installed. If not, you can follow this [**guide from Microsoft**](https://code.visualstudio.com/docs/setup/windows){:target="_blank"} on installing Visual Studio Code on Windows. You can also read about VS Code Extensions [**here**](https://code.visualstudio.com/docs/editor/extension-marketplace){:target="_blank"}. Finally, here's a guide on installing [**Docker Desktop on Windows**](https://docs.docker.com/desktop/install/windows-install/){:target="_blank"}.

Start Docker Desktop and sign into your Docker hub account
![Docker Desktop for Windows](/assets/img/Cloud-Journey-1-Docker-and-Python/dockerdesktop.png "Docker Desktop for Windows")

Open Visual Studio Code and click on the Docker VS Code Extension
![VS Studio Code](/assets/img/Cloud-Journey-1-Docker-and-Python/vscode.png "VS Studio Code")
Under the Help and Feedback section you'll find additional material to learn more about Docker.

The simplest way to create a Container in **Visual Studio Code** is to use *Dev Containers*.
1. Press F1 or `Ctrl+Shift+P` to bring up the *Command Palette*
2. Start typing *Dev Containers* and select **Dev Containers: Add Dev Container Configuration Files**
3. You will be prompted to select a folder to save the configuration files.
4. Once VSCode reopens, select **Dev Containers: Add Dev Container Configuration Files** again and you will get a list of predefined Docker config files that are available.

<img src="/assets/img/Cloud-Journey-1-Docker-and-Python/configfiles.png">{: .mx-auto.d-block :}

<ol start = "5">
    <li>Search for Python. In this example, I'm choosing *Basic Python*</li>
</ol>

<img src="/assets/img/Cloud-Journey-1-Docker-and-Python/basicpython.png">{: .mx-auto.d-block :}

<ol start = "6">
    <li>You can then choose your preferred version of Python.</li>
</ol>

<img src="/assets/img/Cloud-Journey-1-Docker-and-Python/pythonver.png">{: .mx-auto.d-block :}

<ol start = "7">
    <li>Finally, you can select any additional features to install.</li>
</ol>

<img src="/assets/img/Cloud-Journey-1-Docker-and-Python/pythonfeature.png">{: .mx-auto.d-block :}

<ol start = "8">
    <li>VSCode will prompt to open the configuration file in a container</li>
</ol>

<img src="/assets/img/Cloud-Journey-1-Docker-and-Python/openincontainer.png">{: .mx-auto.d-block :}

<ol start = "9">
    <li>Now, VSCode will build the container.</li>
</ol>

<img src="/assets/img/Cloud-Journey-1-Docker-and-Python/buildingcontainer.png">{: .mx-auto.d-block :}

<ol start = "10">
    <li>Once the build completes, you will see a container running in <b>Docker Desktop</b>.</li>
</ol>

<img src="/assets/img/Cloud-Journey-1-Docker-and-Python/runningcontainer.png" width="800">{: .mx-auto.d-block :}

# Using your new Container 

You can access the container in **Docker Desktop** by clicking on it and going to the Exec tab.

![Docker Desktop Exec](/assets/img/Cloud-Journey-1-Docker-and-Python/dockerexec.png "Docker Desktop Exec")

You can also write programs and access the container in **VSCode**.

![VSCode Python Container](/assets/img/Cloud-Journey-1-Docker-and-Python/vscodepython.png "VSCode Python Container")

# Conclusion
I hope this post has shown you that you can leverage **Docker Desktop** and **VSCode** to create development containers for coding, testing, or whatever need you require. As long as your system meets the requirements covered above, you can run these tools for free.

# Learning More
* [Install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install){:target="_blank"}
* [Install Hyper-V on Windows 10](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v){:target="_blank"}
* [Docker Desktop](https://docs.docker.com/desktop/){:target="_blank"}
* [Docker on Windows Requirements](https://docs.docker.com/desktop/install/windows-install/#system-requirements){:target="_blank"}
* [Setting up Visual Studio Code](https://code.visualstudio.com/Docs/setup/setup-overview){:target="_blank"}
* [VS Code Dev Containers](https://code.visualstudio.com/docs/devcontainers/create-dev-container){:target="_blank"}


# Value for Value
If you received any value from reading this post, please [**buy me a coffee**](https://www.buymeacoffee.com/j72aXgIYJh){:target="_blank"} to show your support.
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="j72aXgIYJh" data-color="#16609f" data-emoji="☕"  data-font="Arial" data-text="Buy me a coffee" data-outline-color="#ffffff" data-font-color="#ffffff" data-coffee-color="#FFDD00" ></script>

*Thanks for Reading,*  
*Alain Assaf*