---
title: SPL GroundControl Installation on Amazon AWS
keywords: spl, ardupilot, mavlink, rockblock, satellite, telemetry, iridium, unmanned vehicle, sbd, amazon, aws
sidebar: spl_sidebar
toc: false
permalink: splgroundcontrolaws.html
folder: spl
---

1. Create [Amazon AWS](https://aws.amazon.com/) account.
2. Select the nearest region in AWS Console.
3. [Create a key pair in EC2 service](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair).
4. Copy on-board parameters of the vehicle to S3:
   * Connect QGroundControl GCS to the vehicle, 
   * Save on-board parameters to a file, 
   * [Create an S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html),
   * [Upload the file to the S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/PuttingAnObjectInABucket.html) giving everyone read access to the uploaded file, 
   * Note the S3 object URL (a.k.a. link).  
5. [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)]( https://console.aws.amazon.com/cloudformation/home?#/stacks/new?stackName=spl&templateURL=https://s3-us-west-2.amazonaws.com/envirover/spl/spl.template)
6. Enter the parameters URL, RockBLOCK IMEI, Rock 7 username and password in the CloudFormation stack input parameters section.
7. Complete the stack creation wizard and wait until the stack creation is complete.
8. Switch to the stack outputs.
9. Register the URL specified by RockBLOCKHandlerURL output parameter in Rock 7 Core services for your RockBLOCK.

CloudWatchLogs stack output parameters links to the CloudWatch log group with SPLgroundControl application logs. 

For SPL Stream and SPL Tracks installation instructions see [https://github.com/envirover/SPLGroundControl/wiki/SPLStream-and-SPLTracks-Web-Services](https://github.com/envirover/SPLGroundControl/wiki/SPLStream-and-SPLTracks-Web-Services).

:exclamation: By default MAVLink (5760) and SSH (22) ports can be accessed from any client IP. That is anybody who knows IP address of SPL server can connect to it and control your drone. To make these ports accessible just from your machine, determine the public IP address of the machine [http://checkip.amazonaws.com/](http://checkip.amazonaws.com/) and set ClientCIDR input parameter to IP/32. If the stack is already created change the inbound rules in the security group created by the stack.

:moneybag:  By default the stack creates t2.micro type EC2 instance that is eligible for [AWS free tier](https://aws.amazon.com/s/dm/optimization/server-side-test/free-tier/free_np/).  

