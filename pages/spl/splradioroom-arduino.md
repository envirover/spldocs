---
title: SPL RadioRoom for Arduino
keywords: spl, ardupilot, mavlink, rockblock, satellite, telemetry, iridium, unmanned vehicle, sbd, arduino
sidebar: spl_sidebar
toc: false
permalink: splradioroom-arduino.html
folder: spl
---

SPL RadioRoom for Arduino requires the following hardware components:
* Arduino 101 board,
* RockBLOCK Mk2 Iridium satellite communication module,
* ArduPilot-based autopilot, such as Pixhawk. 

With additional RS-232 Arduino shield SPLRadioRoom might also work with RockBLOCK+ and RockFLEET modules.

#### Wiring 

|  Arduino  | RockBLOCK | AP TELEM1 |
|-----------|-----------|-----------|
| +5V       | 8 +5V     | 1 +5V     |
| 2         |           | 2         |
| 3         |           | 3         |
| 8         | 3 TX      |           |
| 9         | 2 RX      |           |
| +3.3v     | 12 Sleep  |           |
| Gnd       | 7 Gnd     | 6 Gnd     |

In this wiring diagram both Arduino and RockBLOCK are powered by ArduPilot's TELEM1 power line.

