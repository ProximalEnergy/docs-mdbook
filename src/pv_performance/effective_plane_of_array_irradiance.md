# Effective Irradiance

## General

The Effective Plane of Array Irradiance (EPOAI) is the amount of irradiance that is incident upon the active cell area of the photovoltaic array.  This is differentiated from the Plane of Array Irradiance (POAI) which is the amount of irradiance that is incident upon the front face of the photovoltaic array glass.

Conceptually, losses calculated while calculating the effective plane of array irradiance are generally calculated in the same order as the physical losses that occur between the sun's rays and the cell active material.  For example, soiling loss on top of the module glass is calculated before the incidence angle effect inside of the module glass.  On the other hand, some losses such as spectral correction, which are actually material science losses are calculated empirical losses in effective plane of array irradiance.

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
    soiling_loss
  )]:::source
  met_station --> calc_soiling_inputs

  pv_system[(
    --- PV SYSTEM ---
    module:
      anti-reflection coating
      glass thickness: L
  )]:::source
  pv_system --> calc_refractive_index
  pv_system --> calc_aoi_inputs

  subgraph calculated in previous step
    %% --- POAI ---
    poai([
      poai
    ]):::outputs

    %% --- SURFACE ANGLES ---
    surface_angles([
      surface_tilt
    ]):::outputs
  end

  poai --> calc_shading_inputs
  pv_system --> calc_shading_inputs
  surface_angles --> calc_shading_inputs

  %% --- SHADING ---
  calc_shading_inputs[\
    module
    surface_tilt
    poai
    /]:::inputs
  calc_shading_inputs --> calc_shading

  calc_shading[[
    pvlib.shading
    PLACEHOLDER
    ]]:::model
  calc_shading --> calc_shading_outputs

  calc_shading_outputs([
    1_poai_less_shading
  ]):::outputs
  calc_shading_outputs --> calc_soiling_inputs

  %% --- SOILING ---
  calc_soiling_inputs[\
    measured_soiling_loss
    /]:::inputs
    calc_soiling_inputs --> calc_soiling

  calc_soiling[[
    proximal.soiling
    RATIO
    ]]:::model
  calc_soiling --> calc_soiling_outputs

  calc_soiling_outputs([
    2_poai_less_soiling
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
    2_poai_less_soiling
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
    3_poai_less_aoi
  ]):::outputs


  ```




## Edits and Additions

If you would like to see support for another algorithm or would like to suggest edits or additions to this documentation page, please open an issue on the [Proximal GitHub repository](https://github.com/ProximalEnergy/docs-mdbook).
