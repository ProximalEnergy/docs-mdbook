# Solar Position

## General

The solar position algorithm calculates the solar zenith and azimuth angles as well as the "apparent" solar zenith which corrects for the effect of atmospheric refraction.

## Default Algorithm

The algorithm used by Proximal to calculate the solar position is the [NREL 2008](https://midcdmz.nrel.gov/spa/) algorithm.  This algorithm was chosen because of it is the lowest uncertaintly algorithm included in the [pvlib](https://pvlib-python.readthedocs.io/en/stable/reference/generated/pvlib.solarposition.get_solarposition.html) library.

> This algorithm calculates the solar zenith and azimuth angles in the period from the year -2000 to 6000, with uncertainties of +/- 0.0003 degrees based on the date, time, and location on Earth.

Because of this choice, the default solar position algorithm in Proximal is more accurate and not equal to the algorithm chosen by PVsyst.

### Inputs

```python
    solar_position = pvlib.solarposition.get_solarposition(
        time=time,                  # from met station data
        latitude=latitude,          # from project database
        longitude=longitude,        # from project database
        altitude=altitude,          # from project database
        pressure=None,              # calculated inside pvlib function
        temperature=temperature,    # from met station
    )
```

## Other Algorithms

Currently, Proximal does not support any other algorithms for calculating solar position.  If you would like to see support for another algorithm or would like to suggest edits or additions to this documentation page, please open an issue on the [Proximal GitHub repository](https://github.com/ProximalEnergy/docs-mdbook).
