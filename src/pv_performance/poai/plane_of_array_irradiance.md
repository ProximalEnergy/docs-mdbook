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
classDiagram
    System : latitude
    System : longitude
    System : elevation
    System : albedo

    Met Station: time
    Met Station: global horizontal irradiance
    Met Station: relative humidity
    Met Station: ambient temperature

    System --> Site Pressure
    Site Pressure: site pressure
    Site Pressure: portland_state_aerospace_society()

    System --> Solar Position
    Met Station --> Solar Position
    Solar Position: apparent zenith
    Solar Position: azimuth
    Solar Position: zenith
    Solar Position: nrel_2008()

    Solar Position --> Airmass
    Airmass: airmass

    Met Station --> Extraterrestrial DNI
    Extraterrestrial DNI: extraDNI
    Extraterrestrial DNI: spencer()

    Met Station --> Dew Point
    Dew Point: dew point temperature
    Dew Point: magnus_tetens()

    Met Station --> Decomposition Model
    Solar Position --> Decomposition Model
    Site Pressure --> Decomposition Model
    Dew Point --> Decomposition Model
    Decomposition Model: direct normal irradiance
    Decomposition Model: dirint()

    Met Station --> Geometric Model
    Decomposition Model --> Geometric Model
    Geometric Model: diffuse horizontal irradiance

```


## Edits and Additions

If you would like to see support for another algorithm or would like to suggest edits or additions to this documentation page, please open an issue on the [Proximal GitHub repository](https://github.com/ProximalEnergy/docs-mdbook).
