---
title: UV Radio Room 
keywords: spl, ardupilot, px4, mavlink, rockblock, satellite, telemetry, iridium, radio room, isbd
sidebar: home_sidebar_21
toc: false
permalink: 2.1/radioroom.html
folder: spl
---

UV Radio Room is an application for companion computer of MAVLink-based autopilots such as ArduPilot or PX4 that brokers communication between the autopilot and an Internet modem or Iridium Short Burst Data (ISBD) transceiver. Together with [UV Hub](uvhub.html) server it provides two-way communication link between unmanned vehicles and ground control stations such as QGroundControl or Mission Planer.

UV Radio Room not only forwards messages from the autopilot to the communication channels, it also filters, compresses, and buffers the messages when required.

UV Radio Room can be configured to work with just one communication channel or with two channels in fail-over mode. When both channels are enabled, the channel with the smallest report period is called <i>primary</i> and the other channel is called <i>secondary</i>. 

UV Radio Room tries to send mobile-originated messages using the primary channel first. The secondary channel is used when the message cannot be sent over the primary channel during the reporting period of the secondary channel. For example, if the primary channel is a cellular internet connection with report period of one second, and the secondary channel is RockBLOCK transceiver with report period of sixty seconds, Radio Room will try to send reports every second over the cellular link, and will switch to ISBD when sixty seconds elapsed after the last successful report. UV Radio Room always checks both channels for mobile-terminated messages.

Regular radio-telemetry communication between MAVLink-based autopilots and CGSs involves multiple MAVLink messages sent several times per second both ways. This protocol is too chatty for slow and expensive ISBD channel. Radio Room compresses attributes of multiple messages into [HIGH_LATENCY](https://mavlink.io/en/messages/common.html#HIGH_LATENCY) MAVLink message appropriate for high latency connections like Iridium SBD. The HIGH_LATENCY messages periodically are sent to the communication channels.

When GCS client sends mission items to UV Hub, first the all the items are buffered in the desired state of the UV Shadow and then UV Hub sends them to UV Radio Room, one mission item per message. Radio Room buffers all the mission items first and then sends them to the autopilot in a quick succession. The mission acknowledgment from the autopilot is sent back to the GCS. 

UV Radio Room can run on almost any Linux computer. Envirover provides UV Radio Room [installation packages](https://github.com/envirover/SPLRadioRoom/releases) and [instructions for Raspberry Pi](splradioroom-rpi.html) only. 