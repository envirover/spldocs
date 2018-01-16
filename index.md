---
title: "BLOS Telemetry Systems"
keywords: blos, satellite, telemetry, unmanned vehicles
tags: [getting_started]
sidebar: home_sidebar
permalink: index.html
summary: BLOS Telemetry Systems from Envirover.
---

SPL and NVI are global satellite telemetry solution for unmanned vehicles controlled by [ArduPilot](http://ardupilot.org/) or [PX4](http://px4.io/) autopilot. 

## SPL Satellite Telemetry

[SPL](spl.htmp) uses Iridium short burst data (SBD) satellite communication technology provided by [Rock Seven Mobile](http://www.rock7mobile.com/). 

With SPL you can:
- Track position, attitude, and velocity of your vehicles anywhere on Earth.
- Monitor vital signs of your vehicles, such as battery charge, system status, and temperature.
- Update on-board parameters and send commands and missions to your vehicles.
- Control gymbals, and RC servos connected to autopilots.

Essentially, with SPL you could control your unmanned vehicle on the other side of the Earth almost the same way you would with radio telemetry. SPL was designed to work with popular ground control stations such as Mission Planner, QGrouindControl, and MAVProxy.

Because Iridium SBD is very high latency and low bandwidth, SPL does not support FPV video streaming. 

## NVI Satellite Telemetry 

[NVI](nvi.html) is designed for satellite or cellular communication over TCP/IP protocol.