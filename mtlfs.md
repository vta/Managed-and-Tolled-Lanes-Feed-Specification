# Managed and Tolled Lanes Feed Specification (MTLFS)
This document explains the types of files and data that comprise the Managed and Tolled Lanes Feed Specification (MTLFS) and defines the fields used in all of those files.

## Table of Contents
* [Files](#files)
* [Output Format](#output-format)
* Field Definitions
    * [mtlfs.json](#mtlfsjson)
    * [toll_authority_info.json](#toll_authority_infojson)
    * [general_toll_info.json](#general_toll_infojson)
    * [toll_periods.json](#toll_periodsjson)
    * [toll_status.json](#toll_statusjson)
    * [toll_facility.json](#toll_facilityjson)
    * [toll_destination.json](#toll_destinationjson)
    * [toll_signs_geom.json](#toll_signs_geomjson)
    * [facility_geom.json](#facility_geomjson)
    * [gantry_geom.json](#gantry_geomjson)

## Files
This specification defines the following files along with their associated content:

File Name                   | Required      | Defines
--------------------------- | ------------  | ----------
mtlfs.json                  | Optional      | Auto-discovery file that links to all of the other files published by the system. This file is optional, but highly recommended.
toll_authority_info.json    | Yes           | Describes the system including System operator, URLs, contact info, time zone
general_toll_info.json      | Yes           | Describes toll payment methods, min and max tolls, and restrictions
toll_periods.json           | Optional      | Describes the name and operational dates and times of the facilities
toll_status.json           | Optional      | Provides realtime info of the prices and messaging of the toll signs
toll_facility.json          | Optional      | Describes the lane types of the facility
toll_destination.json       | Yes           | Describes entrances and exits to the facility
toll_signs_geom.json     | Optional      | Describes where the physical toll signs are located
facility_geom.json     | Optional      | Describes the different facilities's coverage area 
gantry_geom.json       | Required      | Describes where the physical toll signs are located

### Output Format
Every JSON file presented in this specification except the three geojson files contains the same common header information at the top level of the JSON response object:

Field Name          | Required  | Defines
--------------------| ----------| ----------
last_updated        | Yes       | Integer POSIX timestamp indicating the last time the data in this feed was updated
ttl                 | Yes       | Integer representing the number of seconds before the data in this feed will be updated again (0 if the data should always be refreshed)
data                | Yes       | JSON hash containing the data fields for this response

Example:
```json
{
  "last_updated": 20170815130358,
  "ttl": 3600,
  "data": {}
}
```

### mtlfs.json
The following fields are all attributes within the main "data" object for this feed.

Field Name              | Required    | Defines
------------------------| ------------| ----------
_language_              | Yes         | The language that all of the contained files will be published in. This language must match the value in the system_information file
  \- feeds               | Yes         | An array of all of the feeds that are published by this auto-discovery file
  \- name                | Yes         | Key identifying the type of feed this is (e.g. "system_information", "station_information")
  \- url                 | Yes         | Full URL for the feed

Example:
```json
// 20170815130358
// https://mtlfs.vta.org/mtlfs/mtlfs.json

{
  "last_updated": 1502827438,
  "ttl": 60,
  "data": {
    "en": {
      "feeds": [
        {
          "name": "mtlfs",
          "url": "https://mtlfs.vta.org/mtlfs/mtlfs.json"
        },
        {
          "name": "toll_authority_info",
          "url": "https://mtlfs.vta.org/mtlfs/toll_authority_info.json"
        },
	{
          "name": "general_toll_info",
          "url": "https://mtlfs.vta.org/mtlfs/general_toll_info.json"
        },
	{
          "name": "toll_signs",
          "url": "https://mtlfs.vta.org/mtlfs/toll_signs.json"
        },
        {
          "name": "toll_periods",
          "url": "https://mtlfs.vta.org/mtlfs/toll_periods.json"
        },
        {
          "name": "toll_facility",
          "url": "https://mtlfs.vta.org/mtlfs/toll_facility.json"
        },
        {
          "name": "toll_destination",
          "url": "https://mtlfs.vta.org/mtlfs/toll_destination.json"
        }
	]
    }
  }
}
```

### toll_authority_info.json
Describes the system including System operator, URLs, contact info, time zone.  A JSON array of hours defined as follows:

Field Name          | Required    | Defines
--------------------| ------------| ----------
agency_id        | Yes         | Uniquely identifies a tolling agency. A tolling feed may represent data from more than one agency. The agency_id is dataset unique. This field is optional for tolling feeds that only contain data for a single agency.
agency_name        | Yes         | Full name of the agency operating the tolled or managed lanes. 
agency_url              | Yes         | Contains the URL of the agency. The value must be a fully qualified URL that includes http:// or https://, and any special characters in the URL must be correctly escaped. See http://www.w3.org/Addressing/URL/4_URI_Recommentations.html for a description of how to create fully qualified URL values.
agency_timezone        | Yes         | Contains the timezone where the agency is located. Timezone names never contain the space character but may contain an underscore. Please refer to http://en.wikipedia.org/wiki/List_of_tz_zones for a list of valid values. If multiple agencies are specified in the feed, each must have the same agency_timezone.
agency_lang          | Yes         | Contains a two-letter ISO 639-1 code for the primary language used by this agency. The language code is case-insensitive (both en and EN are accepted). This setting defines capitalization rules and other language-specific settings for all text contained in this transit agency's feed. Please refer to http://www.loc.gov/standards/iso639-2/php/code_list.php for a list of valid values.
agency_phone        | Yes         | Contains a single voice telephone number for the specified agency.
toll_program        | Optional         | Name of the tolling or managed lanes program at the agency.
toll_program_shortname        | Optional         | Abbreviated name of the tolling or managed lanes program.  
toll_program_url        | Optional         | Specifies the URL of a web page that allows a commuter to learn more about the tolling facliity. 
toll_operator        | Optional         | 
toll_authority        | Optional         | 
toll_backend		| Optional	|

Example:
```json
{
 "last_updated": 1502906828,
 "ttl": 60,
 "data": {
  "agency_id": "VTA",
  "agency_name": "Santa Clara Valley Transportation Authority",
  "agency_url": "http://www.vta.org",
  "agency_timezone": "America/Los_Angeles",
  "agency_lang": "EN",
  "agency_phone": "408-321-2300",
  "toll_program": "Silicon Valley Express Lanes",
  "toll_program_shortname": "SVEL",
  "toll_program_url": "http://www.vta.org/getting-around/using-express-lanes",
  "toll_operator": "VTA",
  "toll_authority": "VTA"
  "toll_backend": "BATA"
 }
}
```

### general_toll_info.json
Describes the system including System operator, URLs, contact info, time zone.  A JSON array of hours defined as follows:

Field Name          | Required    | Defines
--------------------| ------------| ----------
tolled_vehicles        | Yes         | 
tolling_methods        | Yes         | 
payment_options              | Yes         | 
toll_periods        | Yes         | 
minimum_toll          | Yes         | 
maximum_toll        | Yes         | 
toll_currency        | Optional         | 
toll_restrictions        | Optional         | 

Example:
```json
{
 "last_updated": 1502906828,
 "ttl": 60,
 "data":
 {
  "tolled_vehicles": "SOV",
  "tolling_methods": "transponder",
  "payment_options": "FasTrak",
  "toll_periods": "dynamic",
  "minimum_toll": "0.50",
  "maximum_toll": "7.00",
  "toll_currency": "USD",
  "toll_exemptions": [ "HOV2", "Motorbike", "transit", "greensticker", "whitesticker" ],
  "toll_restrictions": "none",
 }
}
```

### toll_periods.json
Describes the name and operational dates and times of the facilities

Field Name          | Required    | Defines
--------------------| ------------| ----------
facilities	|		| Array that contains one object per facility in the system as defined below
\- facility_id        | Yes         | 
\- name        | Yes         | 
\- hours_of_operation              | Yes         | 
\- direction        | Yes         | 
\- days_of_week          | Yes         | 
\- start_date        | Yes         | 
\- end_date          | Yes         | 


Example:
```json
{
 "last_updated": 1502829544,
 "ttl": 10,
 "data": {
  "facilities:": [
   {
    "facility_id": "CLW",
    "name": "Calaveras Westbound",
    "hours_of_operation": [[0500, 1000], [1500, 1900]],
    "direction": "westbound",
    "days_of_week": "MTWRF",
    "start_date": "20120325",
    "end_date": "20500101"
   },
   {
    "facility_id": "FSE",
    "name": "First Street Eastbound",
    "hours_of_operation": [[0500, 0900], [1500, 1900]],
    "direction": "northbound",
    "days_of_week": "MTWRF",
    "start_date": "20120325",
    "end_date": "20500101"
   }]
 }
}
```


### toll_status.json
Real time prices displayed on toll signs

Field Name            | Required  | Defines
--------------------- | ----------| ----------
facilities              | Yes       | Array that contains one object per facility in the system as defined below
\- facility_id          | Yes       | Unique identifier of a facility/plaza (see toll_facility.json)
\- interval_starting | Yes       | At which time the given price starts being live
\- pricing_module  | Yes  | Price displayed to users.  Possible values can be: a price or FREE TO ALL or HOV ONLY
\- message_module | Yes       | 
\- algorithm_mode  | Optional  | 
```json
{
  "last_updated": 1502829544,
  "ttl": 10,
  "data": {
    "facilities": [
      {
        "facility_id": "CLW",
        "interval_starting": 1463610000000,
        "pricing_module": "0.50",
        "message_module": "HOV 2+ NO TOLL",
        "algorithm_mode": "EL Speed"
      },
      {
        "facility_id": "FSE",
        "interval_starting": 1463610000000,
        "pricing_module": "2.50",
        "message_module": "HOV 2+ NO TOLL",
        "algorithm_mode": "EL Speed"
      }
    ]
  }
}
```

### toll_facility.json
Describes the lane types of the facility

Field Name          | Required    | Defines
--------------------| ------------| ----------
facilities        | Yes         | 
\- facility_id        | Yes         | 
\- facility_type        | Yes         | 
\- lane_type              | Yes         | 
\- facility_lane        | Yes         | 
\- facility_access          | Yes         | 

Example:
```json
{
 "last_updated": 1502906828,
 "ttl": 60,
 "data": {
  "facilities":[
   {
    "facility_id": "CLW",
    "facility_type": "Express Lanes",
    "lane_type": "HOV2+",
    "facility_lane": "Left Lane",
    "facility_access": "Lane Entry and Exit"
   },
   {
    "facility_id": "FSE",
    "facility_type": "Express Lanes",
    "lane_type": "HOV2+",
    "facility_lane": "Left Lane",
    "facility_access": "Lane Entry and Exit"
   }
  ]
 }
}
```


### toll_destination.json
Describes entrances and exits to the facility

Field Name          | Required    | Defines
--------------------| ------------| ----------
facilities        | Yes         | 
\- facility_id        | Yes         | 
\- destination_id        | Yes         | 
\- agency_url              | Yes         | 
\- CTOC_Entry_Plaza_ID        | Yes         | 
\- CTOC_Exit_Plaza_ID          | Yes         | 
\- toll_destination        | Yes         | 
\- destination_description        | Optional         | 
\- destination_type        | Optional         | 

Example:
```json
{
 "last_updated": 1502906828,
 "ttl": 60,
 "data": {
  "facilities": [
   {
    "facility_id": "CLW",
    "destination_id": "FSW", 
    "CTOC_Entry_Plaza_ID": "5110",
    "CTOC_Exit_Plaza_ID": "5111",
    "toll_destination": "North First St",
    "destination_description": "North First St, San Jose, CA",
    "destination_type": "Freeway"
   },
   {
    "facility_id": "FSE",
    "destination_id": "CLE", 
    "CTOC_Entry_Plaza_ID": "5118",
    "CTOC_Exit_Plaza_ID": "5119",
    "toll_destination": "I-880 North bound",
    "destination_description": "I-880 NB, Milpitas, CA",
    "destination_type": "Freeway"
   }
  ]
 }
}
```

### toll_signs_geom.json
A geojson file that gives points within a a FeatureCollection that describes where the physical toll signs are located

Field Name          | Required    | Defines
--------------------| ------------| ----------
facility_id        | Yes         | 
sign_id        | Yes         | 
geometry              | Yes         | 
sign_type        | Yes         | 
crs              | Yes         | 
toll_destination  | Yes         | 


Example:
[toll_signs_geom.json](draft_files/toll_signs_geom.json)

### facility_geom.json
A geojson file that is the centerline of the facility that would interact with vehicles.

Field Name          | Required    | Defines
--------------------| ------------| ----------
facility_id        | Yes         | 
properties        | Yes         | 
geometry              | Yes         | 
crs              | Yes         | 

Example:
[facility_geom.json](draft_files/facility_geom.json)

### gantry_geom.json
A geojson file that gives points within a a FeatureCollection that describes where the toll collection gantries are located

Field Name          | Required    | Defines
--------------------| ------------| ----------
facility_id        | Yes         | 
toll_gantry        | Yes         | 
geometry              | Yes         | 
crs              | Yes         | 


Example:
[gantry_geom.json](draft_files/gantry_geom.json)
