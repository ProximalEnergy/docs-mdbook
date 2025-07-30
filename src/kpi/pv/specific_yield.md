# Specific Yield KPI

## Description

The **Specific Yield KPI** quantifies how much AC energy is delivered per unit of installed DC capacity. It provides a normalized metric for evaluating site performance independent of plant size and is especially useful for comparing across projects.

This KPI is calculated daily and expressed in units of **kWh/kWp**.

## Methodology

1. **Retrieve time-series data** for:
   - `meter_active_power`: AC power delivered by the site over the day.

2. **Clean and aggregate the data**:
   - Clip negative values to zero to eliminate erroneous readings.
   - Resample data to hourly averages to standardize granularity.

3. **Calculate total energy delivered**:
   - Integrate the hourly AC power measurements to get total daily energy output in kWh.

4. **Normalize by DC capacity**:
   - Divide the total energy output by the site’s installed DC capacity (also referred to as kW-peak, or kWp) to get the specific yield.

5. **Compute Specific Yield**:
   - The formula is:

     $$
     \text{Specific Yield} = \frac{\sum \text{AC Energy Output (kWh)}}{\text{Capacity}_{DC} \text{ (kWp)}}
     $$

6. **Store the KPI result**:
   - The resulting value is stored as a project-level KPI for the corresponding date.
