---
title: UV Radio Room Installation 
keywords: spl, ardupilot, mavlink, rockblock, satellite, telemetry, iridium, raspberry pi, nvidia jetson
sidebar: home_sidebar
toc: false
permalink: splradioroom-rpi.html
folder: spl
---

UV Radio Room is an application for companion computer of MAVLink-based autopilots such as ArduPilot or PX4 that brokers communication between the autopilot and an Internet modem or Iridium Short Burst Data (ISBD) transceiver. Together with [UV Hub](http://envirover.com/docs/uvhub.html) server it provides two-way communication link between unmanned vehicles and ground control stations such as QGroundControl or Mission Planer.

## System Requirements

UV RadioRoom system requires the following hardware and software:

* Autopilot such as Pixhawk with [ArduPilot](http://ardupilot.org/) or [PX4](https://px4.io/) firmware;
* Raspberry Pi or NVIDIA Nano computer;
* Activated [RockBLOCK Mk2](http://www.rock7mobile.com/products-rockblock) or [RockBLOCK 9603](http://www.rock7mobile.com/products-rockblock-9603) Iridium satellite communication module with FTDI USB to UART cable.
* Cellular or satellite Internet modem.

See [rpi-radioroom.md](https://github.com/envirover/SPLRadioRoom/blob/master/hardware/rpi-radioroom.md) for UV Radio Room hardware parts and STL files for a 3D-printed enclosure.

## Wiring

UV Radio Room uses serial devices to communicate with autopilot and ISBD transceiver. Cellular or satellite Internet modems could be connected using USB, Ethernet, or Wi-Fi connections.

![UV Radio Room Wiring](images/radioroom-wiring.jpg)

See the instructions on connecting companion computer to Pixhawk running ArduPilot and PX4 autopilots:

* [ArduPilot Connecting the Pixhawk and RPi](http://ardupilot.org/dev/docs/raspberry-pi-via-mavlink.html)
* [PX4 Companion Computer for Pixhawk class](https://dev.px4.io/en/companion_computer/pixhawk_companion.html)

The safest bet is to use USB to TTL UART serial converter to connect both autopilot and transceiver to the computer's USB ports. Another, less straightforward option is to connect autopilot to the Raspberry Pi serial port using GPIO pins.

It is recommended to connect RockBLOCK module using [FTDI USB to UART cable](https://www.rock7.com/shop-product-detail?productId=16) provided by Rock Seven Mobile, or [short FTDI USB serial cable for RockBLOCK 9603](https://www.amazon.com/Short-FTDI-Serial-Cable-RockBLOCK/dp/B08CXMQ1Y9) from Envirover. 3.3.V FTDI cables or chips from other vendors can also be used, but MaxPower field in the chip must be set to 500 mA to satisfy the power requirements of RockBLOCK (The default setting for MaxPower field is typically 100 mA). Alternatively, RockBLOCK could be powered by directly, and not through the FTDI chip.

Connect the cellular or satellite Internet modem to Raspberry Pi as recommended by the modem manufacturer. The modems are not shown on the diagram.

UV Radio Room 2.5 supports version 2.0 of MAVLink protocol.

![Radio Room with Raspberry Pi Zero W, RockBLOCK 9603, and Hologram Nova modem](images/radioroom.jpg)

_Radio Room setup with Raspberry Pi 3, RockBLOCK 9603, and Sixfab 4G/LTE cellular modem_

## Installing

To install Radio Room on Raspberry Pi or NVIDIA Jetson:

1. Copy [radioroom-2.5.0-raspbian.deb](https://envirover.s3-us-west-2.amazonaws.com/spl/2.5.0/radioroom-2.5.0-raspbian.deb) to the Raspberry Pi or [radioroom-2.5.0-jetson.deb](https://envirover.s3-us-west-2.amazonaws.com/spl/2.5.0/radioroom-2.5.0-jetson.deb) to the NVIDIA Jetson.
2. Install the package.

   ``$ sudo dpkg -i radioroom-2.5.0-raspbian.deb``

3. Configure the reporting period and the serial device paths for autopilot in /etc/radioroom.conf.
4. If ISBD transceiver is used, in [isbd] section set enabled=true and specify the serial device paths of the ISBD transceiver in /etc/radioroom.conf.
5. If TCP/IP connection is used, in [tcp] section set enabled=true and specify the TCP server's IP address and port in /etc/radioroom.conf.
6. Start radioroom service.

   ```bash
   sudo systemctl enable radioroom.service
   sudo systemctl start radioroom.service
   ```

By default the serial device paths are set to /dev/ttyACM0 for autopilot and to /dev/ttyUSB0 for ISBD transceiver. If auto_detect_serials property is set to true, Radio Room can auto-detect autopilot and ISBD if they are available on other serial and USB devices. To make the RadioRoom startup faster and more reliable it is recommended to set the device paths correctly.

USB device paths /dev/ttyUSB0, /dev/ttyUSB1, ... can swap after reboot. For USB devices it is recommended to use symlinks from /dev/serial/by-path or /dev/serial/by-path directories, that do not change with reboots.

Radio Room periodically reports the vehicle's position, attitude, velocity, and other data using HIGH_LATENCY MAVLink message. The message size is 48 bytes. Each report consumes 1 RockBLOCK credit. The reporting period default value is 60 seconds. It can be changed by setting report_period configuration property in /etc/radioroom.conf.

UV RadioRoom relies on TCP keepalive mechanism to detect broken connections. Reliable detection of broken connection is required to reconnect to restarted UV Hub server and to failover to ISBD channel when TCP channel is not healthy. The 7200 seconds system default setting for keepalive time is typically too large. It's recommended to adjust the system default settings for tcp_keepalive_time, tcp_keepalive_intvl, and tcp_keepalive_probes to reduce time required to detect broken connections. See https://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html for TCP keepalive configuration HOWTO.

## Configuring Camera Handlers (optional)

UV Radio Room camera handlers execute custom Linux shell commands on MAVLink commands DO_DIGICAM_CONTROL, IMAGE_START_CAPTURE, IMAGE_STOP_CAPTURE, VIDEO_START_CAPTURE, and VIDEO_STOP_CAPTURE.

The camera handlers are defined in [camera_handler] section in /etc/radioroom.conf file and executed when correspondent commands are received from one of the UV Radio Room's channel or when the autopilot confirms reaching of a mission item with a correspondent command.

The Linux shell commands may include {% raw %}{{param1}}, ..., {{param8}}{% endraw %} variables that during the command execution will be replaced by values of the correspondent MAVLink command parameters.

## Troubleshooting

Run ``$ sudo systemctl status radioroom.service`` to check the status of radioroom service.

If radioroom is properly wired and configured, the output should look like this:

```bash
pi@raspberrypi:~ $ sudo systemctl status radioroom.service
● radioroom.service - UV Radio Room Service
   Loaded: loaded (/etc/systemd/system/radioroom.service; enabled; vendor preset: enabled)
   Active: activating (start) since Tue 2019-06-07 07:27:56 UTC; 6 days ago
     Docs: http://github.com/envirover/SPLRadioRoom
 Main PID: 254 (radioroom)
   CGroup: /system.slice/radioroom.service
           └─254 /usr/sbin/radioroom

Nov 07 07:27:56 raspberrypi systemd[1]: Starting UV Radio Room Service...
Nov 07 07:27:57 raspberrypi radioroom[254]: Starting UV Radio Room 2.5.0...
Nov 07 07:27:57 raspberrypi radioroom[254]: Connecting to autopilot (/dev/ttyUSB0 57600)...
Nov 07 07:27:58 raspberrypi radioroom[254]: Autopilot detected at serial device '/dev/ttyUSB0'.
Nov 07 07:27:58 raspberrypi radioroom[254]: MAV type: 12, system id: 1, autopilot class: 3, firmware version: 3.5.0/255
Nov 07 07:27:58 raspberrypi radioroom[254]: Connecting to ISBD transceiver (/dev/ttyUSB1 19200)...
Nov 07 07:27:58 raspberrypi radioroom[254]: IRIDIUM 9600 Family SBD Transceiver (IMEA 123456789012345) detected at serial device '/dev/ttyUSB1'.
Nov 07 07:27:58 raspberrypi radioroom[254]: UV Radio Room 2.5.0 started.
```

Log file of radioroom service is available at /var/log/radioroom.log.

Add ``-v`` option to the radioroom command line in /etc/systemd/system/radioroom.sevice to enable verbose logging. Verbose logging makes radioroom service log all MAVLink messages sent and received from autopilot and ISBD transceiver.

## Building

To build radioroom on Raspberry Pi.

```bash
sudo apt-get install git cmake
git clone https://github.com/envirover/SPLRadioRoom.git
cd SPLRadioRoom
mkdir bin
cd bin
cmake ..
make
```

To create a debian package run ``$cpack ..`` command after that.

To cross-compile on Windows.

1. Install git and clone SPLRadioRoom repo.

   ``$ git clone https://github.com/envirover/SPLRadioRoom.git``

2. Install [Windows toolchain for Raspberry Pi](http://gnutoolchains.com/raspberry/).
3. Create 'bin' subdirectory inside SPLRadioRoom and change the current directory to it.
4. Run cmake using ../toolchain-arm-windows.cmake toolchain file.

   ``$ cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE:STRING="" -DCMAKE_TOOLCHAIN_FILE:FILEPATH="../toolchain-arm-windows.cmake" ..``

5. Run make.

   ``$ make``

For cross-compilation on Linux raspbian toolchain for Linux is required. toolchain-arm-linux.cmake should be specified as CMake toolchain file.
