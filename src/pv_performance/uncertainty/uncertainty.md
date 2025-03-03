# Measurement Uncertainty

## General
All data in the Proximal platform is inherently uncertain.  This is due to the fact that sensors have inherent uncertainty and models have uncertainty on top of that.  The following tables are a reference for users to use in order to understand the data and models being reported by the platform.

## Defaults
By default all reported values below are:
- 95% Uncertainty (Expanded) (U)

## Irradiance Sensor Uncertainty

| Sensor                  | Measurement Uncertainty        | Reference            |
|-------------------------|--------------------------------|----------------------|
| Class A Pyranometer     | ±2% (daily total absolute)     | [1](#1)  |
| Class A Pyranometer     | ±3% (hourly total absolute)    | [1](#1)  |

## Soiling Sensor Uncertainty

| Sensor                  | Measurement Uncertainty | Reference            |
|-------------------------|-------------------------|----------------------|
| Optical                 | ±4-7% (relative)        | [2](#2)              |
| Cell-Cell               | ±4-7% (relative)        | [2](#2)              |
| Cell-Module (power)     | ±1-2% (relative)        | [2](#2)              |
| Cell-Module (current)   | ±3-5% (relative)        | [2](#2)              |
| Module-Module           | ±4-7% (relative)        | [2](#2)              |
| Cell-Cell               | ±4-7% (relative)        | [2](#2)              |


### Notes
- Optical sensors may not capture the IAM effects of soiling
- Cell-Cell sensors will not capture the effects of non-uniform soiling
- Cell-Cell sensors may have a different glass than the install PV modules and may soil differently

## Inverter

| Sensor                  | Measurement Uncertainty | Reference            |
|-------------------------|-------------------------|----------------------|
| Class A Current Input   | ±2% (absolute)          | [3](#3)              |
| Class A Voltage Input   | ±2% (absolute)          | [3](#3)              |
| Class A Power Input     | ±3% (absolute)          | [3](#3)              |
| Class A Current Output  | ±2% (absolute)          | [3](#3)              |
| Class A Voltage Output  | ±2% (absolute)          | [3](#3)              |
| Class A Power Output    | ±3% (absolute)          | [3](#3)              |

## Meter

| Sensor                  | Measurement Uncertainty | Reference            |
|-------------------------|-------------------------|----------------------|
| Meter                   | ±0.2% (absolute)        | [4](#4)              |

# References

1. [WMO 1.7-12](https://www.weather.gov/media/epz/mesonet/CWOP-WMO8.pdf)<a id="1"></a>
1. [Atonometrics Whitepaper](https://www.atonometrics.com/wp-content/uploads/2023/02/White-Paper-Specifying-a-Soiling-Measurement-System.pdf)<a id="2"></a>
1. [IEC 61724-1:2021 Section 11.1](https://webstore.iec.ch/en/publication/65561)<a id="3"></a>
1. [IEC 62053-22:2020](https://webstore.iec.ch/en/publication/29987)<a id ="4"></a>
