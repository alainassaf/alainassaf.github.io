---
layout: post
title: "Cloud Journey 1: Docker and Python"
date: 2022-12-21
readtime: true
tags: [Docker, Python, VS Code, Cloud Journey]
cover-img: ["/assets/img/Cloud-Journey-1-Docker-and-Python/2022-12-21-Cloud-Journey-1-Docker-and-Python.jpg" : "Pixabay"]
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

In order to kill two birds with one stone, I want to setup a Linux-based Python envrionment using containers. I will get an easily created, light-weight Python environment and gain familiarity with Docker containers. I'm an unabashed Microsoft Windows fan, so I looked at using Docker Desktop for Windows to help in creating this Python enviromnet. 

# Requirements
In order to get started, you will need the following [**requirements**](https://docs.docker.com/desktop/install/windows-install/#system-requirements). These vary depending on whether you have Windows Pro or another version. With **Home and Pro** can use the Windows Subsytem for Linux (WSL). With Windows **Pro**, you can install the Hyper-V feature.

## Hardware Requirements
1. Windows 11 64-bit: Home or Pro version 21H2 or higher, or Enterprise or Education version 21H2 or higher
2. Windows 10 64-bit: Home or Pro 21H1 (build 19043) or higher, or Enterprise or Education 20H2 (build 19042) or higher.
3. 64-bit processor with [**SLAT**](https://en.wikipedia.org/wiki/Second_Level_Address_Translation)
4. 4GB RAM
5. BIOS-level hardware [**virtualization**](https://docs.docker.com/desktop/troubleshoot/topics/#virtualization) support

## Enabling the WSL2 feature on Windows
1. Run PowerShell or Windows Command Prompt as an administrator
2. Install the wsl feature with the following command: `wsl --install`
3. Reboot your system
4. The default Linux distribution installed is Ubuntu

## Enable the Hyper-V feature on windows



# Body


To start, you will need to download Docker for Windows from their official website. Once it is installed, you will need to enable the Hyper-V feature in your Windows operating system. This can be done by going to the Control Panel, selecting Programs and Features, and then clicking on "Turn Windows features on or off". In the window that pops up, check the box next to Hyper-V and click "OK".

Next, you will need to restart your computer to apply the changes. Once it has restarted, you can launch Docker for Windows and click on the "Settings" option in the top right corner. From there, you can choose which version of Docker you would like to use, as well as configure any additional settings to your liking.

Once your Docker settings are configured, you can start creating and running Docker containers. This is done by using the Docker command line interface, which can be accessed by clicking on the Docker icon in your system tray. From there, you can use various Docker commands to create, run, and manage your containers.

One thing to note is that, by default, Docker for Windows only runs Linux containers. However, you can also run Windows containers by switching between the two modes in the Docker settings.

Overall, setting up Docker on Windows may seem like a daunting task at first, but with a few simple steps, you can easily have your own Docker environment up and running. This allows you to quickly and easily create, run, and manage Docker containers, providing you with a powerful tool for developing and testing your applications.

-------------------------------------------------------------------------------------------------------------------------------------

Using Docker containers with Visual Studio Code can greatly enhance your development workflow and allow you to easily manage and deploy your applications.

To get started, you will need to have Docker installed on your system, as well as the Docker extension for Visual Studio Code. Once both of these are installed, you can access the Docker extension by clicking on the Docker icon in the left sidebar of Visual Studio Code.

From there, you can easily create, run, and manage Docker containers directly from within Visual Studio Code. This allows you to quickly spin up a container for your application, test it, and then deploy it to production with minimal effort.

One of the key benefits of using Docker with Visual Studio Code is the ability to easily manage multiple containers and applications. The Docker extension provides a user-friendly interface that allows you to view, stop, and restart your containers with just a few clicks. You can also easily switch between different containers and applications, making it easy to work on multiple projects at the same time.

Another great feature of the Docker extension is the ability to debug your applications directly within the container. This allows you to quickly identify and fix any issues that may arise, saving you time and effort in the long run.

Overall, using Docker containers with Visual Studio Code is a powerful combination that can greatly enhance your development workflow. Whether you are working on a simple project or a large-scale application, Docker and Visual Studio Code can help you quickly and easily manage and deploy your applications.

# Conculsion


# Learning More
* [Docker Desktop](https://docs.docker.com/desktop/)
* [Docker on Windows Requirements](https://docs.docker.com/desktop/install/windows-install/#system-requirements)


### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for Reading,*
*Alain Assaf*
