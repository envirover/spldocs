---
title: UV Tracks REST API v2
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

`GET /uvtracks/api/v2/state?sysid={SysId}`

#### Request Parameters


|Parameter|Data Type|Required|Description|
|---------|---------|--------|-----------|
|SysId|Integer|No|System ID. The default value is 1.|


#### Response

GetState response body is a GeoJSON feature collection that contains single point feature with properties of [HIGH_LATENCY](uvtracks-apidocs.html#high_latency-message-properties) MAVLink message.

#### Example

Request

`GET /uvtracks/api/v2/state`

Response

```JSON
{
  "features" : [ {
    "geometry" : {
      "coordinates" : [ -0.1460533, 0.0731876, 3.0 ],
      "type" : "Point"
    },
    "properties" : {
      "throttle" : 0,
      "latitude" : 340731876,
      "wp_num" : 0,
      "roll" : 2,
      "groundspeed" : 0,
      "landed_state" : 0,
      "gps_nsat" : 6,
      "compid" : 1,
      "temperature" : 8,
      "pitch" : -74,
      "wp_distance" : 0,
      "gps_fix_type" : 4,
      "longitude" : -01460533,
      "failsafe" : 0,
      "heading" : 8711,
      "sysid" : 1,
      "airspeed_sp" : 0,
      "base_mode" : 89,
      "msgid" : 234,
      "airspeed" : 0,
      "temperature_air" : 8,
      "altitude_sp" : 3,
      "battery_remaining" : 99,
      "altitude_amsl" : 3,
      "custom_mode" : 5,
      "heading_sp" : 8700,
      "climb_rate" : 0,
      "time" : 1582261908360
    },
    "type" : "Feature"
  } ],
  "type" : "FeatureCollection"
}
```

### GetTracks

Returns a sequence of reported states of the vehicle or the vehicle's track line in <a href="https://geojson.org">GeoJSON</a> format.

#### Syntax

`GET /uvtracks/api/v2/tracks?sysid={SysId}&geometryType={GeometryType}&startTime={StartTime}&endTime={EndTime}&top={Top}`

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

`GET /uvtracks/api/v2/tracks?geometryType=line`

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

`GET /uvtracks/api/v2/missions?sysid={SysId}&geometryType={GeometryType}`

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

`GET /uvtracks/api/v2/missions`

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

`GET /uvtracks/api/v2/parameters?sysid={SysId}`

#### Request Parameters


|Parameter|Data Type|Required|Description|
|---------|---------|--------|-----------|
|SysId|Integer|No|System ID. The default value is 1.|


#### Response

GetState response body contains flat key-value map of on-board parameters in JSON format.

#### Example

Request

`GET /uvtracks/api/v2/parameters`

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
|temperature|Integer|Battery 1 voltage (volts)|
|temperature_air|Integer|Battery 2 voltage (volts)|
|failsafe|Integer|Failsafe (each bit represents a failsafe where 0=ok, 1=failsafe active (bit0:RC, bit1:batt, bit2:GPS, bit3:GCS, bit4:fence)|
|wp_num|Integer|Current waypoint number|
|wp_distance|Integer|Distance to target (meters)|
|time|Integer|Report time (UNIX epoch time)|
  
## Errors

When operations fail, UV Tracks service returns HTTP status code greater or equal to 400 and an errors response in the following format.

```JSON
{
    "code":500,
    "message":"Operation failed."
}
```

