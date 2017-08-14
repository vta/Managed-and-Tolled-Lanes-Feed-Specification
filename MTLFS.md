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
