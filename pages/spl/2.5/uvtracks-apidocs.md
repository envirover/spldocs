---
title: UV Tracks REST API v3
keywords: spl, mavlink, rockblock, satellite, telemetry, rest, api, service
sidebar: home_sidebar
toc: false
permalink: uvtracks-apidocs.html
folder: spl
---

UV Tracks web service provides access to reported states, missions, and on-board parameters stored in the
vehicle's shadow.

## Actions

The following table provides a quick reference operations for the REST interface to the UV Tracks service. 
The description of each operation also includes the required HTTP method.


| Action                                               | HTTP Method |
|------------------------------------------------------|-------------|
| [GetState](uvtracks-apidocs.html#getstate)           | GET         |
| [GetTracks](uvtracks-apidocs.html#gettracks)         | GET         |
| [GetMissions](uvtracks-apidocs.html#getmissions)     | GET         |
| [GetParameters](uvtracks-apidocs.html#getparameters) | GET         |

### GetState

Returns the last reported state of the vehicle in <a href="https://geojson.org">GeoJSON</a> format.

#### Syntax

`GET /uvtracks/api/v3/state?sysid={SysId}`

#### Request Parameters


|Parameter|Data Type|Required|Description|
|---------|---------|--------|-----------|
|SysId|Integer|No|System ID. The default value is 1.|


#### Response

GetState response body is a GeoJSON feature collection that contains single point feature with the last [state report](uvtracks-apidocs.html#state-report-message-properties).

#### Example

Request

`GET /uvtracks/api/v3/state`

Response

```JSON
{
  "features" : [ {
    "geometry" : {
      "coordinates" : [ -123.4567, 89.01234, 489.0 ],
      "type" : "Point"
    },
    "properties" : {
      "altitude" : 489.0,
      "throttle" : 0,
      "target_altitude" : 1.0,
      "battery_voltage" : 8.0,
      "target_distance" : 0.0,
      "autopilot" : 3,
      "latitude" : 89.01234,
      "wp_num" : 0,
      "roll" : 0.0,
      "channel" : 1,
      "failure_flags" : 3325,
      "groundspeed" : 0.0,
      "battery" : 98,
      "compid" : 1,
      "pitch" : -2.0,
      "mav_type" : 2,
      "timestamp" : 618415,
      "longitude" : -123.4567,
      "target_heading" : 0.0,
      "heading" : 278.0,
      "sysid" : 1,
      "airspeed_sp" : 0.0,
      "wind_heading" : 0.0,
      "msgid" : 235,
      "airspeed" : 0.0,
      "temperature_air" : 51.0,
      "custom_mode" : 81,
      "windspeed" : 0.0,
      "climb_rate" : 0.0,
      "time" : 1620965880129,
      "battery2_voltage" : 8.0
    },
    "type" : "Feature"
  } ],
  "type" : "FeatureCollection"
}
```

### GetTracks

Returns a sequence of reported states of the vehicle or the vehicle's track line in <a href="https://geojson.org">GeoJSON</a> format.

#### Syntax

`GET /uvtracks/api/v3/tracks?sysid={SysId}&geometryType={GeometryType}&startTime={StartTime}&endTime={EndTime}&top={Top}`

#### Request Parameters


|Parameter|Data Type|Required|Description|
|---------|---------|--------|-----------|
|SysId|Integer|No|System ID. The default value is 1.|
|GeometryType|'point' or 'line'| No | Default value is 'point'.|
|StartTime|Integer|No|Start of time range query as UNIX epoch time.|
|EndTime|Integer|No|End of time range query as UNIX epoch time.|
|Top|Integer|No|Maximum number of returned entries. The default value is 100.|


#### Response

Depending on `geometryType` request parameter value the GetTracks response body contains 
GeoJSON FeatureCollection with either multiple point features or single line feature.

#### Example

Request

`GET /uvtracks/api/v3/tracks?geometryType=line`

Response

```JSON
{
  "features" : [ {
    "geometry" : {
      "coordinates" : [ [ -0.1460726, 0.0731741, 3.0 ], [ -0.1460927, 0.0731382, 3.0 ], [ -0.1460174, 0.073185, 2.0 ], [ -0.1460505, 0.0731599, 2.0 ], [ -0.1459696, 0.0732142, 2.0 ], [ -0.1459233, 0.0732356, 2.0 ], [ -0.1459405, 0.0732113, 2.0 ], [ -0.1459403, 0.0732076, 2.0 ], [ -0.1459539, 0.0731775, 2.0 ], [ -0.1459616, 0.0731438, 2.0 ] ],
      "type" : "LineString"
    },
    "properties" : {
      "sysid" : 1,
      "length" : 42.65323908357078,
      "to_time" : 1582261894276,
      "from_time" : 1582261047402
    },
    "type" : "Feature"
  } ],
  "type" : "FeatureCollection"
}
```

### GetMissions

Returns mission items of the vehicle in <a href="https://geojson.org">GeoJSON</a> format.

#### Syntax

`GET /uvtracks/api/v3/missions?sysid={SysId}&geometryType={GeometryType}`

#### Request Parameters

|Parameter|Data Type|Required|Description|
|---------|---------|--------|-----------|
|SysId|Integer|No|System ID. The default value is 1.|
|GeometryType|'point' or 'line'| No | Default value is 'point'.|

#### Response

Depending on `geometryType` request parameter value the GetMissions response body contains 
GeoJSON FeatureCollection with either multiple point features or single line feature.

#### Example

Request

`GET /uvtracks/api/v3/missions`

Response

```JSON
{
  "features" : [ {
    "geometry" : {
      "coordinates" : [ -0.14617156982422, 0.072689056396484, 469.0 ],
      "type" : "Point"
    },
    "properties" : {
      "sysid" : 255,
      "msgid" : 39,
      "param3" : 0.0,
      "param4" : 0.0,
      "param1" : 0.0,
      "command" : 16,
      "param2" : 0.0,
      "autocontinue" : 1,
      "target_component" : 1,
      "current" : 1,
      "compid" : 0,
      "x" : 0.07269,
      "y" : -0.14617,
      "z" : 469.0,
      "target_system" : 1,
      "seq" : 0,
      "frame" : 0
    },
    "type" : "Feature"
  }, 
...
  {
    "geometry" : {
      "coordinates" : [ -0.1454849243164, 0.0704460144043, 519.0 ],
      "type" : "Point"
    },
    "properties" : {
      "sysid" : 255,
      "msgid" : 39,
      "param3" : 0.0,
      "param4" : "NaN",
      "param1" : 0.0,
      "command" : 16,
      "param2" : 0.0,
      "autocontinue" : 1,
      "target_component" : 1,
      "current" : 0,
      "compid" : 0,
      "x" : 0.070446,
      "y" : -0.145485,
      "z" : 50.0,
      "target_system" : 1,
      "seq" : 5,
      "frame" : 3
    },
    "type" : "Feature"
  } ],
  "type" : "FeatureCollection"
}
```

### GetParameters

Returns the last on-board parameters of the vehicle in JSON format.

#### Syntax

`GET /uvtracks/api/v3/parameters?sysid={SysId}`

#### Request Parameters


|Parameter|Data Type|Required|Description|
|---------|---------|--------|-----------|
|SysId|Integer|No|System ID. The default value is 1.|


#### Response

GetState response body contains flat key-value map of on-board parameters in JSON format.

#### Example

Request

`GET /uvtracks/api/v3/parameters`

Response

```JSON
{
  "YAW2SRV_DAMP" : 0.0,
  "CRASH_DETECT" : 0.0,
  "SERVO3_TRIM" : 1100.0,
...
  "HOME_RESET_ALT" : 0.0,
  "ARSPD_PIN" : 15.0,
  "RTL_AUTOLAND" : 0.0
}
```

## Data Types

### State Report Message Properties

|Field|Data Type|Description|
|-----|---------|-----------|
|airspeed_sp|Float|Airspeed setpoint (meters/seconds)|
|airspeed|Float|Airspeed (meters/seconds)|
|altitude|Float|Altitude above mean sea level (meters)|
|autopilot|Integer|Autopilot type/class (MAV_AUTOPILOT)|
|battery_voltage|Float|Battery 1 voltage (volts)|
|battery|Integer|Remaining battery (percentage)|
|battery2_voltage|Float|Battery 2 voltage (volts)|
|channel|Integer|Comm channel used|
|climb_rate|Float|Maximum climb rate magnitude since last message (meters/second)|
|compid|Integer|Component Id|
|custom_mode|Integer|A bitfield for use for autopilot-specific flags (2 byte version)|
|failure_flags|Integer|Bitmap of failure flags|
|groundspeed|Float|Ground speed (meters/seconds)|
|heading|Float|Heading (degrees)|
|latitude|Float|Latitude (decimal degrees)|
|longitude|Float|Longitude (decimal degrees)|
|mav_type|Integer|Type of the MAV (MAV_TYPE)|
|msgid|Integer|Message Id|
|pitch|Float|Pitch (degrees)|
|roll|Float|Roll (degrees)|
|sysid|Integer|System Id|
|target_altitude|Float|Altitude setpoint relative to the home position (meters)|
|target_distance|Float|Distance to target (meters)|
|target_heading|Float|Target heading (degrees)|
|temperature_air|Float|Air temperature from airspeed sensor (degrees)|
|throttle|Integer|Throttle (percentage)|
|time|Integer|UNIX Epoch time when the message was received|
|timestamp|Integer|Time since boot (milliseconds)|
|wind_heading|Float|Wind heading (degrees)|
|windspeed|Float|Windspeed (meters/second)|
|wp_num|Integer|Current waypoint number|

## Errors

When operations fail, UV Tracks service returns HTTP status code greater or equal to 400 and an errors response in the following format.

```JSON
{
    "code":500,
    "message":"Operation failed."
}
```
