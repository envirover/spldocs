---
title: SPL GroundControl
keywords: spl, ardupilot, mavlink, rockblock, satellite, telemetry, iridium, unmanned vehicle, sbd
sidebar: spl_sidebar
toc: false
permalink: splgroundcontrol.html
folder: spl
---

SPL GroundControl is a MAVLink proxy server for ArduPilot drones that uses Iridium sort burst data (ISBD) satellite communication system provided by [RockBLOCK](http://www.rock7mobile.com/products-rockblock) unit. It is designed to work with [SPL RadioRoom](https://github.com/envirover/SPLRadioRoom) field application, providing two way communication channel between ArduPilot based drones and MAVLink ground control stations such as MAVProxy, Mission Planer, or QGroundControl.

### SPLGroundControl Installation and Use

The machine that runs SPLGroundControl must be accessible from the Internet. Port 8080 must be accessible from RockBLOCK services, and port 5760 must be accessible from the ground control station client machines.

SPLGroundControl installation instructions for different environments are available on [wiki](https://github.com/envirover/SPLGroundControl/wiki) pages. Probably the easiest way to get started with SPLGroundControl is to [deploy it on Amazon AWS](https://github.com/envirover/SPLGroundControl/wiki/SPLGroundControl-Installation-on-Amazon-AWS).

Once SPLGroundControl is started, you can connect to it from MAVProxy, Mission Planer, or QGroundControl using TCP connection on port 5760. For example, MAVPoxy ground control could be connected this way: 

``mavproxy.py --master=tcp:<IP>:5760 --mav10``

Currently SPLGroundControl supports one GCS client connection at a time.




