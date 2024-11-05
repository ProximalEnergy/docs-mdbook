# Site Pressure

## General

The solar position algorithm calculates the solar zenith and azimuth angles as well as the "apparent" solar zenith which corrects for the effect of atmospheric refraction.

## Default Algorithm

The algorithm used by Proximal to calculate the site pressure is the [pvlib.atmosphere.alt2pres](https://pvlib-python.readthedocs.io/en/stable/reference/generated/pvlib.atmosphere.alt2pres.html#pvlib.atmosphere.alt2pres) function.

### Inputs

```python
    pressure = pvlib.atmosphere.alt2pres(
        altitude=altitude,          # from project database
    )
```

## Other Algorithms

Currently, Proximal does not support any other algorithms.  If you would like to see support for another algorithm or would like to suggest edits or additions to this documentation page, please open an issue on the [Proximal GitHub repository](https://github.com/ProximalEnergy/docs-mdbook).
