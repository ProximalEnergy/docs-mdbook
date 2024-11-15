# Plane of Array Irradiance

## General

The Plane of Array Irradiance (POAI) is the amount of irradiance that is incident upon the front face of the photovoltaic array.

This section of the documentation explains all of the different models and sub-models required to calculate POAI in the order that they are calculated in the simulation.

## Acronyms:
- Irradiance
  - **DNI**: Direct Normal Irradiance
  - **extraDNI**: Extraterrestrial Direct Normal Irradiance
  - **DHI**:  Diffuse Horizontal Irradiance
  - **POAI**: Plane of Array Irradiance
- Meteorological
 - **RH**: Relative Humidity

```mermaid
flowchart TD

  %% --- CLASSES ---
  classDef source fill:#6B7A8F, color:#CCCCCC
  classDef model fill:#202020, color:#CCCCCC
  classDef inputs fill:#1A1A1A, color:#CCCCCC
  classDef outputs fill:#B39245, color:#CCCCCC

  %% --- SOURCES ---
  met_station[(Met Station)]:::source
  met_station --> met_station_outputs

  met_station_outputs((
    time
    ambient_temperature
    global_horizontal_radiation
    relative_humidity
    wind_speed
   )):::outputs
  met_station_outputs --> solar_position_inputs
  met_station_outputs --> extraDNI_inputs

  pv_system[(PV System)]:::source
  pv_system --> pv_system_outputs

  pv_system_outputs((
    latitude
    longitude
    elevation
   )):::outputs
  pv_system_outputs --> calc_pressure_inputs
  pv_system_outputs --> solar_position_inputs

  %% --- ATMOSPHERIC PRESSURE ---
  calc_pressure_inputs[\elevation/]:::inputs
  calc_pressure_inputs --> calc_pressure

  calc_pressure[[
    pvlib.atmosphere
    .alt2pres
    ]]:::model
  calc_pressure --> calc_pressure_outputs

  calc_pressure_outputs((
    pressure
  )):::outputs

  %% --- SOLAR POSITION ---
  solar_position_inputs[\
    time
    ambient_temperature
    latitude
    longitude
    altitude
  /]:::inputs
  solar_position_inputs --> solar_position

  solar_position[[
    pvlib.solarposition
    .get_solarposition
    NREL_2008
    ]]:::model
  solar_position --> solar_position_outputs

  solar_position_outputs((
    apparent_zenith
    azimuth
  )):::outputs
  solar_position_outputs --> airmass_inputs
  solar_position_outputs --> DNI_inputs

  %% --- AIRMASS ---
  airmass_inputs[\
    apparent_zenith
  /]:::inputs
  airmass_inputs --> airmass

  airmass[[
    pvlib.atmosphere
    .get_relative_airmass
    KASTEN_YOUNG_1989
  ]]:::model
  airmass --> airmass_outputs

  airmass_outputs((
    airmass
    )):::outputs

  %% --- EXTRATERRESTRIAL DNI ---
  extraDNI_inputs[\
   time
   solar_constant=1360.8
   epoch_year=2014
  /]:::inputs
  extraDNI_inputs --> extraDNI

  extraDNI[[
    pvlib.irradiance
    .get_extra_radiation
    SPENCER
    ]]:::model
  extraDNI --> extraDNI_outputs

  extraDNI_outputs((
     extraterrestrial_DNI
     )):::outputs

  %% --- DNI ---

  DNI_inputs[\
    solar_zenith
    ghi
    pressure
    temp_dew
    use_delta_kt_prime=False,
    min_cos_zenith=0.065
    max_zenith=87
  /]:::inputs
  DNI_inputs --> DNI

  DNI[[
    pvlib.irradiance
    .dirint
  ]]:::model







```


## Edits and Additions

If you would like to see support for another algorithm or would like to suggest edits or additions to this documentation page, please open an issue on the [Proximal GitHub repository](https://github.com/ProximalEnergy/docs-mdbook).
