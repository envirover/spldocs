---
title: SPL GroundControl Installation on Amazon AWS
keywords: spl, ardupilot, rockblock, satellite, telemetry, iridium, unmanned vehicle, sbd, amazon, aws
sidebar: spl_sidebar
toc: false
permalink: splgroundcontrol-aws.html
folder: spl
---
SPL GroundControl server handles communication between ground control stations and Rock7Core web services. Though SPL GroundControl can be installed on any computer accessible from the Internet, it is recommended to deploy it on Amazon Web Services (AWS) using the provided CloudFormation template.

## System Requirements

Deploying SPL GroundControl system on AWS requires the following data and services:
1. [Amazon Web Services](https://aws.amazon.com/) account.
2. Activated [RockBLOCK Mk2](http://www.rock7mobile.com/products-rockblock) or [RockBLOCK 9603](http://www.rock7mobile.com/products-rockblock-9603) Iridium satellite communication module.
3. On-board parameters file created by [QGroundControl](http://qgroundcontrol.com/) ground control station.

## Installing

Follow these steps to deploy SPL GroundControl on AWS:
1. Open [AWS Console](https://aws.amazon.com/) and select the AWS region.
2. [Create a key pair in EC2 service](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair).
3. Copy on-board parameters of the vehicle to S3:
   * Connect QGroundControl GCS to the vehicle, 
   * Save on-board parameters to a file, 
   * [Create an S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html),
   * [Upload the file to the S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/PuttingAnObjectInABucket.html) giving everyone read access to the uploaded file, 
   * Note the S3 object URL.  
4. [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)]( https://console.aws.amazon.com/cloudformation/home?#/stacks/new?stackName=spl&templateURL=https://s3-us-west-2.amazonaws.com/envirover/spl/spl.template)
5. Enter the parameters URL, RockBLOCK IMEI, Rock 7 username and password, and other CloudFormation stack's input parameters.
6. Complete the stack creation wizard and wait until the stack creation is complete.
7. Register URL specified by the stack's `RockBLOCKHandlerURL` output parameter as delivery address in [Rock 7 Core services](https://rockblock.rock7.com/Operations) for your RockBLOCK.

By default the stack creates t2.micro type EC2 instance that is eligible for [AWS free tier](https://aws.amazon.com/s/dm/optimization/server-side-test/free-tier/free_np/).  

## Tightening Security

By default MAVLink port 5760 can be accessed from any client IP address. That is anybody who knows IP address of SPL GroundControl server can connect to it and control your vehicle. To make these ports accessible just from your machine, change the in-bound rules for port 5760 in the security group created by the stack. Determine the public IP address of the GCS machine ([http://checkip.amazonaws.com/](http://checkip.amazonaws.com/)) and set the source IP address for 5760 port to `<IP address>/32`. 

Port 8080 is used for delivery of mobile originated messages from RockBLOCK. To prevent unauthorized access to this port, it is recommended to restrict access to this port to the originating IP addresses `109.74.196.135` and `212.71.235.32` of Rock 7 services.

## Troubleshooting

`CloudWatchLogs` stack output parameters links to the CloudWatch log group with the SPL GroundControl application logs. 
