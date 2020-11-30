---
title: UV Hub Deployment on Amazon AWS
keywords: spl, ardupilot, rockblock, satellite, telemetry, iridium, isbd, amazon, aws
sidebar: home_sidebar_23
toc: false
permalink: 2.3/uvhub-aws.html
folder: spl
---

This page provides steps-by-step instructions for deployment of SPL server components ([UV Hub](uvhub.html), [UV Shadow](uvshadow.html), [UV Tracks](uvtracks.html), and [UV Cockpit](uvcockpit.html)) on Amazon Web Services (AWS) cloud computing platform.

## System Requirements

Deploying SPL servers on AWS requires the following prerequisites:
1. Computer with internet access and [Mission Planner](http://ardupilot.org/planner/) or [QGroundControl](http://qgroundcontrol.com/) GCS installed.
2. [Amazon Web Services](https://aws.amazon.com/) account.
3. Activated [RockBLOCK Mk2](http://www.rock7mobile.com/products-rockblock) or [RockBLOCK 9603](http://www.rock7mobile.com/products-rockblock-9603) Iridium satellite communication module, if ISBD channel will be used.

> Use the AWS region with the lowest network latency for Amazon EC2 service. The latency could be measured from the GCS machine using http://cloudharmony.com/speedtest-latency-dns-for-aws tool.

## Installation

Follow these steps to deploy UV Hub and UV Tracks servers on AWS:

1. Open [AWS Console](https://aws.amazon.com/) and select the AWS region.
2. [![Launch Stack](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?#/stacks/new?stackName=uvhub&templateURL=https://envirover.s3-us-west-2.amazonaws.com/spl/2.3.0/uvhub.template)
3. Enter the RockBLOCK IMEI, Rock 7 user name and password, and other CloudFormation stack's input parameters. Make sure that values of `MAVAutopilot` and `MAVType` input parameters match your autopilot firmware.
4. Complete the stack creation wizard and wait until the stack creation is completed.  Once the stack creation is successfully completed, the stack output parameters will be available.
5. Register URL specified by `RockBLOCKHandlerURL` output parameter as delivery address in [Rock 7 Core services](https://rockblock.rock7.com/Operations) for your RockBLOCK. Choose HTTP_POST as the delivery format.
6. Use the IP address and port number specified by `RadioRoomClientURL` output parameter to set `host` and `port` configuration properties of [tcp] configuration section in radioroom.conf UV Radio Room configuration file.
7. Connect the GCS to the autopilot using regular radio-telemetry or USB connection and save the on-board parameters to a file.
8. Connect the GCS to the IP address and port number specified by `ShadowClientURL` output parameter and upload the on-board parameters from the file to the vehicle. (This will update the parameters in the vehicle shadow.)

![UV Hub CloudFormation template](images/uvhub-cf.jpg)

The CloudFormation template deploys UV Hub, UV Tracks, UV Shadow, and UV Cockpit Docker containers with Amazon ECS service using [container images from Docker Hub](https://hub.docker.com/u/envirover).

## Tightening Security

By default ports 5760 and 5757 can be accessed from any client IP address. That is anybody who knows IP address of UV Hub server can connect to it and control your vehicle. To make these ports accessible just from your machine, Specify the client CIDR using `ClientCIDR` stack's input parameter, or after the stack is created, change the in-bound rules for ports 5760 and 5757 in the EC2 security group created by the stack. Determine the public IP address of the GCS machine ([http://checkip.amazonaws.com/](http://checkip.amazonaws.com/)) and set the source IP address for the ports to `<IP address>/32`. Make sure that the security group in-bound rules are updated whenever the clients IP addresses are changed.

Access to port 5080 is used for delivery of mobile originated messages from RockBLOCK. It should be restricted to originating IP addresses `109.74.196.135` and `212.71.235.32` of Rock 7 services.

Controlling access to port 5060 used by TCP/IP connections from UV Radio Room based on IP address ranges is not straightforward. Cellular internet providers typically use dynamic IP addresses those may be reassigned on reboots. To secure UV Radio Room to UV Hub TCP/IP communication it's recommended to use VPN remote-access solutions (for example, see [Software Remote-Access VPN](https://docs.aws.amazon.com/aws-technical-content/latest/aws-vpc-connectivity-options/software-remote-access-vpn-internal-user.html)).

## Troubleshooting

UV Hub and UV Tracks servers logs are stored in /var/log/messages file on the EC2 instance.

To connect to the EC2 instance and view the logs:

1. In AWS Console select EC2 service,
2. Select "Instances" tab in the left-side tabs,
3. Select the EC2 instances and click "Connect" button,
4. In the "Connect to your instance" dialog box select "Session Manager" as connection method and click "Connect" button in the dialog box.
5. In the appeared Session Manager terminal enter:

   `sudo tail -n 100 /var/log/messages`
