---
title: SPL Operation Instructions
keywords: spl, ardupilot, mavlink, rockblock, satellite, telemetry, iridium
sidebar: home_sidebar
toc: false
permalink: sploperation.html
folder: spl
summary: This page describes guidelines for operating SPL global telemetry after it was set up.
---


## Connecting to SPL GroundControl

Launch QGroundControl, go to “Comm Links” application settings, and create a new comm link configuration of TCP type using the IP address of SPL GroundControl server and 5760 port.   

Connect to the created comm link.

## Testing the Comm Link

To test the comm link it is recommended to arm and disarm the vehicle using SPL connection and check the vehicle status and position reported by the vehicle.

## Setting the Report Period

As soon as UV Radio Room starts it retrieves and reports the vehicle’s state using HIGH_LATENCY MAVLink message. Each report is 48 bytes long (consumes 1 RockBLOCK credit) and includes the following information:
* Autopilot system mode, 
* position (latitude, longitude, and altitude AMSL),
* attitude (roll, pitch, and heading),
* ground speed and climb rate,
* heading and attitude setpoints,
* airspeed,
* current waypoint number,
* distance to target,
* GPS fix type and number of satellites visible,
* remaining battery percentage,
* throttle percentage.

UV Radio Room sends this report periodically. The default report period is defined by the UV Radio Room configuration properties. The report period can be changed in flight by setting HL_REPORT_PERIOD on-board parameter.  The parameter value is specified in seconds.

The report is also sent immediately after UV Radio Room receives and handles commands or missions.

## Setting On-board Parameters in-flight

SPL supports changing on-board autopilot parameters in-flight. Please check the autopilot manual for the list of parameters that can be changed in-flight. 

When an on-board parameter value is changed by a ground control station client, SPL GroundControl immediately confirms the parameter change to the client, but the actual change of the parameter value in the vehicle might take a few minutes. Once the parameter value is changed on the vehicle, UV Radio Room confirms the change to SPL GroundControl, and SPL GroundControl updates the parameter value in the reported state of the vehicle’s “shadow”. The reported state is returned to the ground control station clients when they read the parameters.

## Uploading Missions

SPL supports uploading missions to vehicles. Uploading fences and rally points is not currently supported by SPL.  

Each waypoint in the mission is sent to the vehicle in a separate MAVLink message. In addition one message is sent with the total number of uploaded mission items and one message is sent back from the vehicle to acknowledge that the mission was accepted by the vehicle. 

Iridium SBD is capable of sending and receiving about 1 message per minute. Though SPL does not limit the number of items in a mission, sending very large missions is not recommended.

SPL GroundControl immediately acknowledges an uploaded mission, however sending the mission to the vehicle over Iridium SBD may take a long time, depending on the number of waypoints in the mission and quality of the satellite link. As soon as UV Radio Room receives all the mission items is writes the mission to the autopilot and sends the acknowledgement back to SPL GroundControl. After receiving the acknowledgement, SPL GroundControl updates the mission items in the reported state of the vehicle’s “shadow”. The reported state is returned to the ground control station clients when they read the missions. UV Radio Room suspends all other operations while it handles the missions. 

## Using SPL With Other Comm Links

When SPL is used with other comm links such as radio telemetry, make sure that the GCS is not connected to the vehicle using both of the comm links at the same time. For QGroundControl GCS it is recommended to turn off AutoConnect in the general application settings.

SPL GroundControl stores in the vehicle's "shadow" uploaded missions and updated parameter values if they were sent to the vehicle through the SPL comm link. If missions or parameters were changed using other comm links, to keep the shadow in sync with the vehicle's state it is recommended to update the missions and parameters in the shadow by connecting GCS to a special "shadow" port 5757.

To update missions or parameters in the shadow:
- Connect GCS to TCP port 5757 of SPL GroundControl,
- Upload the missions,
- Disconnect from  TCP port 5757,
- Connect GCS to port 5760,
- Download the missions.


## Troubleshooting the Comm Link

SPL GroundControl logs errors and all messages sent to and received from the satellite communication link. The log file could be used to obtain detailed information about the satellite communication. 