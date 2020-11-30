---
title: UV Cockpit 
keywords: spl, mavlink, uv shadow, rest, api, service
sidebar: home_sidebar_23
toc: false
permalink: 2.3/uvcockpit.html
folder: spl
---

UV Cockpit is a suite of in-browser web applications that visualize data received from [UV Tracks](uvtracks.html) web service.

![UV Tracks 3D](images/uvtracks3d.jpg)

UV Tracks 3D demo application demonstrates use of UV Tracks web service for tracking aerial vehicles on a 3D map.

The vehicle track displayed by the web applications can be changed by HTTP query string parameters.

|Parameter|Data Type|Required|Description|
|---------|---------|--------|-----------|
|startTime|Integer|No|Start of time range query as UNIX epoch time.|
|endTime|Integer|No|End of time range query as UNIX epoch time.|
|top|Integer|No|Maximum number of state reports. The default value is 100.|