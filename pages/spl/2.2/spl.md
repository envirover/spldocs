---
title: SPL Global Telemetry
keywords: spl, ardupilot, mavlink, rockblock, satellite, telemetry, iridium, unmanned vehicle
sidebar: home_sidebar
toc: false
permalink: 2.2/spl.html
folder: spl
---

SPL is a global satellite telemetry system for autonomous vehicles controlled by [ArduPilot](http://ardupilot.org/) or [PX4](https://px4.io/) autopilots. With SPL you can track, command, and control your solar powered boats, planes, blimps, and other autonomous vehicle from the other side of the Earth.

SPL supports TCP/IP Internet connections and Iridium short burst data (ISBD) satellite communication technology provided by [Rock Seven Mobile](http://www.rock7mobile.com/products-rockblock).

ISBD is a high latency, low bandwidth messaging technology, yet it is relatively inexpensive compared to other global communication solutions. The required hardware is very compact and lightweight.

SPL works with popular ground control stations (GCS) such as [Mission Planner](http://ardupilot.org/planner/) and
[QGroundControl](http://qgroundcontrol.com/).

![SPL System Architecture](images/spl.jpg)

The SPL software system includes:
- [UV Radio Room](radioroom.html) firmware for autopilot's companion computer that brokers communication between the autopilot, the Internet modem and ISBD transceiver.
- [UV Hub](uvhub.html) proxy server brokers communication between the ground control stations and the vehicle comm link channels.
- [UV Shadow](uvshadow.html) database that stores message logs, desired and reported states of the vehicle.
- [UV Tracks](uvtracks.html) web service provides access to the data stored in the vehicle shadow by UV Hub.
- [UV Cockpit](uvcockpit.html) web app suite that visualizes data retrieved from UV Tracks web service.
