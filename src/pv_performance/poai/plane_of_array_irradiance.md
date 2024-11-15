# Plane of Array Irradiance

## General

The Plane of Array Irradiance (POAI) is the amount of irradiance that is incident upon the front face of the photovoltaic array.

This section of the documentation explains all of the different models and sub-models required to calculate POAI in the order that they are calculated in the simulation.

## Acronyms:
- Irradiance
  - **extraDNI**: Extraterrestrial Direct Normal Irradiance
  - **DNI**: Direct Normal Irradiance
  - **DHI**:  Diffuse Horizontal Irradiance
  - **RHI**:  Reflected Horizontal Irradiance
  - **POAI**: Plane of Array Irradiance
- Meteorological
 - **RH**: Relative Humidity



## Simulation Pipeline
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

  database --> model_outputs
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
  met_station[(
    --- MET STATION ---
    time
    ambient_temperature
    global_horizontal_radiation
    relative_humidity
    wind_speed
    *albedo
  )]:::source

  met_station --> extraDNI_inputs
  met_station --> RHI_inputs
  met_station --> DHI_inputs
  met_station --> TDEW_inputs
  met_station --> solar_position_inputs

  pv_system[(
    --- PV SYSTEM ---
    latitude
    longitude
    elevation
    modules
    trackers
    combiner boxes
    inverters
    transformers
    plant controller
  )]:::source
  pv_system --> solar_position_inputs
  pv_system --> calc_pressure_inputs
  pv_system --> tracker_rotation_angles_inputs
  pv_system --> surface_angle_inputs

  %% --- ATMOSPHERIC PRESSURE ---
  calc_pressure_inputs[\elevation/]:::inputs
  calc_pressure_inputs --> calc_pressure

  calc_pressure[[
    pvlib.atmosphere
    .alt2pres
    ]]:::model
  calc_pressure --> calc_pressure_outputs
  click calc_pressure "site_pressure.html" "site_pressure"

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

  solar_position_outputs([
    apparent_zenith
    azimuth
  ]):::outputs
  solar_position_outputs --> DNI_inputs
  solar_position_outputs --> DHI_inputs
  solar_position_outputs --> airmass_inputs
  solar_position_outputs --> tracker_rotation_angles_inputs

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

  airmass_outputs([
    airmass
    ]):::outputs
  airmass_outputs --> POAI_inputs

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

  extraDNI_outputs([
     extraterrestrial_DNI
     ]):::outputs
  extraDNI_outputs --> POAI_inputs

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

  DNI_outputs([
    DNI
  ]):::outputs
  DNI_outputs --> POAI_inputs
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

  DHI_outputs([
    DHI
    ]):::outputs
  DHI_outputs --> POAI_inputs

  %% --- RHI ---
  RHI_inputs[\
    GHI
    albedo
    /]:::inputs
  RHI_inputs --> RHI

  RHI[[
    proximal.ratio
    RATIO
  ]]:::model
  RHI --> RHI_outputs

  RHI_outputs([
    RHI
    ]):::outputs
  RHI_outputs --> front_reflected_inputs

  %% --- Tracker Rotation Angles ---
  tracker_rotation_angles_inputs[\
    apparent_zenith
    azimuth
    tracker_tilt
    tracker_azimuth
    tracker_max_angle
    tracking_type
    gcr
    /]:::inputs
  tracker_rotation_angles_inputs --> tracker_rotation_angles

  tracker_rotation_angles[[
    pvlib.tracking
    .single_axis
    ANDERSON_MIKOFSKI_2020
    ]]:::model
  tracker_rotation_angles --> tracker_rotation_angles_outputs

  tracker_rotation_angles_outputs([
    tracker_rotation_angle
    ]):::outputs
  tracker_rotation_angles_outputs --> surface_angle_inputs

  %% --- surface angles ---
  surface_angle_inputs[\
    tracker_rotation_angle
    axis_tilt
    axis_azimuth
   /]:::inputs
  surface_angle_inputs --> surface_angles

  surface_angles[[
    pvlib.tracking
    .calc_surface_orienation
    ]]:::model
  surface_angles --> surface_angle_outputs

  surface_angle_outputs([
    surface_tilt
    surface_azimuth
    ]):::outputs
  surface_angle_outputs --> POAI_inputs

  %% --- Reflection on Front Side ---
  front_reflected_inputs[\
    surface_tilt
    surface_azimuth
    rhi
    /]:::inputs
  front_reflected_inputs --> front_reflected

  front_reflected[[
    proximal.front_reflected
    ]]:::model_dashed
  front_reflected --> front_reflected_outputs

  front_reflected_outputs([
    front_reflected
    ]):::outputs
  front_reflected_outputs --> POAI_inputs

  %% --- POAI ---
  POAI_inputs[\
    surface_tilt
    surface_azimuth
    dhi
    dni
    front_reflected
    extraterrestrial_dni
    apparent_zenith
    solar_azimuth
    airmass
    /]:::inputs
  POAI_inputs --> POAI

  POAI[[
    proximal.poai
    POAI
  ]]:::model
  POAI --> POAI_outputs

  POAI_outputs([
    POAI
    ]):::outputs
  ```


## Edits and Additions

If you would like to see support for another algorithm or would like to suggest edits or additions to this documentation page, please open an issue on the [Proximal GitHub repository](https://github.com/ProximalEnergy/docs-mdbook).
