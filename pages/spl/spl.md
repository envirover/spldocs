---
title: SPL Global Telemetry
keywords: spl, ardupilot, mavlink, rockblock, satellite, telemetry, iridium, unmanned vehicle, sbd
sidebar: spl_sidebar
toc: false
permalink: spl.html
folder: spl
---

SPL is a global satellite telemetry system for autonomous vehicles controlled by [ArduPilot](http://ardupilot.org/) or [px4](http://px4.io/) autopilots.

With SPL you can:
- Track position, attitude, and velocity of your vehicles anywhere on Earth.
- Monitor vital signs of your vehicles, such as battery charge, system status, and temperature.
- Update on-board parameters and send commands and missions to your vehicles.
- Control gymbals, and RC servos connected to autopilots.

Essentially, with SPL you could control your unmanned vehicle on the other side of the Earth almost the same way you would with radio telemetry. SPL was designed to work with popular ground control stations such as Mission Planner, QGrouindControl, and MAVProxy.

![SPL System Architecture](images/spl.jpg)
     
SPL uses Iridium short burst data (SBD) satellite communication technology provided by [Rock Seven Mobile](http://www.rock7mobile.com/). 

Iridium SBD is a high latency, low bandwidth messaging technology, yet it is relatively inexpensive compared to other global communication solutions. The required hardware is very compact and lightweight.

The SPL software suite includes firmware for autopilot companion computer called SPL RadioRoom and a web service application called SPL GroundControl, which serves as a proxy between ground control stations such as Mission Planer or QGroundControl and Rock7Core web services. Optional SPL Stream and SPL Tracks web services provide a solution for storing and visualizing data reported by SPL RadioRoom.

## SPL RadioRoom

SPL Radio Room requires the following hardware components:
- [RockBLOCK Mk2](http://www.rock7mobile.com/products-rockblock) Iridium satellite communication module>
- Raspberry Pi single board computer
- [ArduPilot-based autopilot](http://ardupilot.org/dev/docs/supported-autopilot-controller-boards.html), such as Pixhawk

RockBLOCK MK2 naked communication module costs around 250$. RockBLOCK line rental is about $13 per month. Arduino 101 is around $30.

By default SPL Radio Room reports every 5 minutes. Each report costs 1 RockBLOCK credit. The cost of credits varies from $0.05 to $0.14 depending on the amount of credits purchased. 

## SPL GroundControl

SPL GroundControl is a MAVLink proxy server for ArduPilot drones that uses Iridium sort burst data (ISBD) satellite communication system provided by [RockBLOCK](http://www.rock7mobile.com/products-rockblock) unit. It is designed to work with [SPL RadioRoom](https://github.com/envirover/SPLRadioRoom) field application, providing two way communication channel between ArduPilot based drones and MAVLink ground control stations such as MAVProxy, Mission Planer, or QGroundControl.

## SPL Stream and SPL Tracks

Optional SPL Stream and SPL Tracks web services provide a solution for storing and visualizing data reported by SPL RadioRoom.

SPL Stream web service saves messages received from SPL RadioRoom to AWS DynamoDB database service.

SPL Tracks provides access to the reported data in GeoJSON format. 

[SPL Tracks GeoJSON web service demo](http://spldemo.envirover.com/spltracks/features/?devices=300234064280890&startTime=1499736149000&endTime=1499742468000)

[SPL Tracks web map demo](http://spldemo.envirover.com/spltracks/?devices=300234064280890&startTime=1499736149000&endTime=1499742468000)

