# Meteorological Parameters

## General

The first step of the Proximal expected energy simulation is to ingest data from the project meteorological stations.  This data is then used to calculate the intermediate meteorological parameters that are required to calculate the Plane of Array Irradiance (POAI).

This section of the documentation shows all models in the order that they are calculated in the simulation.

## Acronyms:
- **DHI**:  Diffuse Horizontal Irradiance
- **DNI**: Direct Normal Irradiance
- **extraDNI**: Extraterrestrial Direct Normal Irradiance
- **PWAT**: Precipitable Water
- **RH**: Relative Humidity

## Simulation Pipeline
The following flow diagram shows how meteorological parameters is calculated in the Proximal expected energy simulation.  The flow chart is meant to be interactive.  Clicking on any of the modeling step nodes will take you to the documentation for that modeling step.

You may need to zoom in to be able to better see all of the details in the flow chart.

### Legend
```mermaid
  flowchart LR

  %% --- CLASSES ---
  classDef source fill:#6B7A8F, color:#CCCCCC
  classDef model fill:#202020, color:#CCCCCC
  classDef inputs fill:#1A1A1A, color:#CCCCCC
  classDef outputs fill:#B39245, color:#CCCCCC

  database[(Database)]:::source
  model_step[[
    Modeling Step
    DEFAULT MODEL CHOICE
  ]]:::model
  model_inputs[\
    Input Parameters
    for Modeling Step
  /]:::inputs
  model_outputs([Calculated Parameters]):::outputs

  database --> model_inputs
  model_inputs --> model_step --> model_outputs --> model_inputs

```

### Model Chain
```mermaid
flowchart TD

  %% --- CLASSES ---
  classDef source fill:#6B7A8F, color:#CCCCCC
  classDef model fill:#202020, color:#CCCCCC
  classDef model_dashed fill:#202020, color:#CCCCCC, stroke-dasharray: 5 5
  classDef inputs fill:#1A1A1A, color:#CCCCCC
  classDef outputs fill:#B39245, color:#CCCCCC

  %% --- SOURCES ---

  pv_system[(
    --- PV SYSTEM ---
    elevation
  )]:::source
  pv_system --> calc_pressure_inputs

  met_station[(
    --- MET STATION ---
    time
    ambient_temperature
    global_horizontal_radiation
    relative_humidity
    wind_speed
    *albedo
  )]:::source
  met_station --> TDEW_inputs
  met_station --> solar_position_inputs
  met_station --> extraDNI_inputs
  met_station --> DHI_inputs

  %% --- ATMOSPHERIC PRESSURE ---
  calc_pressure_inputs[\elevation/]:::inputs
  calc_pressure_inputs --> calc_pressure

  calc_pressure[[
    pvlib.atmosphere
    .alt2pres
    ]]:::model
  calc_pressure --> calc_pressure_outputs
  click calc_pressure "https://pvlib-python.readthedocs.io/en/stable/reference/generated/pvlib.atmosphere.alt2pres.html"

  calc_pressure_outputs([
    pressure
  ]):::outputs
  calc_pressure_outputs --> DNI_inputs

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
  click solar_position "https://pvlib-python.readthedocs.io/en/stable/reference/generated/pvlib.solarposition.get_solarposition.html#pvlib.solarposition.get_solarposition"

  solar_position_outputs([
    apparent_zenith
    azimuth
  ]):::outputs
  solar_position_outputs --> DNI_inputs
  solar_position_outputs --> DHI_inputs
  solar_position_outputs --> airmass_inputs

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
  click airmass "https://pvlib-python.readthedocs.io/en/stable/reference/generated/pvlib.location.Location.get_airmass.html#pvlib.location.Location.get_airmass"

  airmass_outputs([
    airmass
    ]):::outputs

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
  click extraDNI "https://pvlib-python.readthedocs.io/en/stable/reference/generated/pvlib.irradiance.get_extra_radiation.html#pvlib.irradiance.get_extra_radiation"

  extraDNI_outputs([
    extraterrestrial_DNI
    ]):::outputs

  %% --- TDEW ---

  TDEW_inputs[\
    relative_humidity
  /]:::inputs
  TDEW_inputs --> TDEW

  TDEW[[
    pvlib.atmosphere
    .tdew_from_rh
    MAGNUS_TETENS
    ]]:::model_dashed
  TDEW --> TDEW_outputs
  click TDEW "https://pvlib-python.readthedocs.io/en/stable/reference/generated/pvlib.atmosphere.tdew_from_rh.html#pvlib.atmosphere.tdew_from_rh"

  TDEW_outputs([
    temp_dew_point
    ]):::outputs
  TDEW_outputs --> DNI_inputs

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
    DIRINT
  ]]:::model
  DNI --> DNI_outputs
  click DNI "https://pvlib-python.readthedocs.io/en/stable/reference/generated/pvlib.irradiance.dirint.html#pvlib.irradiance.dirint"

  DNI_outputs([
    DNI
  ]):::outputs
  DNI_outputs --> DHI_inputs

  %% --- DHI ---
  DHI_inputs[\
    GHI
    DNI
    solar_zenith
  /]:::inputs
  DHI_inputs --> DHI

  DHI[[
    pvlib.irradiance
    .complete_irradiance
    GEOMETRIC
  ]]:::model
  DHI --> DHI_outputs
  click DHI "https://pvlib-python.readthedocs.io/en/stable/reference/generated/pvlib.irradiance.complete_irradiance.html#pvlib.irradiance.complete_irradiance"

  DHI_outputs([
    DHI
    ]):::outputs
  ```


## Edits and Additions

If you would like to see support for another algorithm or would like to suggest edits or additions to this documentation page, please open an issue on the [Proximal GitHub repository](https://github.com/ProximalEnergy/docs-mdbook).
