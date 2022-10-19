# How to Build a Low-Cost HPC Cluster

> This guide is the result of an independent study performed by [Nathan H. Silverman](https://www.linkedin.com/in/nathan-silverman-b35767173/), a  [Kalamazoo College](https://www.kzoo.edu)'s student in Computer Science, under the supervision of Sandino Vargas-Pérez. 
>
> The guide presents the list of components needed to create a low-cost Bewolf-type cluster, helpful pictures, instructions for assembling hardware, software installation, commands to execute proper configuration, and more.
>
> For questions, please contact Dr. Vargas-Pérez via [E-mail](mailto:sandino.vargasperez@kzoo.edu).

## About the Guide

The guide was designed to build a cluster using 4 Raspberry Pi (RPi) computers, but it can be used to build a cluster of any size. The step-by-step instructions will detail how to assemble hardware, configure software, and the setup needed to have a fully functioning RPi cluster. Raspberry Pi 3 B+s were used for this project, although with a few tweaks, any modern RPi should work.

### Materials

- 4 Raspberry Pi computers
- 4 MicroSD cards (32 or 64 GB w/adapter)
- 5 Ethernet cable (cat 5e or 6, 1 foot in length or more)
- 4 Micro USB Cords
- 1 Ethernet switch (with 5 ports or more)
- 1 USB power adapter (with 5 ports or more, 2.4 amps per port)
- 1 Stackable acrylic case for RPis
	- Make sure this item includes 4 Layers and heatsinks, and it is compatible with the RPi model chosen
- Recommended tools:
	- Tweezers (Optional)
	- Screw driver
	- Monitor (or Ethernet to USB adapter to connect via VNC)

## Section 1: Hardware Assembly

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

1. Stack:
	1. Put the screw end of the nub through the plate into the open end of the leg to keep it in place. Repeat for all four of the first layers: **nub &rarr; plate &rarr; leg**
	1. Place the next plate on top of the screw end of the leg: **leg &rarr; plate &rarr; leg**
	1. Place the final plate on legs then and secure it with the silver nut on top: **leg &rarr; plate w/o Pi &rarr; silver nut**

