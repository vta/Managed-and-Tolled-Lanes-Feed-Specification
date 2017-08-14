# Managed and Tolled Lanes Feed Specification (MTLFS)
This document explains the types of files and data that comprise the Managed and Tolled Lanes Feed Specification (MTLFS) and defines the fields used in all of those files.

## Table of Contents
* [Files](#files)


## Files
This specification defines the following files along with their associated content:

File Name                   | Required      | Defines
--------------------------- | ------------  | ----------
MTLFS.json                  | Optional      | Auto-discovery file that links to all of the other files published by the system. This file is optional, but highly recommended.
general_toll_info.json      | Yes           | Describes toll payment methods, min and max tolls, and restrictions
toll_destination.json       | Yes           | Describes entrances and exits to the facility
toll_facility.json          | Optional      | Describes the lane types of the facility
toll_periods.json           | Optional      | Describes the name and operational dates and times of the facilities
toll_signs.json             | Optional      | Describes the regions the system is broken up into
toll_authority_info.json    | Yes           | Describes the system including System operator, URLs, contact info, time zone

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
```

### toll_destination.json
Describes the system including System operator, URLs, contact info, time zone.  A JSON array of hours defined as follows:

Field Name          | Required    | Defines
--------------------| ------------| ----------
facility_id        | Yes         | 
destination_id        | Yes         | 
agency_url              | Yes         | 
CTOC_Entry_Plaza_ID        | Yes         | 
CTOC_Exit_Plaza_ID          | Yes         | 
toll_destination        | Yes         | 
destination_description        | Optional         | 
destination_type        | Optional         | 

Example:
```json

[{
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
}]
```
