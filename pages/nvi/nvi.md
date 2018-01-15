---
title: NVI
keywords: nvi, ardupilot, mavlink, satellite, telemetry, unmanned vehicle
sidebar: nvi_sidebar
toc: false
permalink: nvi.html
folder: nvi
---

NVI is a global satellite telemetry solution for unmanned vehicles controlled by [ArduPilot](http://ardupilot.org/)

With NVI you can:
- Track position, attitude, and velocity of your vehicles anywhere on Earth.
- Monitor vital signs of your vehicles, such as battery charge, system status, and temperature.
- Update on-board parameters and send commands and missions to your vehicles.
- Control gymbals, and RC servos connected to autopilots.

NVI can also be used together with radio telemetry as a long range backup channel to track and control vehicles if they leave radio channel range.

Essentially, with SPL you could control your unmanned vehicle on the other side of the Earth almost the same way you would with radio telemetry. NVI was designed to work with popular ground control stations such as Mission Planner, QGrouindControl, and MAVProxy.

Not only does SPL transmit messages between autopilot and ground control stations, it also filters messages and aggregates data to adapt MAVLink protocol for high latency asynchronous communication.