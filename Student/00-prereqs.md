# Challenge 0: Pre-requisites - Ready, Set, GO! 

**[Home](../README.md)** - [Next Challenge >](./01-containers.md)

## Introduction

A smart cloud solution architect always has the right tools in their toolbox. 

## Description

In this challenge we'll be setting up all the tools we will need to complete our challenges.  This challenge is prework intended to be completed before you arrive for the hack.

### Personal Workstation
- Install the recommended toolset:
    - [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
    - [git](https://git-scm.com/downloads)
    - [Visual Studio Code](https://code.visualstudio.com/Download)
    - Visual Studio Code recommended extenstions:
      - Docker Extension
      - Kubernetes Extension
      - Remote SSH Extension
    - Optional but recommended: [Lens Kubernetes GUI](https://docs.k8slens.dev/v4.1.4/) and/or [K9S](https://k8slens.dev/)

All of the challenges can be completed on a Linux workstation within the Bash Shell.  You can also do most of the challenges from a Windows desktop, however, some of the early challenges which require you to containerize applications using docker must be completed from a Linux workstation, since the containers used for this hack are Linux containers.  If you do not have a Linux workstation (which we assume), you have two choices:
- Install the Windows Subsystem for Linux, Version 2 in your Windows Laptop, and then use that for at least the Linux portions
- Use a Linux virtual machine in Azure for the Linux Portions.

### Deploying Linux and Docker on Windows with WSL2
If you choose to use the first option, instructions on installing WSL2 can be found here: [WSL2](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
You will also need to install docker in the WSL2 subsystem: [Docker Installation](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers)

You will then need to copy the code that you witll need into the Lunux subsystem.  There are two folders that you need: "Student/Resources/Chapter 0/content-api" and "Student/Resources/Chapter 0/content-web"

### Deploying and Access a Linux VM Build Server
If you choose to use the second option, we have already created a virtual machine template for your use, which also contains the code which will need to be containerized.

If you choose to use a VM in Azure, deploy the build machine VM with Linux + Docker using the provided ARM Template and parameters file in the "Student/Resources/Chapter 0/build-machine" folder of this repo.  You can run the following script, after logging in with the Azure CLI:
```
RG="akshack-RG"  #Change as appropriate
LOCATION="EastUS"  # Change as appropriate
az group create --name $RG --location $LOCATION
az deployment group create --name buildvmdeployment -g $RG \
    -f docker-build-machine-vm.json \
	-p docker-build-machine-vm.parameters.json
```
You will then need to ssh into the machine (which is using port 2266):

`ssh -p 2266 wthadmin@12.12.12.12 # replace 12.12.12.12 with the public IP of the vm`

- **Tip:** You can complete almost all of the challenges with the Azure Cloud Shell!  But be a good cloud architect and make sure you have experience installing the tools locally.

You need to clone this repo locally (regardless of where you are working):  `git clone https://github.com/tbd`

## Success Criteria

1. You have a bash shell and docker at your disposal (WSL, Mac, Linux)
2. Running `az --version` shows the version of your Azure CLI
3. Running `docker ps` shows that docker is running 
4. Visual Studio Code is installed.
