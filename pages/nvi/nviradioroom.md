---
title: NVI RadioRoom
keywords: nvi, ardupilot, mavlink, satellite, telemetry, unmanned vehicle
sidebar: nvi_sidebar
toc: false
permalink: nviradioroom.html
folder: nvi
---

NVI RadioRoom is a firmware for a companion computer of MAVLink-based autopilots such as ArduPilot that provides telemetry over satellite or cellular TCP/IP connections. Together with NVI GroundControl TCP/IP server it provides a two-way communication solution between the autopilots and ground control stations such as QGroundControl or Mission Planer.

## System Requirements

NVI RadioRoom system requires the following hardware and software:
* Autopilot such as Pixhawk with [ArduPilot](http://ardupilot.org/);
* Raspberry Pi computer with [Raspbian Stretch](https://www.raspberrypi.org/downloads/raspbian/) Desktop or Light;
* Satellite or cellular modem.

## Wiring 

NVI RadioRoom uses serial devices to communicate with the autopilot. The safest bet is to use USB to TTL UART serial converter to connect both autopilot and transceiver to the Raspberry Pi's USB ports. For testing it's also possible to connect the autopilot to Raspberry Pi by connecting micro USB port on the autopilot to an USB port on Raspberry Pi, though such connection is not recommended in flight. 

## Installing

To install NVI RadioRoom on Raspberry Pi:

1. Copy nviradioroom-1.0.0-raspbian.deb from [https://s3-us-west-2.amazonaws.com/envirover/nvi/shared/NVIRadioRoom/nviradioroom-1.0.0-raspbian.deb](https://s3-us-west-2.amazonaws.com/envirover/nvi/shared/NVIRadioRoom/nviradioroom-1.0.0-raspbian.deb) to the Raspberry Pi. 
2. Install nviradioroom-1.0.0-raspbian.deb package.

   ``$ sudo dpkg -i nviradioroom-1.0.0-raspbian.deb``
    
3. Configure the reporting period and GCS host IP address in /etc/nviradioroom.conf file. 
4. Make sure that NVI GroundControl service is up an running at the host with the specified IP address.
5. Enable and start nviradioroom service.

```
$ sudo systemctl enable nviradioroom.service
$ sudo systemctl start nviradioroom.service
```
   
By default the serial device for autopilot is set to /dev/ttyUSB0. If auto_detect_serials property is set to true, NVI RadioRoom can autodetect autopilot if it is available on other serial and USB devices. To make nviradioroom servicestartup faster and more reliable it is recommended to set the device paths correctly. 

USB device paths /dev/ttyUSB0, /dev/ttyUSB1, ... can change after reboot if uther USB devices are connected. For USB devices it is recommended to use symlinks from /dev/serial/by-path or /dev/serial/by-path directories, that do not change with reboots. 

RadioRoom periodically reports the vehicle's position, attitude, velocity, and other data using HIGH_LATENCY MAVLink message. The message size is 48 bytes. The reporting period default value is 10 seconds. It can be changed by setting report_period configuration property in /etc/nviradioroom.conf, or remotely at runtime by setting HL_REPORT_PERIOD on-board parameter.

Raspberry Pi require an orderly shutdown procedure, otherwise the SD card may become corrupted and the system will no longer boot. To prevent the SD card corruption during power cuts it is recommended to [configure Raspbian to work in a read-only mode](https://learn.adafruit.com/read-only-raspberry-pi/). Alternatively, UPS and a shutdown circuit could be used to orderly shutdown Raspberry Pi after power cuts.
  
## Troubleshooting

Run ``$ sudo systemctl status nviradioroom.service`` to check the status of nviradioroom service.

If radioroom is properly wired and configured, the output should look like this:

```
● nviradioroom.service - NVI RadioRoom Service
   Loaded: loaded (/etc/systemd/system/nviradioroom.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2018-01-08 19:50:46 PST; 4s ago
     Docs: http://github.com/envirover/NVIRadioRoom
 Main PID: 1051 (nviradioroom)
   CGroup: /system.slice/nviradioroom.service
           └─1051 /usr/sbin/nviradioroom

Jan 08 19:50:46 raspberrypi systemd[1]: Started NVI RadioRoom Service.
Jan 08 19:50:46 raspberrypi nviradioroom[1051]: Starting NVI RadioRoom 1.0.0....
Jan 08 19:50:46 raspberrypi nviradioroom[1051]: Connecting to autopilot (/dev/ttyUSB0 57600)...
Jan 08 19:50:47 raspberrypi nviradioroom[1051]: Autopilot detected at serial device '/dev/ttyUSB0'.
Jan 08 19:50:47 raspberrypi nviradioroom[1051]: MAV type: 12, system id: 1, autopilot class: 3, firmware version: 3.5.2/Jan 08 19:50:47 raspberrypi nviradioroom[1051]: Connected to 'tcp://10.0.0.1:5060'.
Jan 08 19:50:47 raspberrypi nviradioroom[1051]: NVI RadioRoom 1.0.0. started.
```

Log file of nviradioroom service is available at /var/log/nviradioroom.log.

Add -v option to the radioroom command line in /etc/systemd/system/nviradioroom.sevice to enable verbose logging. Verbose logging makes nviradioroom service log all MAVLink messages sent and received from autopilot and GCS service.


## Licensing
```
Envirover confidential

[2018] Envirover
All Rights Reserved.

NOTICE:  All information contained herein is, and remains the property of
Envirover and its suppliers, if any.  The intellectual and technical concepts
contained herein are proprietary to Envirover and its suppliers and may be
covered by U.S. and Foreign Patents, patents in process, and are protected
by trade secret or copyright law.

Dissemination of this information or reproduction of this material
is strictly forbidden unless prior written permission is obtained
from Envirover.
```