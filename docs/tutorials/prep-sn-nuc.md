# Deploy a Secret Node on your NUC
Guide Version 0.1 | Date Sep 22, 2019 | Draft | Pre-Genesis Games Edition

This guide will cover how to install the prerequisite software needed for your SGX compatible NUC. Once completed your Secret Node will be prepared for the Genesis Games.

# Intel NUC Overview

Next Unit of Computing (NUC) is a line of small-form-factor barebone computer kits designed by Intel. While [Enigma](https://enigma.co) has [partnered with Intel](https://blog.enigma.co/announcing-enigmas-collaboration-with-intel-43bbf73a86a7) to work on SGX integration, it is not a requirement that you use an Intel NUC for your Secret Node.

**Intel NUC Models Tested**
1. `Intel NUC 8i7BEK`

# Part 1 - Enabling SGX in the BIOS

1. Select your newly created server. Hint : You can easily spot it named as the server hostname you selected in the previous step.

> Note : Take note of the password vultr sets as root on the server page.

2. Select "View Console".

3. During bootup press “Del” to get into the bios.

Navigate through the bios as follows.
* Advanced
* Chipset Configuration
* System Agent (SA) Configuration
* Enable "SW Guard Extensions (SGX)"

Now exit and save the settings.

# Part 2 - Installing Ubuntu Server 18.04
Getting the right configuration on Vultr.

1. Sign up for an account at [Vultr.com](https://www.vultr.com). [Help support secretnodes.org by using our affiliate link.](https://www.vultr.com/?ref=8255176)

2. Deploy New Instance

3. Choose Server : Bare Metal

4. Server Location : Any location near you.

5. Server Type : 64 bit OS : Ubuntu 18.04 x64

6. Disk Configuration : RAID 1

7. Server Hostname & Label : What ever you want!

# Part 2 - Remote into your Secret Node

1. Get the password for your VPS from the server page.

2. If you're using Windows 10 64-bit or newer, launch PuTTY then enter the IP address of your Secret Node into the "Host Name" field. Then click open.

If you don't already have putty then download it.
* Go to [Putty.org](https://www.putty.org/)
* Click "here" where it says "You can download PuTTY here."
* Below the "Package Files" section, see "MSI (‘Windows Installer’)" and download the most recent 64-bit version of Putty.
* Run the Installer.
* Run Putty.
* Enter the IP address of your Secret Node into the "Host Name" field. Then click open.


3. On OSX, or Linux open Terminal and login to your node. (Skip if using Windows.)

```bash
ssh root@<your-nodes-ip>
```
> NOTE: (1) Be sure to replace <your-nodes-ip> with your nodes ip address. (2) If asked to add an ECDSA fingerprint, answer yes.

# Part 3 - Creating non-root User

Create a non-root User. Here we will create a user named asn, you can substitute this for anything.
```bash
USERNAME=asn
```

This will permission this user to access log files and sudo.
```bash
useradd -m -s /bin/bash -G adm,systemd-journal,sudo $USERNAME && passwd $USERNAME
```

Now exit your terminal session and start a new one, this time login with the new user you created with the password you set.
```bash
ssh asn@<your-nodes-ip>
```
Next proceed to Part 4.

# Part 4 - Package Installation and Initial Configuration

Here we will download the Discovery Docker Network, and scripts for configuring and installing Docker, Docker Compose, and The Intel SGX Driver. Running this script will automate the process.

```bash
wget https://raw.githubusercontent.com/secretnodes/scripts/master/sendnodes.sh
```

Run the bash script.
```bash
sudo bash sendnodes.sh
```
> Note : While running the scripts, say yes to all prompts.

## Install SGX

This script will download and install all relevant SGX files and drivers. This is a prerequisite needed for running a Secret Node.

Run the bash script.
```bash
sudo bash install-sgx.sh
```
> Note : While running the scripts, say yes to all prompts.
> When prompted "Do you want to install in current directory? [yes/no]" respond with "yes".

## Install Docker & Docker Compose

This script will download and install Docker & Docker Compose. This is a prerequisite needed for running a Secret Node.

Run the bash script.
```bash
sudo bash install-docker.sh
```
> Note While running the scripts, say yes to all prompts.

# Part 5 - Deploy Secret Node

Running this script will start 9 enigma workers.

> Note 9 is the max number of workers.

Run the bash script.
```bash
sudo bash start.sh
```

Congratulations! 🎉 Your Secret Node is now prepared for the Genesis Games. Please note until the networked version of testnet is launched of the enigma network, your node will be in non-networked mode.