# PV Model Overview

## Why Proximal

The Proximal PV performance model has been specifically designed with operating assets in mind.  The following are a few highlights of the model that distinguish it from other PV models used in asset management.

<div class="feature-grid">
  <div class="feature-card">
    <div class="feature-icon-logo">
        <img
        src="./assets/pvlib.webp"
        alt="pvlib-logo"
        />
    </div>
    <h3 class="feature-title">Built on Open Source</h3>
    <p class="feature-description">
        Modern, tested, transparent, trusted by the community.  By building on top of pvlib, we can ensure that the proximal energy model is understandable by all users and easily extendable.
    </p>
  </div>

  <div class="feature-card">
    <div class="feature-icon">⑃</div>
    <h3 class="feature-title">Nodal</h3>
    <p class="feature-description">
        Expected energy can be calculated at the combiner, inverter, array, or substation level.
    </p>
  </div>

  <div class="feature-card">
    <div class="feature-icon">🕑</div>
    <h3 class="feature-title">Sub-Hourly</h3>
    <p class="feature-description">
        Operating assets emit high frequency data which is captured by the energy model.
    </p>
  </div>

  <div class="feature-card">
    <div class="feature-icon">🗺️</div>
    <h3 class="feature-title">Geospatial</h3>
    <p class="feature-description">
        Each block of the system uses the meteorological data that is closest to it which can yield great improvements for large systems with passing clouds.
    </p>
  </div>

  <div class="feature-card">
    <div class="feature-icon">📄</div>
    <h3 class="feature-title">Documented</h3>
    <p class="feature-description">
        Each step of the model is documented in detail through pvlib and through this documentation set.
    </p>
  </div>

  <div class="feature-card">
    <div class="feature-icon">♲</div>
    <h3 class="feature-title">Efficient</h3>
    <p class="feature-description">
        Inputs to each individual model are automatically factored into unique combinations.  This makes the model capable of handling large systems with high levels of detail at high frequency possible.
    </p>
  </div>
</div>


### Model Chain
The proximal performance model at a high level is comprised of 6 major sub-models.  Clicking on any of the models will take you to the actual model steps that occur in each sub-model.


```mermaid
flowchart TD

%% --- CLASSES ---
classDef source fill:#6B7A8F, color:#CCCCCC
classDef model fill:#202020, color:#CCCCCC
classDef model_dashed fill:#202020, color:#CCCCCC, stroke-dasharray: 5 5
classDef inputs fill:#1A1A1A, color:#CCCCCC
classDef outputs fill:#B39245, color:#CCCCCC

met_params[Meteorological Parameters]:::source
click met_params "./meteorological_parameters.html"

tracker_rotation_angles[Tracker Rotation Angles]:::source
click tracker_rotation_angles "./tracker_rotation_angles.html"

poai[Plane of Array Irradiance]:::source
click poai "./plane_of_array_irradiance.html"

epoai[
    Effective
    Plane of Array Irradiance
]:::source
click epoai "./effective_plane_of_array_irradiance.html"

DC[DC System Losses]:::source
click DC "dc_system_losses.html"

AC[AC System Losses]:::source
click AC "ac_system_losses.html"

met_params --> tracker_rotation_angles
tracker_rotation_angles --> poai
poai --> epoai
epoai --> DC
DC --> AC
```



## Edits and Additions

If you would like to see support for another algorithm or would like to suggest edits or additions to this documentation page, please open an issue on the [Proximal GitHub repository](https://github.com/ProximalEnergy/docs-mdbook).
