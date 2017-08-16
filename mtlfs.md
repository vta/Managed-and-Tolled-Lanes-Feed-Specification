# Managed and Tolled Lanes Feed Specification (MTLFS)
This document explains the types of files and data that comprise the Managed and Tolled Lanes Feed Specification (MTLFS) and defines the fields used in all of those files.

## Table of Contents
* [Files](#files)
* [Output Format](#output-format)
* Field Definitions
    * [mtlfs.json](#mtlfsjson)
    * [toll_authority_info.json](#toll_authority_infojson)
    * [general_toll_info.json](#general_toll_infojson)
    * [toll_signs.json](#toll_signsjson)
    * [toll_periods.json](#toll_periodsjson)
    * [toll_status.json](#toll_statusjson)
    * [toll_facility.json](#toll_facilityjson)
    * [toll_destination.json](#toll_destinationjson)

## Files
This specification defines the following files along with their associated content:

File Name                   | Required      | Defines
--------------------------- | ------------  | ----------
mtlfs.json                  | Optional      | Auto-discovery file that links to all of the other files published by the system. This file is optional, but highly recommended.
toll_authority_info.json    | Yes           | Describes the system including System operator, URLs, contact info, time zone
general_toll_info.json      | Yes           | Describes toll payment methods, min and max tolls, and restrictions
toll_signs.json             | Optional      | Describes where the physical toll signs are located
toll_periods.json           | Optional      | Describes the name and operational dates and times of the facilities
toll_status.json           | Optional      | Provides realtime info of the prices and messaging of the toll signs
toll_facility.json          | Optional      | Describes the lane types of the facility
toll_destination.json       | Yes           | Describes entrances and exits to the facility

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
  "data": {...}
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
agency_id        | Yes         | 
agency_name        | Yes         | 
agency_url              | Yes         | 
agency_timezone        | Yes         | 
agency_lang          | Yes         | 
agency_phone        | Yes         | 
toll_program        | Optional         | 
toll_program_shortname        | Optional         | 
toll_program_url        | Optional         | 
toll_operator        | Optional         | 
toll_authority        | Optional         | 

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
  "toll_authority": "BATA"
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

### toll_signs.json
Describes where the physical toll signs are located

Field Name          | Required    | Defines
--------------------| ------------| ----------
facility_id        | Yes         | 
sign_id        | Yes         | 
geometry              | Yes         | 
sign_type        | Yes         | 
toll_destination          | Yes         | 

Example:
```json
[{
 "facility_id": "CLW",
 "sign_id": "750",	 
 "geometry": {
	  "type": "Point",
	  "coordinates": [-121.92073, 37.44290]
	},
 "sign_type": ["Dynamic Toll Rate", "Dynamic Messages"],
 "toll_destination" : "North First St"
},
{
 "facility_id": "FSE",
 "sign_id": "250",	 
 "geometry": {
	  "type": "Point",
	  "coordinates": [-121.94227, 37.41967]
	},
 "sign_type": ["Dynamic Toll Rate","Dynamic Messages"],
 "toll_destination" : "I-880 NB"
}]
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
