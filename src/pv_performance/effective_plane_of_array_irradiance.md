# Effective Irradiance

## General

The Effective Plane of Array Irradiance (EPOAI) is the amount of irradiance that is incident upon the active cell area of the photovoltaic array.  This is differentiated from the Plane of Array Irradiance (POAI) which is the amount of irradiance that is incident upon the front face of the photovoltaic array glass.

Conceptually, losses calculated while calculating the effective plane of array irradiance are generally calculated in the same order as the physical losses that occur between the sun's rays and t`he cell active material.  For example, soiling loss on top of the module glass is calculated before the incidence angle effect inside of the module glass.  On the other hand, some losses such as spectral correction, which are actually material science losses are calculated empirical losses in effective plane of array irradiance.

## Acronyms:
- Irradiance
  - **POAI**: Plane of Array Irradiance
-  **EPOAI**: Effective Plane of Array Irradiance



## Simulation Pipeline
The following flow diagram shows how the Effective Plane of Array Irradiance is calculated in the Proximal expected energy simulation.  The flow chart is meant to be interactive.  Clicking on any of the modeling step nodes will take you to the documentation for that modeling step.

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

  met_station[(
    --- MET STATION ---
    time
    solar_zenith
    solar_azimuth
    soiling_loss
    ambient temperature
    relative_humidity
  )]:::source
  met_station --> calc_projected_zenith_inputs
  met_station --> calc_p_wat_inputs

  met_station --> calc_soiling_inputs

  pv_system[(
    --- PV Modules ---
    anti-reflection coating
    glass thickness: L
    spectral coefficients
  )]:::source
  pv_system --> calc_projected_zenith_inputs
  pv_system --> calc_shading_inputs
  pv_system --> calc_aoi_inputs
  pv_system --> calc_refractive_index
  pv_system --> calc_spectral_inputs

  subgraph calculated in previous step
    %% --- POAI ---
    poai([
      poai
    ]):::outputs

    %% --- SURFACE ANGLES ---
    surface_angles([
      surface_tilt
    ]):::outputs

    %% --- Axis Tilts
    axis_tilt([
      axis_tilt
    ]):::outputs

    %% --- Axis Azimuths
    axis_azimuth([
      axis_azimuth
    ]):::outputs
  end

  poai --> calc_shading_inputs
  surface_angles --> calc_shading_inputs
  axis_azimuth --> calc_projected_zenith_inputs
  axis_tilt --> calc_projected_zenith_inputs


  %% --- SHADING ---
  calc_projected_zenith_inputs[\
    solar_zenith
    solar_azimuth
    axis_tilt
    axis_azimuth
    /]:::inputs
  calc_projected_zenith_inputs --> calc_projected_zenith

  calc_projected_zenith[[
    pvlib.shading
    .projected_solar
    _zenith_angle
  ]]
  calc_projected_zenith --> calc_projected_zenith_outputs

  calc_projected_zenith_outputs([
    projected_zenith
  ])
  calc_projected_zenith_outputs -->
  determine_direction

  determine_direction{
    projected_zenith
  }
  determine_direction -->|zenith < 0| shade_fraction_1
  determine_direction -->|zenith > 0| shade_fraction_2

  shade_fraction_1([
    shade_fraction = 1
  ]):::outputs
  shade_fraction_1 --> calc_shading_inputs

  shade_fraction_2([
    shade_fraction = 0
  ]):::outputs
  shade_fraction_2 --> calc_shading_inputs

  calc_shading_inputs[\
    module
    surface_tilt
    poai
    /]:::inputs
  calc_shading_inputs --> calc_shading

  calc_shading[[
    pvlib.shading
    shaded_fraction1d
    ]]:::model
  calc_shading --> calc_shading_outputs

  calc_shading_outputs([
    p1:
    poai - shading
  ]):::outputs
  calc_shading_outputs --> calc_soiling_inputs

  %% --- SOILING ---
  calc_soiling_inputs[\
    p1
    measured_soiling_loss
    /]:::inputs
    calc_soiling_inputs --> calc_soiling

  calc_soiling[[
    proximal.soiling
    RATIO
    ]]:::model
  calc_soiling --> calc_soiling_outputs

  calc_soiling_outputs([
    p2:
    p1 - soiling
  ]):::outputs
  calc_soiling_outputs --> calc_aoi_inputs

  %% --- REFRACTIVE INDEX ---
  calc_refractive_index{
    module has
    anti-reflection coating
  }
  calc_refractive_index -->|yes| refractive_index_standard
  calc_refractive_index -->|no| refractive_index_ar

  refractive_index_standard([
    n = 1.526
  ]):::outputs
  refractive_index_standard --> calc_aoi_inputs

  refractive_index_ar([
    n = 1.290
  ]):::outputs
  refractive_index_ar --> calc_aoi_inputs


  %% --- AOI ---
  calc_aoi_inputs[\
    p2
    angle_of_incidence
    refractive_index: n
    K=4.0
    glass_thickness: L
    /]:::inputs
  calc_aoi_inputs --> calc_aoi

  calc_aoi[[
    pvlib.iam
    .physical
    ]]:::model
  calc_aoi --> calc_aoi_outputs

  calc_aoi_outputs([
    p3:
    p2 - aoi
  ]):::outputs
  calc_aoi_outputs --> calc_spectral_inputs

  %% --- SPECTRAL ---
  calc_p_wat_inputs[\
    ambient temperature
    relative humidity
    /]:::inputs
  calc_p_wat_inputs --> calc_p_wat

  calc_p_wat[[
      pvlib.atmosphere
      .gueymard94_pw
  ]]:::model
  calc_p_wat --> calc_p_wat_outputs

  calc_p_wat_outputs([
    precipitable water
   ]):::outputs
  calc_p_wat_outputs --> calc_spectral_inputs

  calc_spectral_inputs[\
    p3
    precipitable water
    airmass absolute
    spectral coefficients
    min_p_wat=0.1
    max_p_wat=8.0
    min_airmass_abs=0.58
    max_airmass_abs=10.0
    /]:::inputs
  calc_spectral_inputs --> calc_spectral

  calc_spectral[[
    pvlib.spectral
    .spectral
    ]]:::model
  calc_spectral --> calc_spectral_outputs

  calc_spectral_outputs([
    effective poai
  ]):::outputs

  ```




## Edits and Additions

If you would like to see support for another algorithm or would like to suggest edits or additions to this documentation page, please open an issue on the [Proximal GitHub repository](https://github.com/ProximalEnergy/docs-mdbook).
