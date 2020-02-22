---
title: "What's New"
keywords: spl, blos, satellite, telemetry, unmanned vehicles
tags: [getting_started]
sidebar: home_sidebar
toc: false
summary: Version 2.3 of SPL global telemetry system is now available.
permalink: index.html
---

In version 2.3 SPL was updated under the hood to support low latency TCP/IP communication:

- UV Radio Room was rewritten to use multiple threads to send/receive messages to/from autopilot and comm channels independently in parallel. That made it possible to make UV Radio Room more responsive and to support reliable failover of the channels.  
- Elasticsearch implementation of UV Shadow was replaced by a fast and slim implementation that uses MondoDB database.
- To support frequent state reports, UV Shadow now has separate state and logs storage subsystems. The state is updated with on each report received from the vehicle, and the state is stored in the logs periodically with period defined by HL_REPORT_PERIOD on-board parameter.
- UV Hub now sends heartbeats and other state updates to GCS two times per second instead of once.
- UV Tracks web service V2 API now supports retrieving the last reported state, on-board parameters. The vehicle tracks and missions can now be requested as both point and line GeoJSON features.
- UV Cockpit web apps were updated to use UV Tracks V2 API.
