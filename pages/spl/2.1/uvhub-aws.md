---
title: UV Hub and UV Tracks Installation on Amazon AWS
keywords: spl, ardupilot, rockblock, satellite, telemetry, iridium, isbd, amazon, aws
sidebar: home_sidebar_21
toc: false
permalink: 2.1/uvhub-aws.html
folder: spl
---

These instructions provide steps-by-step instructions for [UV Hub](uvhub.html) and [UV Tracks](uvtracks.html) servers installation on Amazon Web Service (AWS) cloud services platform.

## System Requirements

Deploying UV Hub system on AWS requires the following data and services:
1. Computer with internet access and [Mission Planner](http://ardupilot.org/planner/) or [QGroundControl](http://qgroundcontrol.com/) GCS installed.
2. [Amazon Web Services](https://aws.amazon.com/) account.
3. Activated [RockBLOCK Mk2](http://www.rock7mobile.com/products-rockblock) or [RockBLOCK 9603](http://www.rock7mobile.com/products-rockblock-9603) Iridium satellite communication module, if ISBD channel will be used.

## Installation

Follow these steps to deploy UV Hub and UV Tracks servers on AWS:
1. Open [AWS Console](https://aws.amazon.com/) and select the AWS region.
2. [Create a key pair in EC2 service](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair).
3. [![Launch Stack](images/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?#/stacks/new?stackName=uvhub&templateURL=https://s3-us-west-2.amazonaws.com/envirover/UVHub/uvhub.template)
5. Enter the RockBLOCK IMEI, Rock 7 user name and password, and other CloudFormation stack's input parameters. Select the key pair name in the KeyName input parameter drop-down list. Make sure that values of `MAVAutopilot` and `MAVType` input parameters match your autopilot firmware.
6. Complete the stack creation wizard and wait until the stack creation is completed.  Once the stack creation is successfully completed, the stack output parameters will be available.
7. Register URL specified by `RockBLOCKHandlerURL` output parameter as delivery address in [Rock 7 Core services](https://rockblock.rock7.com/Operations) for your RockBLOCK.
8. Use the IP address and port number specified by `RadioRoomClientURL` output parameter to set `host` and `port` configuration properties of [tcp] configuration section in radioroom.conf UV Radio Room configuration file.
9. Connect the GCS to the autopilot using regular radio-telemetry or USB connection and save the on-board parameters to a file. 
10. Connect the GCS to the IP address and port number specified by `ShadowClientURL` output parameter and upload the on-board parameters from the file to the vehicle. (This will update the parameters in the vehicle shadow.)


![UV Hub CloudFormation template](images/uvhub-cf.jpg)

By default, the stack creates two EC2 instances of t2.micro type. t2.micro instances are eligible for [AWS free tier](https://aws.amazon.com/s/dm/optimization/server-side-test/free-tier/free_np/). The stack also creates [AWS Elasticsearch service](https://aws.amazon.com/elasticsearch-service/) instance of t2.small.elasticsearch type.

## Tightening Security

By default ports 5760 and 5757 can be accessed from any client IP address. That is anybody who knows IP address of UV Hub server can connect to it and control your vehicle. To make these ports accessible just from your machine, Specify the client CIDR using `ClientCIDR` stack's input parameter, or after the stack is created, change the in-bound rules for ports 5760 and 5757 in the EC2 security group created by the stack. Determine the public IP address of the GCS machine ([http://checkip.amazonaws.com/](http://checkip.amazonaws.com/)) and set the source IP address for the ports to `<IP address>/32`. Make sure that the security group in-bound rules are updated whenever the GCS clients IP addresses are changed.

Access to port 8080 is used for delivery of mobile originated messages from RockBLOCK is restricted to originating IP addresses `109.74.196.135` and `212.71.235.32` of Rock 7 services.

Controlling access to port 5060 used by TCP/IP connections from UV Radio Room based on IP address ranges is not practical. Cellular internet providers will typically use dynamic IP addresses those may be reassigned on reboots. To secure UV Radio Room to UV Hub TCP/IP communication it's recommended to use VPN  remote-access solutions (for example, see [Software Remote-Access VPN](https://docs.aws.amazon.com/aws-technical-content/latest/aws-vpc-connectivity-options/software-remote-access-vpn-internal-user.html)).

## Troubleshooting

`CloudWatchLogs` stack output parameters links to the CloudWatch log group with UV Hub and UV Tracks servers logs. 
