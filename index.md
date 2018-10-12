---
title: "What's New "
keywords: spl, blos, satellite, telemetry, unmanned vehicles
tags: [getting_started]
sidebar: home_sidebar
toc: false
summary: Version 2.1 of SPL global telemetry system is now available.
permalink: index.html
---

SPL 2.1 includes the following new features:
- Supports both Iridium SBD and the Internet comm links. The system can be configured to use only one channel, or two channels in fail-over mode. For example, slower and more expensive ISBD channel could be used only when WiFi or cellular Internet connection is not available.
- Supports persistent shadow that stores reported states, mission plans, and on-board parameters of the vehicles. A web service provides read access to the shadow's data.
- Includes a new 3D vehicle tracking sample web application that shows both vehicle tracks and missions.
- Simplifies the server software deployment.

In SPL 2.1 SPL GroundControl and SPL Stream servers were replaced by [UV Hub](uvhub.html) server. SPL Tracks server was redesigned and renamed to [UV Tracks](uvtracks.html).