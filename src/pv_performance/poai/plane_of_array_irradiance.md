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

  classDef source fill:#E6F3FF
  classDef data fill:#e8f5e9
  classDef model fill:#FFFFFF

  met_station(Met Station):::source

  pv_system(PV System):::source
  pv_system --> calc_pressure_inputs

  calc_pressure_inputs{{elevation}}:::data
  calc_pressure_inputs --> calc_pressure

  calc_pressure[pvlib.atmosphere.alt2pres]:::model

```


## Edits and Additions

If you would like to see support for another algorithm or would like to suggest edits or additions to this documentation page, please open an issue on the [Proximal GitHub repository](https://github.com/ProximalEnergy/docs-mdbook).
