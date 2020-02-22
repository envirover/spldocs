---
title: UV Hub Deployment with Docker Containers
keywords: spl, ardupilot, rockblock, satellite, telemetry, iridium, isbd, docker
sidebar: home_sidebar
toc: false
permalink: 2.2/uvhub-docker.html
folder: spl
---

This page provides steps-by-step instructions for deployment of SPL server components ([UV Hub](uvhub.html), [UV Shadow](uvshadow.html), [UV Tracks](uvtracks.html), and [UV Cockpit](uvcockpit.html)) with Docker containers.

## System Requirements

Deploying SPL system with Docker containers requires the following prerequisites:

1. Computer with [Mission Planner](http://ardupilot.org/planner/) or [QGroundControl](http://qgroundcontrol.com/) GCS installed.
2. Computer accessible from the Internet with [Docker](https://www.docker.com/) installed.
3. Activated [RockBLOCK Mk2](http://www.rock7mobile.com/products-rockblock) or [RockBLOCK 9603](http://www.rock7mobile.com/products-rockblock-9603) Iridium satellite communication module, if ISBD channel will be used.

## Installation

Follow these steps to deploy SPL servers with Docker using [container images from Docker Hub](https://hub.docker.com/u/envirover):

1. Download [docker-compose.yml](https://envirover.s3-us-west-2.amazonaws.com/spl/2.2.0/docker-compose.yml) file to the computer with Docker client installed.
2. Modify docker-compose.yml file to change the values of MAV_AUTOPILOT, MAV_TYPE, MAV_SYSID, ROCKBLOCK_IMEI, ROCKBLOCK_USERNAME, ROCKBLOCK_PASSWORD container's environment variable to match your autopilot firmware, RockBLOCK IMEI, Rock 7 user name and password.
3. Use docker-compose command to create the Docker containers:

   ```shell
   docker-compose -f docker-compose.yml up
   ```

4. Register URL `http://<IP address>:5080/mo` as delivery address in [Rock 7 Core services](https://rockblock.rock7.com/Operations) for your RockBLOCK. Choose HTTP_POST as the delivery format.
5. Set `host` configuration properties of [tcp] configuration section in radioroom.conf UV Radio Room configuration file to the UV Hub server IP address.
6. Connect the GCS to the autopilot using regular radio-telemetry or USB connection and save the on-board parameters to a file.
7. Connect the GCS to shadow port 5757 on the UV Hub server machine and upload the on-board parameters from the file to the vehicle. (This will update the parameters in the vehicle shadow.)

After the installation is completed, GCS can be connected to port 5760 on the UV Hub server machine. UV Cockpit home web page will be available at `http://<IP address>/uvcockpit/` address.

## Tightening Security

To restict access to the SPL server configure firewall on the UV Hub server machine to:

* Allow access to port 5080 to IP addresses `109.74.196.135` and `212.71.235.32` of Rock 7 services;
* Allow access to port 5060 to the IP ranges of  UV Radio Room. (Cellular internet providers will typically use dynamic IP addresses those may be reassigned on reboots.)
* Allow access to ports 5760 and 5757 to the GCS machines.
* All access to port 80 to the web browser's client machines.

Make sure that the firewall's in-bound rules are updated whenever the GCS clients IP addresses are changed.

## Troubleshooting

Use `docker log` command to retrieve logs of Docker containers.
