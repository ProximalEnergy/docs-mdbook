# Performance Ratio KPI

## Description

The **Performance Ratio KPI** quantifies the overall efficiency of a PV site by comparing the actual energy produced to the theoretical maximum possible under prevailing irradiance. This KPI helps identify underperformance due to issues such as soiling, inverter clipping, or balance of system losses.

The KPI is calculated daily and results in a dimensionless number typically between 0 and 1, where 1 represents a perfectly efficient system.

## Methodology

1. **Retrieve time-series data** at hourly resolution for:
   - `meter_active_power`: Total AC power delivered by the site.
   - `met_station_poa`: Plane-of-array irradiance from on-site weather stations.

2. **Clean the data** by:
   - Removing any columns (sensor streams) where the maximum value is less than or equal to 0.
   - Clipping all negative values to zero to remove erroneous negative readings.
   - Averaging the values to a uniform hourly granularity.

3. **Calculate Specific Yield**:
   - Total AC energy delivered by the site (sum of `meter_active_power` across all meters) divided by the total DC capacity of the project.
   - Units: kWh/kWp
   - See also: [Specific Yield](./specific_yield.md)

4. **Calculate Reference Yield**:
   - Total POA irradiance summed across all sensors, averaged, and then normalized by a reference peak irradiance value of **1000 Wp/m²** (Watt-peak per square meter).
   $$
   \text{Reference Yield (kWh / kWp)} = \frac{\sum\text{POA Irradiance (Wh/m²)}}{\text{Reference Irradiance (Wp/m²)}}
   $$

5. **Compute Performance Ratio**:
   $$
   \text{Performance Ratio} = \frac{\text{Specific Yield (kWh / kWp)}}{\text{Reference Yield (kWh / kWp)}}
   $$
   - This results in a dimensionless efficiency metric.

6. **Store the KPI result**:
   - The daily PR value is inserted as a project-level KPI for the given date.
