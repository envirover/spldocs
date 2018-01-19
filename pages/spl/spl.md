---
title: SPL Global Telemetry
keywords: spl, ardupilot, mavlink, rockblock, satellite, telemetry, iridium, unmanned vehicle
sidebar: spl_sidebar
toc: false
permalink: spl.html
folder: spl
---

SPL is a global satellite telemetry system for autonomous vehicles controlled by [ArduPilot](http://ardupilot.org/) or [PX4](http://px4.io/) autopilots that  uses Iridium short burst data (ISBD) satellite communication technology provided by [Rock Seven Mobile](http://www.rock7mobile.com/products-rockblock).
     
ISBD is a high latency, low bandwidth messaging technology, yet it is relatively inexpensive compared to other global communication solutions. The required hardware is very compact and lightweight.

With SPL you can track, command, and control your solar powered boats, planes, blimps, and other autonomous vehicle from the other side of the Earth. 

SPL works with popular ground control stations (GCS) such as [Mission Planner](http://ardupilot.org/planner/), 
[QGroundControl](http://qgroundcontrol.com/), and [MAVProxy](http://ardupilot.github.io/MAVProxy/html/index.html).

![SPL System Architecture](images/spl.jpg)

The SPL software system includes:
- [SPL RadioRoom](splradioroom-rpi.html) embedded application for autopilots' companion computers that provides communication between the autopilots and ISBD transceivers.
- [SPL GroundControl](splgroundcontrol-aws.html) server handles communication between ground control stations and Rock7Core web services.
- [SPL Stream](spltracks-aws.html) web service stores telemetry data reported by SPL RadioRoom in DynamoDB database.
- [SPL Tracks](spltracks-aws.html) web service provides access to the stored data in [GeoJSON format](https://tools.ietf.org/html/rfc7946).



