# Events

## Description

Events are a term used to describe any type of addressable underperformance of a project or its components. If a device is underperforming, the time frame is identified and energy production and financial impact is calculated. Failure Mode and Root Cause analysis are used to identify causes and remedies to each Event.

## Event Types

1. Project Offline
2. Device Offline
3. Device Underperforming\*
4. Tracker Underperforming
5. Curtailment\*
6. Soiling\*

\*These Event types are currently under development.

## Event Detection

Event detection is an automated process that identifies events heuristically. This process runs for each project every 15 minutes, and analyses the previous 1 hour of data. Every 6 hours, the day so far is re-processed to ensure completeness with backfilled data. Every 24 hours, the previous 3 days are re-processed for the same reason.

## Energy Production and Financial Impact

Once an Event is detected, the energy production and financial impact can be calculated by comparing the expected energy production against the actual energy production. Multiplying energy production impact by a project's PPA price gives the financial impact of the event. This occurs on every Event detection run.

## Parent-Child Relationships

To minimize the number of events detected, a device-wise hierarchy is used to group devices and identify dependent Events. For example if an inverter is offline, an Event will be opened for that inverter, but not for any of its constituent combiner boxes or trackers. This methodology ensures that any Events displayed are the highest addressable level, and prevents sending field technicians to fix a device that is not the root cause of the problem.
If an Event is detected on a child device before its parent device, the child Event remains open until the parent Event is closed. It is assumed in this case that the Events are distinct from each other, and that the parent device is not root cause of the child Event.

## Event Detection Filters

Event detection is valid only if a device could be reasonably expected to operate. To this end, Events are only detected if the average irradiance onsite is above 100 W/m^2^. This value is intended to minimize the number of Events associated with low-irradiance conditions such as late start Events. If the filters are not met during Event detection, each device is considered to be in the same state as previously known. If a device is repaired overnight, the Event will not be closed until the next day when the irradiance conditions are again valid.

## Failure Mode and Root Cause Analysis

The term "Failure Mode" is used to describe the first cause of an Event and is detectable from data analysis alone, while the term "Root Cause" is used to describe the underlying issue and is often not detectable from data.
For example, if an inverter begins to derate on a hot summer day, the failure mode may be temperature derating identified from the inverter's internal temperature sensor. The root cause may be a broken or dusty cooling fan, which is not detectable from data.

## Tracker Events

Tracker Events are detected by comparing the actual tracking pattern against an ideal tracking pattern. The ideal pattern takes both true-tracking and backtracking behaviors into account. Typically, tracker rows are assigned Events if the issue is localized to a single tracker row. If all tracker rows in a tracker zone are not tracking at the same time, the Event is elevated to a tracker zone Event. Tracker stow information is displayed in an Event's failure mode.
