---
title: UV Radio Room 
keywords: spl, ardupilot, px4, mavlink, rockblock, satellite, telemetry, iridium, radio room, isbd
sidebar: home_sidebar
toc: false
permalink: radioroom.html
folder: spl
---

UV Radio Room is an application for companion computer of MAVLink-based autopilots such as ArduPilot or PX4 that brokers communication between the autopilot and an Internet modem or Iridium Short Burst Data (ISBD) transceiver. Together with [UV Hub](uvhub.html) server it provides two-way communication link between unmanned vehicles and ground control stations such as QGroundControl or Mission Planer.

UV Radio Room not only forwards messages from the autopilot to the communication channels, it also filters, compresses, and buffers the messages when required.

UV Radio Room can be configured to work with just one communication channel or with two channels in fail-over mode. When both channels are enabled, the channel with the smallest report period is called _primary_ and the other channel is called _secondary_.

UV Radio Room tries to send mobile-originated messages using the primary channel first. The secondary channel is used when the message cannot be sent over the primary channel during the reporting period of the secondary channel. For example, if the primary channel is a cellular internet connection with report period of one second, and the secondary channel is RockBLOCK transceiver with report period of sixty seconds, Radio Room will try to send reports every second over the cellular link, and will switch to ISBD when sixty seconds elapsed after the last successful report. UV Radio Room always checks both channels for mobile-terminated messages.

Regular radio-telemetry communication between MAVLink-based autopilots and CGSs involves multiple MAVLink messages sent several times per second both ways. This protocol is too chatty for slow and expensive ISBD channel. Radio Room compresses attributes of multiple messages into [HIGH_LATENCY](https://mavlink.io/en/messages/common.html#HIGH_LATENCY) MAVLink message appropriate for high latency connections like Iridium SBD. The HIGH_LATENCY messages periodically are sent to the communication channels.

When GCS client sends mission items to UV Hub, first the all the items are buffered in the desired state of the UV Shadow and then UV Hub sends them to UV Radio Room, one mission item per message. Radio Room buffers all the mission items first and then sends them to the autopilot in a quick succession. The mission acknowledgment from the autopilot is sent back to the GCS.

UV Radio Room also supports execution of configurable Linux shell commands (camera handlers) when camera control MAVLink commands DO_DIGICAM_CONTROL, IMAGE_START_CAPTURE, IMAGE_STOP_CAPTURE, VIDEO_START_CAPTURE, or VIDEO_STOP_CAPTURE are received from the comm channels, or when mission items associated with these MAVLink commands are reached. The camera handlers allow using the companion computers to take pictures and save them to the local disk or send the pictures to a server machine.

UV Radio Room is supported on Raspberry Pi and NVIDIA Jetson platforms, but it can run on almost any Linux computer. See [UV Radio Room installation](splradioroom-rpi.html) for the specific installation instruction.
