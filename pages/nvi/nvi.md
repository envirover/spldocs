---
title: NVI BLOS Telemetry
keywords: nvi, blos, ardupilot, px4, mavlink, satellite, telemetry, autonomou vehicle
sidebar: nvi_sidebar
toc: false
permalink: nvi.html
folder: nvi
---

NVI BLOS telemetry is designed to work with satellite or cellular modems such as
[Iridium GO!](https://www.iridium.com/products/details/iridiumgo).

NVI provides peer-to-peer communication between an autonomous vehicle with 
[ArduPilot](http://ardupilot.org/) or [PX4](http://px4.io/) autopilot and 
[Mission Planner](http://ardupilot.org/planner/) or 
[QGroundControl](http://qgroundcontrol.com/) ground control station.

![NVI System Architecture](images/nvi.jpg)

The NVI software suite includes firmware for the autopilot companion computer [NVI RadioRoom](nviradioroom.html) and  
a TCP/IP server application [NVI GroundControl](nvigroundcontrol.html). 

NVI not simply transmits messages between autopilot and ground control stations, 
it also filters and aggregates data to support communication over high latency and low bandwidth networks.