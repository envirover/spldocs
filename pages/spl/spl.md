---
title: SPL Satellite Telemetry
keywords: spl, ardupilot, mavlink, rockblock, satellite, telemetry, iridium, unmanned vehicle, sbd
sidebar: spl_sidebar
toc: false
permalink: spl.html
folder: spl
---

SPL is a global satellite telemetry solution for unmanned vehicles controlled by [ArduPilot](http://ardupilot.org/).

With SPL you can:
- Track position, attitude, and velocity of your vehicles anywhere on Earth.
- Monitor vital signs of your vehicles, such as battery charge, system status, and temperature.
- Update on-board parameters and send commands and missions to your vehicles.
- Control gymbals, and RC servos connected to autopilots.

SPL can also be used together with radio telemetry as a long range backup channel to track and control vehicles if they leave radio channel range.

Essentially, with SPL you could control your unmanned vehicle on the other side of the Earth almost the same way you would with radio telemetry. SPL was designed to work with popular ground control stations such as Mission Planner, QGrouindControl, and MAVProxy.

Not only does SPL transmit messages between autopilot and ground control stations, it also filters messages and aggregates data to adapt MAVLink protocol for high latency asynchronous SBD communication.

![SPL System Architecture](images/spl.jpg)
     
SPL uses Iridium short burst data (SBD) satellite communication technology provided by [Rock Seven Mobile](http://www.rock7mobile.com/). 

Iridium SBD is a high latency, low bandwidth messaging technology, yet it is relatively inexpensive compared to other global communication solutions. The required hardware is very compact and lightweight.

The SPL software suite consists of firmware for ArduPilot companion computer called [SPL RadioRoom](radioroom.html) and a web service application called [SPL GroundControl](gGroundcontrol.html), which serves as a proxy between ground control stations such as Mission Planer or QGroundControl and Rock7Core web services.

SPL Radio Room and SPL Ground Control are open source software.
     
SPL Radio Room requires the following hardware components:
- [RockBLOCK Mk2](http://www.rock7mobile.com/products-rockblock) Iridium satellite communication module>
- Raspberry Pi single board computer
- [ArduPilot-based autopilot](http://ardupilot.org/dev/docs/supported-autopilot-controller-boards.html), such as Pixhawk

RockBLOCK MK2 naked communication module costs around 250$. RockBLOCK line rental is about $13 per month. Arduino 101 is around $30.

By default SPL Radio Room reports every 5 minutes. Each report costs 1 RockBLOCK credit. The cost of credits varies from $0.05 to $0.14 depending on the amount of credits purchased. 

SPL GroundControl requires a computer accessible over the Internet. Running SPL Ground Control on Amazon AWS</a> is probably the easiest way to get started with SPL.

Optional SPL Stream and SPL Tracks web services could be used to store and visualize the vehicle tracks.

