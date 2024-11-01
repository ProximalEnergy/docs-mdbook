# PV Model Overview

## Why Proximal

The Proximal PV performance model has been specifically designed with asset management in mind.  This means that:

- The model is **sub-hourly** which corresponds with the high frequency data that comes from operating assets.
- The model is **nodal** which allows users to see the expected energy of different nodes in the system including at the combiner, the inverter, and the substation.  It also means that there is finally a model which allocates met station data geo-spatially to the closest node in the system.  This can be a great improvement for real time systems with passing clouds.
- The model is **based on pvlib** which means that it is modern, tested, open-source, trusted by the community, and extendable.  If there is a model you would like us to use in the proximal PV performance model, all you need to do is submit a pull request to pvlib.
- The model is **documented** which means that you can see exactly what is being calculated and how.
- The model is focused on **Root Mean Squared Error (RMSE)** as well as the **Mean Squared Error (MSE)**.  While most PV models in development focus only on MBE, RMSE can be a better metric for models which operate on a real time basis.
