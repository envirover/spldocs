---
title: "What's New"
keywords: spl, blos, satellite, telemetry, unmanned vehicles
tags: [getting_started]
sidebar: home_sidebar_24
toc: false
summary: Version 2.4 of SPL global telemetry system is now available.
permalink: 2.4/index.html
---

SPL 2.4 highlights:

- UV Radio Room now supports execution of configurable Linux shell commands (camera handlers) when camera control MAVLink commands DO_DIGICAM_CONTROL, IMAGE_START_CAPTURE, IMAGE_STOP_CAPTURE, VIDEO_START_CAPTURE, or VIDEO_STOP_CAPTURE are received from the comm channels, or when mission items associated with these MAVLink commands are reached. The camera handlers allow using the companion computers to take pictures and save them to the local disk or send the pictures to a server machine.

- UV Radio Room is now supported on [NVIDIA Jetson](https://developer.nvidia.com/buy-jetson) - a small, powerful computer that lets you run accelerated image and video processing, as well as use neural networks for applications like image classification and object detection.

- [UV Hub](https://github.com/envirover/UVHub) and [UV Cockpit](https://github.com/envirover/UVCockpit) are now open source code licensed under Apache 2.0 license.

- Fixed log files rotation issues across all the system components.
