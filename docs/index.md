# Intelligent Document Processor Installation Documentation

## Overview


This documentation contains the step by step guide to set up the workstation for the use of **Intelligent Document Processor**.
It will include the installation of the operrating system, correct drivers, developement tools and finally the application itself.

[TOC]

<a id="quickstart"></a>
## Quickstart

<a id="prerequisites"></a>
### Prerequisites
- [Python 3.10](https://docs.python.org/3.10/): Required for compatibility with the library.  
- System Requirements: Linux-based system (e.g., [Ubuntu](https://ubuntu.com/download/desktop)) with internet access for dependency installation.  
- [Docker](https://docs.docker.com/engine/install/ubuntu/): Required for running the dockerised application.  

<a id="installation"></a>
## Installation



### 1. Initial Ubuntu 24.04 Installation
You need a working Ubuntu installation

#### 1.1 Download and Prepare 

- **Download**: Get the official [Ubuntu 24.04 LTS .iso file](https://releases.ubuntu.com/24.04/) from the Ubuntu website.  
- **Create Bootable USB**: Use a tool like [Rufus](https://rufus.ie/) (Windows) or [balenaEtcher](https://etcher.balena.io/) (Cross-platform) to create a bootable USB drive.  
- **Boot**: Restart your computer, enter the BIOS/UEFI settings (usually by pressing Del, F2, or F12), and select the USB drive as the boot device.  

#### 1.2 Installation Steps 

1. **Live Session:** When the installer loads, choose the "Try Ubuntu" option first. This lets you check if everything works. 

2. **Start Install:** Double-click the "Install Ubuntu 24.04 LTS" icon. 

3. **Third-Party Software:** On the "Updates and other software" screen, check the box for "Install third-party software for graphics and Wi-Fi hardware and additional media formats." This is crucial; it helps install the initial, official NVIDIA drivers. 

5. **Finish:** Follow the remaining steps (disk partitioning, region, and user account) to complete the installation. 

6. **Restart:** When prompted, remove the USB drive and restart the computer. 

### 2. System Updates and Cleanup 
This ensures you have the latest security patches and stable software versions. 

#### 2.1 Update the System 
1. Open the Terminal application. 

2. Run the following commands, pressing  after each one. You will be asked for your password. 
   
    * Update the list of available software packages:
    ```bash     
    sudo apt update 
    ``` 

    * Upgrade all installed packages to their latest versions: 
    ```bash 
    sudo apt upgrade -y 
    ```
    * Remove any unnecessary packages that were installed as dependencies but are no longer needed: 
    ```bash 
    sudo apt autoremove -y
    ```

### 3. Install Docker
Install Docker on your system using the following Bash commands to enable dockerised application support:

```bash
sudo apt update
sudo apt install \
ca-certificates \
curl \
gnupg \
lsb-release -y

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

sudo systemctl enable docker
sudo systemctl start docker

newgrp docker

docker --version
docker run hello-world
```  

### 4. NVIDIA GPU Driver Installation 

The official Ubuntu repository contains certified, stable NVIDIA drivers. This is the safest, most reliable method. 

#### 4.1 Install the Recommended Driver 


The official Ubuntu repository contains certified, stable [NVIDIA drivers](https://help.ubuntu.com/community/BinaryDriverHowto/Nvidia).

1. Open the **"Software & Updates"** application (you can search for it in the main application menu). 

2. Go to the **"[Additional Drivers](https://help.ubuntu.com/stable/ubuntu-help/hardware-driver.html)"** tab. 

3. The system will scan for proprietary drivers. You should see a list of available NVIDIA drivers. 

4. **Select the highest numbered**, tested driver that says **"(proprietary, tested)"** next to it. For example, if you see nvidia-driver-550, select that one. 

5. Click **"Apply Changes"** and wait for the installation to finish. 

6. **Reboot:** You must restart the computer for the new drivers to take effect. 
    ```bash 
    reboot 
    ``` 

#### 4.2 Verify the Installation 

After rebooting, check that the driver is active and working. 

1. Open the Terminal. 

2. Run the NVIDIA System Management Interface tool: 
    ```bash 
    nvidia-smi 
    ```

1. **Success:** If you see a table showing your GPU model (e.g., RTX 6000 Ada), the driver version, and current GPU usage, the installation was successful. 

### 5. Setting up Python for Development 

#### 5.1 Install Python 3.10:
   Install Python 3.10 using the following commands in a Bash terminal:
   
   ```bash
   sudo apt update
   sudo apt upgrade
   sudo apt autoremove
   sudo add-apt-repository ppa:deadsnakes/ppa
   sudo apt update
   sudo apt install python3.10 python3.10-venv python3.10-distutils python3.10-dev -y
   ```
   ```bash
   curl -sS https://bootstrap.pypa.io/get-pip.py | sudo python3.10
   ``` 

#### 5.2 Install Development Dependencies 
Open the Terminal and install the tools needed for compiling software and managing Python environments: 
```bash 
sudo apt install build-essential git cmake python3-dev 
``` 

#### 5.3 Use Virtual Environments (Recommended) 

Virtual Environments (venv) create a separate, isolated folder for each Python project. This prevents conflicts between projects and keeps the system Python clean. 

1. **Create a Project Folder:** 
    ```bash
    mkdir ~/my_python_project 
    cd ~/my_python_project 
    ``` 
2. **Create the Virtual Environment:** Use the system's python3 to create a virtual environment named .venv. 
    ```bash 
    python3.10 -m venv .venv 
    ``` 
3. **Activate the Environment:** You must activate the environment every time you start
   ```bash
   source .doc_env/bin/activate
   ```
### 6. Application installation

For the application installation first the application has to be downloaded and then the required packages have to be installed in the virtual environment

#### 6.1 Copy the Repository Zip:
   Navigate to your target directory, copy the code zip, extract the code and open the IDP directory:
    ```bash
    cd IDP
    ```

#### 6.2 Install Dependencies:
   Run the following commands in the same terminal to install the necessary packages:
    ```bash
    pip install -r reqirements.txt
    ```