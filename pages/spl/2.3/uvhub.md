---
title: UV Hub
keywords: spl, mavlink, rockblock, satellite, telemetry, isbd, ArduPilot, PX4, Rock7Core
sidebar: home_sidebar_23
toc: false
permalink: 2.3/uvhub.html
folder: spl
---

UV Hub is a server application that brokers communication with unmanned vehicles controlled by ArduPilot or PX4 autopilots over the Internet and/or RockBLOCK/ISBD communication channels. It is designed to work with <a href="http://envirover.com/docs/radioroom.html">UV Radio Room</a> companion computer, providing two-way communication between the autopilots and ground control stations such as Mission Planer or QGroundControl.

UV Hub not only forwards messages from the autopilot to the communication channels, it also filters, queues, and buffers the messages when required.

When UV Hub sends mobile-terminated messages to UV Radio Room it uses the Internet communication channel if there is a live TCP/IP connection from UV Radio Room. If there is no TCP/IP connection from UV Radio Room and ISBD channel is enabled, UV Hub uses the ISBD channel, sending the message to Rock7Core web service. UVHub needs the RockBLOCK transceiver IMEI number, Rock7Core user name and password to send messages to RockBLOCK. The ISBD channel is disabled is the IMEI number is not specified in the UV Hub configuration properties. Mobile-originated messages are received from both enabled channels.

UV Hub stores mobile-originated messages, on-board parameters, and mission plans in a database called _UV Shadow_. [UV Tracks](uvtracks.html) web service provides read access to the information stored in the shadow.

## UV Hub Ports

| Port | Description                                                              |
|------|--------------------------------------------------------------------------|
| 5080 | HTTP port used by Rock7Core services to send mobile-originated messages  |
| 5060 | TCP port used by connections from UV Radio Room                          |
| 5760 | TCP port used for MAVLink ground control stations connections            |
| 5757 | TCP port used to update reported parameters and missions in the shadow   |
