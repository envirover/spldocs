---
title: UV Tracks REST API
keywords: spl, mavlink, rockblock, satellite, telemetry, rest, api, service
sidebar: home_sidebar_21
toc: false
permalink: 2.1/uvtracks-apidocs.html
folder: spl
---

UV Tracks service provides access to missions and reported states saved to the 
vehicle shadow by UV Hub server of <a href="http://envirover.com/spl.html">SPL Global Telemetry System</a>.

## Actions

The following table provides a quick reference operations for the REST interface to the UV Tracks service. 
The description of each operation also includes the required HTTP method.


| Action                                            | HTTP Method |
|---------------------------------------------------|-------------|
| [GetTracks](uvtracks-apidocs.html#gettracks)      | GET         |
| [GetMissions](uvtracks-apidocs.html#getmissions)  | GET         |

### GetTracks

Returns reported positions and state information of the vehicle in <a href="http://geojson.org">GeoJSON</a> format.

#### Syntax

`GET /uvtracks/api/v1/tracks?sysid={SysId}&startTime={StartTime}&endTime={EndTime}&top={Top}`

#### Request Parameters


|Parameter|Data Type|Required|Description|
|---------|---------|--------|-----------|
|SysId|Integer|No|System ID. The default value is 1.|
|StartTime|Integer|No|Start of time range query as UNIX epoch time.|
|EndTime|Integer|No|End of time range query as UNIX epoch time.|
|Top|Integer|No|Maximum number of returned entries. The default value is 100.|


#### Response

GetTracks response body contains GeoJSON point feature collection with properties of [HIGH_LATENCY](uvtracks-apidocs.html#high_latency-message-properties) MAVLink message.

#### Example

Request

`GET /uvtracks/api/v1/tracks?top=1`

Response

```JSON
{
   "features":[
      {
         "geometry":{
            "coordinates":[
               -117.1461032,
               34.0733229,
               469.0
            ],
            "type":"Point"
         },
         "properties":{
            "throttle":0,
            "latitude":340733229,
            "wp_num":0,
            "roll":0,
            "groundspeed":0,
            "landed_state":0,
            "gps_nsat":6,
            "compid":0,
            "temperature":16,
            "pitch":198,
            "wp_distance":0,
            "gps_fix_type":3,
            "longitude":-1171461032,
            "failsafe":0,
            "heading":19177,
            "sysid":1,
            "airspeed_sp":0,
            "base_mode":89,
            "msgid":234,
            "airspeed":0,
            "temperature_air":0,
            "altitude_sp":0,
            "battery_remaining":99,
            "altitude_amsl":469,
            "custom_mode":5,
            "heading_sp":-16800,
            "climb_rate":0,
            "time":1538107742933
         },
         "type":"Feature"
      }
   ],
   "type":"FeatureCollection"
}
```

### GetMissions

Returns mission plan of the vehicle.

#### Syntax

`GET /uvtracks/api/v1/missions?sysid={SysId}}`

#### Request Parameters

|Parameter|Data Type|Required|Description|
|---------|---------|--------|-----------|
|SysId|Integer|No|System ID. The default value is 1.|

#### Response

GetMissions response body contains mission plan in [QGroundControl JSON format](uvtracks-apidocs.html#qgroundcontrol-mission-plan).

#### Example

Request

`GET /uvtracks/api/v1/missions`

Response

```JSON
{
   "fileType":"Plan",
   "groundStation":"QGroundControl",
   "version":1,
   "geoFence":{
      "polygon":[
      ],
      "version":1
   },
   "mission":{
      "cruiseSpeed":0,
      "firmwareType":3,
      "hoverSpeed":0,
      "items":[
         {
            "type":"SimpleItem",
            "autoContinue":true,
            "command":22,
            "doJumpId":2,
            "frame":3,
            "params":[
               15.0,
               0.0,
               0.0,
               0.0,
               0.0,
               0.0,
               500.0
            ]
         },
         {
            "type":"SimpleItem",
            "autoContinue":true,
            "command":16,
            "doJumpId":3,
            "frame":3,
            "params":[
               0.0,
               0.0,
               0.0,
               0.0,
               34.073726654052734,
               -117.15079498291016,
               500.0
            ]
         }
      ],
      "plannedHomePosition":[
         34.072021484375,
         -117.15083312988281,
         474.0
      ],
      "vehicleType":2,
      "version":2
   },
   "rallyPoints":{
      "version":1,
      "points":[
      ]
   }
}
```

## Data Types

### HIGH_LATENCY Message Properties

|Field|Data Type|Description|
|-----|---------|-----------|
|base_mode|Integer|Bitmap of enabled system modes (see MAV_MODE_FLAG)|
|custom_mode|Integer|A bitfield for use for autopilot-specific flags|
|landed_state|Integer|The landed state (see MAV_LANDED_STATE enum). Is set to MAV_LANDED_STATE_UNDEFINED if landed state is unknown.|
|roll|Integer|Roll (degrees)|
|pitch|Integer|Pitch (degrees)|
|heading|Integer|Heading (degrees)|
|throttle|Integer|Throttle (percentage)|
|heading_sp|Integer|Heading setpoint (degrees)|
|latitude|Integer|Latitude (decimal degrees)|
|longitude|Integer|Longitude (decimal degrees)|
|altitude_amsl|Integer|Altitude above mean sea level (meters)|
|altitude_sp|Integer|Altitude setpoint relative to the home position (meters)|
|airspeed|Integer|Airspeed (meters/seconds)|
|airspeed_sp|Integer|Airspeed setpoint (meters/seconds)|
|groundspeed|Integer|Ground speed (meters/seconds)|
|climb_rate|Integer|Climb rate (meters/seconds)|
|gps_nsat|Integer|Number of satellites visible. If unknown, set to 255|
|gps_fix_type|Integer|GPS Fix type (GPS_FIX_TYPE enum)|
|battery_remaining|Integer|Remaining battery (percentage)|
|temperature|Integer|Autopilot temperature (degrees C) or battery voltage (volts)|
|temperature_air|Integer|Air temperature (degrees C) from airspeed sensor or battery current (amp)|
|failsafe|Integer|Failsafe (each bit represents a failsafe where 0=ok, 1=failsafe active (bit0:RC, bit1:batt, bit2:GPS, bit3:GCS, bit4:fence)|
|wp_num|Integer|Current waypoint number|
|wp_distance|Integer|Distance to target (meters)|
|time|Integer|Report time (UNIX epoch time)|
  
### QGroundControl Mission Plan

|Field|Data Type|Description|
|-----|---------|-----------|
|fileType|String|Document type|
|groundStation|String|GCS type|
|version|Integer|document version|
|geoFence|Object|GeoFence (not supported yet)|
|mission|Object|Mission|
|rallyPoints|Object|Rally Points (not supported yet)|


## Errors

When operations fail, UV Tracks service returns HTTP status code greater or equal to 400 and an errors response in the following format.

```JSON
{
    "code":500,
    "message":"Operation failed."
}
```
