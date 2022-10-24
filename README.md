# How to Build a Low-Cost HPC Cluster
###### Last modified 
> This guide is the result of an independent study performed by [Nathan H. Silverman](https://www.linkedin.com/in/nathan-silverman-b35767173/), a  [Kalamazoo College](https://www.kzoo.edu)'s student in Computer Science, under the supervision of Sandino Vargas-Pérez. 
>
> The guide presents the list of components needed to create a low-cost Bewolf-type cluster, helpful pictures, instructions for assembling hardware, software installation, commands to execute proper configuration, and more.
>
> For questions, please contact Dr. Vargas-Pérez via [e-mail](mailto:sandino.vargasperez@kzoo.edu).

The guide was designed to build a cluster using 4 Raspberry Pi (RPi) computers, but it can be used to build a cluster of any size. The step-by-step instructions will detail how to assemble hardware, configure software, and the setup needed to have a fully functioning RPi cluster. Raspberry Pi 3 B+s were used for this project, although with a few tweaks, any modern RPi should work.

# Hardware Setup and Assembly

Below are instructions to assemble a 4 RPi cluster. One RPi will be used as a head node, and the rest will be designated as worker nodes. The following diagram shows the layout for the cluster and its components: <br><br><img src="img/cluster.png" alt="cluster diagram">

### Materials Required:
*The materials needed might change depending on their availability.*

- 4 Raspberry Pi (RPi) computers
- 4 MicroSD cards (32 or 64 GB w/adapter)
- 5 Ethernet cable (cat 5e or 6, 1 foot in length or more)
- 4 Micro USB Cords
- 1 Ethernet switch (with 5 ports or more)
- 1 USB power adapter (with 5 ports or more, 2.4 amps per port)
- 1 Stackable acrylic case for RPis
	- Make sure this item includes 4 layers, heatsinks, and it is compatible with the RPi model chosen <a href="https://www.amazon.com/dp/B07BGYGLZG/?coliid=I3A352BNN34AM2&colid=GT5EJQ2GW4AP&psc=1" target="_blank">&#10697;</a>. 
- Recommended tools:
	- Tweezers (Optional)
	- Screw driver
	- Monitor (or Ethernet to USB adapter to connect via VNC)

## Hardware Assembly

1. Installing Heat Sink: The stackable acrylic case included heat sinks. Install three heat sinks per RPi; two larger ones and a smaller one. The two larger heat sinks go on the Broadcom CPU and the RAM chip, the smaller one goes on the Ethernet and USB controller. Install heat sinks by removing the filament on the bottom and pushing firmly until they are attached securely. For all RPis: 
	1. Install a large heat sink on Broadcom CPU and a small heat sink on the Ethernet and USB controller, both located on top of the RPi:<br><img src="img/fig1.jpeg" alt="fig 1" width="50%" height="50%">
	1. Install a large heat sink on the ram chip located on the bottom of the RPi:<br><img src="img/fig2.jpeg" alt="fig 1" width="50%" height="50%">

1. Place the RPi on acrylic plate. For all RPis:
	1. Review any included instructions for the case. 
	1. Remove protective films from each side of the plate. (This can be difficult; use a screwdriver or tweezers).
	1. Place screws in the plate such that the screw heads are against the plate and the threads are pointing up, making sure that it is the correct orientation. The square cutout in the plate should line up with the heatsink on the bottom of the RPi.
	1. Put spacers on the screws on top of the plate: **screw -> plate -> spacer**.
	1. Place RPi through the screws such that the heat sink on the bottom fits through the square cutout: **screw &rarr;  plate &rarr;  spacer &rarr; Pi**. 
	1. Place and tighten nuts on the screws on top of the RPi, making sure to tighten them: **screw &rarr; plate &rarr; spacer &rarr; RPi &rarr; nut**. 

1. Stack RPis to form a tower:
	1. Put the screw end of the nub through the plate into the open end of the leg to keep it in place. Repeat for all four of the first layers: **nub &rarr; plate &rarr; leg**
	1. Place the next plate on top of the screw end of the leg: **leg &rarr; plate &rarr; leg**
	1. Place the final plate on legs then and secure it with the silver nut on top: **leg &rarr; plate w/o Pi &rarr; silver nut**

# Software Installation and Setup

The next sections will walk through setting up the microSD cards for the cluster. These examples were using macOS, but all the software needed can be downloaded for Windows OS.

### Software Download Requirements:
- Latest version of **Raspberry Pi OS with desktop** for head node and **Raspberry Pi OS Lite** for worker node <a href="https://www.amazon.com/dp/B07BGYGLZG/?coliid=I3A352BNN34AM2&colid=GT5EJQ2GW4AP&psc=1" target="_blank">&#10697;</a>.
- **Etcher** to install (flash) Raspberry Pi OS images into the microSD card <a href="https://www.balena.io/etcher/" target="_blank">&#10697;</a>.
- **VNC Viewer** to connect to the head node using an external computer (optional) <a href="https://www.realvnc.com/en/connect/download/viewer/" target="_blank">&#10697;</a>.


## Setting Up MicroSD Cards 

1. Formatting MicroSD Card:
	1. Plug microSD card into a computer. *Use a USB to microSD adapter if the computer doesn't have microSD or SD card slots*.
	1. On macOS: open **Disk Utility** and navigate to the microSD card. Select **Erase** in the top center of the window. In the "Format" dropdown menu, select "MS-DOT (FAT)". Click **Erase** (the name of the device does not matter).
	1. On Windows: use **Windows Explorer** to locate the microSD card, right-click on it, and select **Format**. Select **Quick Format** and click start.
1. Open Etcher to flash the microSD card:
	1. Click **Select image** and navigate to where the Raspberry Pi OS was downloaded. The files should end in `.img`. Select **Raspberry Pi OS with desktop** if flashing the head node's microSD card, or  **Raspberry Pi OS Lite** if flashing the worker nodes' card.
	1. Click **Select target** and chose the microSD card.
	1. Click **Flash!** <br><img src="img/fig3.png" alt="fig 3">
1. Remove and re-insert the flashed microSD card into the computer. *Must of the time Etcher will automatically eject SD cards when finished*
1. Using a text editor such as **Sublime Text**, create an empty file named `ssh` (without an extension) and save it on the microSD card. Do this for all 4 microSD cards. Alternatively, navigate to the microSD card via terminal, and use the command `touch ssh` to create the empty file.

## Configuring RPis

Next are instructions to logging into the RPis and edit some configuration files to make them work as a cluster. Accessing the RPis can be done with or without an external monitor. Follow **1. Access RPi with external monitor** if using an external monitor, or **2. Access RPi without external monitor** otherwise (most common way).

1. **Access RPi with external monitor**:
	1. Insert the microSD card into the RPi. 
	1. Connect RPi to Internet using an Ethernet cable (if the RPi comes with WiFi, this is optional).
	1. Connect external monitor through HDMI.
	1. Connect the RPi to the USB power adapter using one of the micro USB cables, and then turn the RPi on. A boot up screen should show up in the external monitor.
	1. Once booted up, some versions of **Raspberry Pi OS** will have a prompt to change password, set time zone, and update the OS. Follow these instructions, or do it manually later.

    If the RPi has started but the external monitor is not getting a signal, unplug RPi and remove its microSD card. Insert the card into a computer and navigate to its contents. Locate and open the file `config.txt`. If the following lines are commented out, uncomment them by removing the `#` in front of them (if they don't exist, add them): 
    ```
    hdmi_force_hotplug=1
    hdmi_drive=2
    ```
    Re-insert the microSD in the RPi and turn it on. If the previous modifications didn't fix the problem, check that the monitor is connected properly to the correct input, and the HDMI cable is functional.

1. **Access RPi without external monitor** (most common way): This step will require an Ethernet to USB adapter to connect the RPi via Ethernet cable to a computer using **VNC viewer**. If the computer has an Ethernet port, the adapter is not needed.
	1. Start with the RPi designated as head node, and then repeat these steps with the rest of the RPis . Insert the microSD card into the RPi (this is the card with **Raspberry Pi OS with desktop** in it). 
	1. Connect the RPi to a computer using an Ethernet cable. Use an Ethernet to USB adapter if the computer doesn't have an Ethernet port. You might also power up the RPi by connecting the micro USB power cable to one of the computer's USB ports, or into the USB power adapter.<br><img src="img/fig5.png" alt="fig 5">
	1. Be sure to turn on "Internet Sharing". 
		- On macOS: Select **System Preferences &rarr; Sharing**, and make sure "Internet Sharing" is checked and the WiFi connection is being shared with the LAN connection.
		- On Windows: *Needs instructions* 
	1. Open the **Terminal** and run the command `ping raspberrypi.local`
	1. Copy the IP address that is associated to the RPi. Press <kbd>Ctrl</kbd> + <kbd>z</kbd>  (<kbd>^</kbd> + <kbd>z</kbd> on macOS).<br><img src="img/fig6.png" alt="fig 6">
	1. Then type the command `ssh pi@192.168.2.8`, where `192.168.2.8` is the IP address you copied in the previous step.
	1. Type `Yes` and press <kbd>enter</kbd> (<kbd>return</kbd> in macOS).
	1. Enter the RPi **username** and **password**. By default the username is `pi` and the password is `raspberry`. 
		- *If working with an RPi designated as a worker node, skip next steps and proceed to:* **3. Configuring Hostnames**
	1. Head node only: Start the VNC virtual desktop in the RPi by opening a terminal and typing `vncserver`
	1. Copy the IP of the RPi: <br><img src="img/fig7.png" alt="fig 7">
	1. Open the **VNC Viewer** application installed in the computer and paste the IP in the top search bar. It should prompt you for the RPi **username** and **password**. Remember by default it should be username: `pi`, password: `raspberry`.

1. **Configuring Hostnames**: Choose a host name for your RPis carefully. Later, a scheduler called Slurm will be used to manage workload and running code in the cluster. Slurm requires nodes to be named a certain way, such as `node001` for the head node, and `node002`, `node003`, and `node004` for the worker node. 
	1. Open a **Terminal** in the RPi and type the following commands to change the host name:
		- `sudo hostname headnode001`, where `headnode001` is the host name you chose for the RPi.
		- `sudo nano /etc/hostname`, this will open the file `/etc/hostname` using the text editor **nano**. Delete everything, and enter the same host name, i.e., `headnode001` or the host name chosen for the RPi. To save press <kbd>Ctrl</kbd> + <kbd>x</kbd> (<kbd>^</kbd> + <kbd>x</kbd> on macOS), then press <kbd>Y</kbd> follow by <kbd>enter</kbd> (<kbd>return</kbd> in macOS). <br><img src="img/fig8.png" alt="fig 8">
		- `sudo nano /etc/hosts`,  and replace `raspberrypi` with your chosen host name (e.g., `headnode001` ). To save press <kbd>Ctrl</kbd> + <kbd>x</kbd> (<kbd>^</kbd> + <kbd>x</kbd> on macOS), then press <kbd>Y</kbd> follow by <kbd>enter</kbd> (<kbd>return</kbd> in macOS).<br><img src="img/fig8b.png" alt="fig 8b">

1. **RPi's OS Configuration**:
	1. On a **Terminal** in the RPi type the command `sudo raspi-config` to bring up the software configuration tool. <br><img src="img/fig9.png" alt="fig 9">	
	1. Select "Change User Password" and follow the instructions. *It is recommended (for the purpose of this guide) to make all passwords the same for each RPi*.
	1. Next, open "Localisation Options" &rarr; "Change Locale". Scroll down until the `en_US.UTF-8 UTF-8` option is highlighted and press <kbd>enter</kbd>, then <kbd>enter</kbd> again.
		- If it cannot be set by default, exit the menu and type `sudo nano /etc/locale.gen`, uncomment the line containing `en_US.UTF-8 UTF-8` (if it is commented out). Then, in the **Terminal** type `locale-gen` and try again. If that still does not work, try `sudo dpkg-reconfigure locales` and select the correct locale.
	1. Once back to the software configuration tool window, select "Localisation Options" and change the timezone.
	1. Next, choose "Advanced Options" &rarr; "Expand Filesystem" and press <kbd>enter</kbd>.
	1. Finally, select the option "Update".
	1. To activate all these changes, type `sudo reboot`. To shut down this node, the command `sudo shutdown -h now` can be used.
	1. Repeat the steps from sections **2.** to **4.** for all of your RPis, making sure to choose appropriate host names.


## Software Installation

1. Update existing packages by running the following commands. It should be perform in all the RPis, so connect to the nodes (either from the head node, or individually using a computer and **VNC Viewer**: 
	```
	sudo apt-get update
	sudo apt-get upgrade -y
	sudo reboot
	```
1. Install **MPICH**, a free, open-source implementation of the message passing interface (MPI), is one of the main components needed to for a high-performance computing (HPC) cluster because it is used for parallel programing. **MPICH** needs three directories to be created: `mpi-dl` to open the source code, `mpi` to be the installation path (“mpi”), and `mpi-build` used to build the code, as well as some other dependencies like **Gfortran**:
	```
	sudo apt install gfortran -y
	sudo apt install ntpdate -y
	sudo mkdir /opt/mpi-dl
	sudo mkdir /opt/mpi
	sudo mkdir /opt/mpi-build
	```
1. Move to the `mpi-dl` directory with the command `cd /opt/mpi-dl`
1. Download `mpich`. The next few steps might take a while:<br>
`sudo wget http://www.mpich.org/static/downloads/3.3/mpich-3.3.tar.gz`
1. Extract the downloaded file: `sudo tar zxvf mpich-3.3.tar.gz`
1. Navigate to the `mpi-build` directory: `cd /opt/mpi-build`
1. Prepare the configuration file: `sudo /opt/mpi-dl/mpich-3.3/configure --prefix=/opt/mpi`
1. Run the configuration build using: `sudo make`
1. Finish with the actual installation: `sudo make install`
 
1. Install **MPI4PY**, which will make **Python** available to communicate with the cluster.
1. Move back to the `opt` directory with the command `cd /opt/`
1. Download the needed dependencies using: 
`sudo apt install python-pip python-dev libopenmpi-dev -y`
1. Start installing **MPI4PY** with: `sudo pip install mpi4py` 
 
## Cluster Configurations

<!-- 1. Configuring **Auto Login**: these next steps will enable passwordless login between the head node and the worker nodes.
	1. On the head node, open a **Terminal** and move to the RPi directory: `cd /home/pi/`.
	1. Run `ssh-keygen -t rsa`. Make sure to press <kbd>enter</kbd> three times, accepting the default path and creating no password.
3.)	“scp /home/pi/.ssh/id_rsa.pub pi@~worker nodes IP~:/home/pi/master.pub”    (You can find your nodes IP by sshing into a node and using the command “IP a”. Locate the “eth0” section and look for the inet, stopping at the “/”)
4.)	Then, ssh into that node “ssh pi@~worker nodes IP~”
5.)	Run “ssh-keygen -t rsa” on worker node. Make sure you hit enter three times. Accepting the default path and creating no password
6.)	“cat master.pub >> .ssh/authorized_keys” on worker node
7.)	“exit” on worker node
8.)	Repeat steps 3-7 for all of your nodes.
9.)	To test if it worked, ssh into one of your worker nodes. You should not have to enter a password.



Section 5: Setting Static IPs
	Every Pi needs a static IP to allow the cluster to communicate with each other regardless what networks it connects to. The cluster only works if they are on the same network, meaning they are all plugged into the same network switcher or router. I would recommend making the IPs correlate to the hostname of the node. 

	>I tend to prefer to have a static network as it can make it easier to find and connect to your devices as upon restarting the whole system Raspberry Pi’s may change (and swap) their IP-address. The first thing to do is set it up on your host computer.
5.1: Setting Static IP
1.)	To set a static IP edit the dhcpcd.conf file. “sudo nano /etc/dhcpcd.conf”
2.)	And add these line at the bottom (you can look at the picture for a reference)
“interface eth0
static ip_address=~your chosen IP~
static routers=~your chosen router IP~ (this will be the IP of the switch)
static domain_name_servers=
static domain_search= 
noipv6”
3.)	For the non-head nodes only add
“interface eth0
static ip_address=~your chosen IP~
static routers=~the same router IP as before~”
4.)	“sudo reboot”
5.)	Repeat steps 1-5 for all of your nodes  -->
> THIS SECTION IS UNDER CONSTRUCTION :construction_worker: