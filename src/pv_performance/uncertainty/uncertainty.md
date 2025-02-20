# Measurement Uncertainty

## General
All data in the Proximal platform is inherently uncertain.  This is due to the fact that sensors have inherent uncertainty and models have uncertainty on top of that.  The following tables are a reference for users to use in order to understand the data and models being reported by the platform.

## Defaults
By default all reported values below are:
- 95% Uncertainty (Expanded) (U)

## Irradiance Sensor Uncertainty

| Sensor                  | Measurement Uncertainty        | Reference            |
|-------------------------|--------------------------------|----------------------|
| Class A Pyranometer     | ±2% (daily total absolute)     | [1](uncertainty#References)  |
| Class A Pyranometer     | ±3% (hourly total absolute)    | [1](uncertainty#References)  |

## Soiling Sensor Uncertainty

Looking at the image, I notice there are actually two tables. I'll help you format the first table (the system configuration table) in the markdown format you showed, since it contains more complete information:

| Sensor                  | Measurement Uncertainty | Reference            |
|-------------------------|-------------------------|----------------------|
| Optical                 | ±4-7% (relative)        | [2](uncertainty#References)              |
| Cell-Cell               | ±4-7% (relative)        | [2](uncertainty#References)              |
| Cell-Module (power)     | ±1-2% (relative)        | [2](uncertainty#References)              |
| Cell-Module (current)   | ±3-5% (relative)        | [2](uncertainty#References)              |
| Module-Module           | ±4-7% (relative)        | [2](uncertainty#References)              |
| Cell-Cell               | ±4-7% (relative)        | [2](uncertainty#References)              |


### Notes
- Optical sensors may not capture the IAM effects of soiling
- Cell-Cell sensors will not capture the effects of non-uniform soiling
- Cell-Cell sensors may have a different glass than the install PV modules and may soil differently

## DC Electrical Sensor Uncertainty

| Sensor                  | Measurement Uncertainty | Reference            |
|-------------------------|-------------------------|----------------------|
| DC Combiner              | ±?% (daily total absolute)       | [None](uncertainty#References)  |



# References
1. [WMO 1.7-12](https://www.weather.gov/media/epz/mesonet/CWOP-WMO8.pdf)
1. [Atonometrics Whitepaper](https://www.atonometrics.com/wp-content/uploads/2023/02/White-Paper-Specifying-a-Soiling-Measurement-System.pdf)
