SPLStream and SPLTracks web services provide a solution for storing and visualizing data reported by SPLRadioRoom.

![SPLStream and SPLTracks](/images/spltracks.jpg)

### SPLStream 
SPLStream web service saves MAVLink messages received from Rock7Core to AWS DynamoDB database service.

### SPLTracks
SPLTracks provides access to the reported data in GeoJSON format. 

The required query string parameter 'devices' must contain a comma-separated list of RockBLOCK IMEI. The optional query string parameters 'startTime' and 'endTime' specify a time range in UNIX epoch time for the retrieved data. The optional query string parameter 'type' defines the GeoJSON geometry type. The accepted values of 'type' parameter are 'point' (default) and 'linestring'.

[SPL Tracks GeoJSON web service demo](http://spldemo.envirover.com/spltracks/features/?devices=300234064280890&startTime=1499736149000&endTime=1499742468000)

[SPL Tracks web map demo](http://spldemo.envirover.com/spltracks/?devices=300234064280890&startTime=1499736149000&endTime=1499742468000)

### Depoyment
SPLStream and SPLTracks web applications could run in any java application servers. Because the application require low latency connection to DynamoDB database service, it is recommended to run these web applications in AWS EC2. 

The easiest way to deploy SPLStream and SPLTracks web applications on AWS is using [Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk) service. 

1. Login to [AWS Console](https://console.aws.amazon.com/console/home).
2. Switch to [IAM service](https://console.aws.amazon.com/iam/home) service.
3. Select 'Roles' in the vertical menu on the left.
4. Click on aws-elasticbeanstalk-ec2-role in the list of available roles.
5. Click 'Attach Policy' button, find and check 'AmazonDynamoDBFullAccess' policy in the list of IAM policies.
6. Click 'Attach Policy'.
7. Download spltracks-2.0-bin.zip file from [https://github.com/envirover/SPLGroundControl/releases](https://github.com/envirover/SPLGroundControl/releases).
8. Switch to [Elastic Beanstalk](https://console.aws.amazon.com/elasticbeanstalk/home) service in the AWS console.
9. Click 'Create New Application' link.
10. Enter 'spltracks' as the application name and 'SPLStream and SPLTracks' as the application description. Click 'Create'.
11. Click 'Actions' button and select 'Create environment'.
12. Select 'Web server environment' in the menu and click 'Select'.
13. Select 'Preconfigured platform' option, and in the list of preconfigured platforms select 'Tomcat'.
14. In the 'Application code' option select 'Upload your code' and select the downloaded spltracks-2.0-bin.zip.
15. Click 'Create environment'.
16. After the application is ready, register ```http://<application URL>/splstream/rockblock``` URL as a delivery address for your RockBBLOCK. 

The GeoJSON web service URL will be available at ``http://<application URL>/spltracks/features/?devices=<ISBD IMEA>``. The demo web page URL will be available at ``http://spldemo.envirover.com/spltracks/?devices=<ISBD IMEA>``. Query string parameters ``startTime`` and ``endTime`` could be used to specify the time interval of the requested data.