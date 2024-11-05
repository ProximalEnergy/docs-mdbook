# Plane of Array Irradiance

## General

The Plane of Array Irradiance (POAI) is the amount of irradiance that is incident upon the front face of the photovoltaic array.

This section of the documentation explains all of the different models and sub-models required to calculate POAI in the order that they are calculated in the simulation.

## Acronyms:
- **DNI**: Direct Normal Irradiance
- **DHI**:  Diffuse Horizontal Irradiance
- **POAI**: Plane of Array Irradiance

## Steps to Calculate POAI

| Step | Algorithm |
|------|-----------|
| 1    | Calculate Site Pressure |
| 2    | Calculate Solar Position |
| 3    | Calculate Air Mass |
| 4    | Calculate Extraterrestrial DNI |
| 5    | Calculate DNI |
| 6    | Calculate DHI |
| 7    | Calculate Irradiance Reflected onto Front Face |
| 8    | Calculate Horizon Loss |
| 9    | Calculate Tracker Rotation Angles |


## Edits and Additions

If you would like to see support for another algorithm or would like to suggest edits or additions to this documentation page, please open an issue on the [Proximal GitHub repository](https://github.com/ProximalEnergy/docs-mdbook).
