# Module State of Health Report

## Overview

This report is designed to characterize the performance of the modules using the combiners as a proxy. Through heavy filtering, the report aims to analyze only the combiners that are performing at the highest level, to achieve a picture representative of the best possible performance of the modules. The filters are designed to remove any effects that can be attributed to the underperformance of inverters, trackers, or other non-DC related effects.

After filtering, the performance of each combiner is characterized through the following formula:

$$
\text{DC Performance} = 1 - \frac{E_\text{modeled} - E_\text{actual}}{E_\text{modeled}}
$$

Where:

- $E_\text{modeled}$ is the expected energy production of the combiner, detailed in the [PV Model Overview](../pv_performance/pv_model_overview.md). $E_\text{modeled}$ is representative of the warranted degradation curve of the modules at the time of analysis.
- $E_\text{actual}$ is the actual energy production of the combiner, calculated as the combiner current × inverter DC voltage.

The resulting metric is representative of the performance of the modules, with 100% indicating that the modules are performing at the expected level.

## Filters

Unlike the [DC Amperage report](dc_amperage.md), the module state of health report is not capable of user-defined filters. Instead, the report has a defined set of filters that are applied to the data, and the analysis is automatically generated on a daily basis. In addition to clearsky filters, several performance-based filters are applied to ensure that the analysis is only performed on combiners that can be considered representative of high performance.

#### Clearsky Filters

- Minimum POA: 250 W/m²
- Maximum POA Derivative: 2 W/m²/minute
- Standard Deviation filters
  - Maximum POA Standard Deviation: 7.5 W/m²
  - Maximum POA Derivative Standard Deviation: 0.25 W/m²/minute
  - To filter on standard deviation and derivative standard deviation, both conditions must be met to exclude data. All other conditions are applied individually.
- POA sensors on errant trackers are excluded analytically
- At least 1 hour of clearsky-filtered data

#### Performance-Based Filters

- Inverter AC output between 5% and 95% of the inverter's nameplate capacity
  - This ensures the inverter is online and not AC clipping
- Inverter module voltages are within 5V of each other (as applicable)
  - Since combiner power must be calculated as (combiner current) × (inverter DC voltage), the module voltages must be within a narrow band to ensure that the voltage is representative of the combiners.
- [Combiner DC Field Health](../kpi/combiner_fuse_health.md) is at least 97.5% of the project nonzero mean per day.
  - If a combiner fails the fuse health filter, it is removed from the analysis.
- [Tracker position deviating from setpoint](../kpi/trackers.md#position-deviation-from-setpoint) is less than 1° on average per day.
- [Tracker setpoint deviating from median](../kpi/trackers.md#setpoint-deviation-from-median) is less than 1° on average per day
  - Both tracker filters are applied to entire blocks. If a block fails the analysis, all combiners on the block are excluded for that day.

#### Other

- Soiling is accounted for in the expected performance calculation through the onsite soiling sensors. Since this adjustment is applied to both expected and actual performance, it is assumed to cancel out in the performance calculation and is not considered in the analysis.

#### Manual Filtering

- On the Clearsky tab, users may select individual days to view. The individual days show combiner-level performance as well as the clearsky POA data shaded in green for timestamps selected for analysis.
- On all tabs, users may select a specific date range to view. It is recommended to select a date range that is at least one month long to ensure enough data is available for analysis.
- The report is presented for the previous year of data by default, but either of these filters are applied to data presentation on all tabs.

## Outputs

Outputs are available for each combiner, as well as various summary statistics. Graphics are available on the project level, individual combiner level, and aggregates for inverter and circuit level roll-ups. Combiner-level graphics are colored based on the capacity and bin classification of modules associated with each combiner.
On the GIS tab, the report is available on a combiner-level basis.

## Caveats

- Due to the heavy filtering applied to the data, the report may not generate for some days.
- If the system definition is incorrect, (for example if the DC capacity reported to Proximal during commissioning was incorrect) then the report will contains inaccuracies for those combiners which are defined incorrectly since the combiner DC capacity is used in the normalization calculation.
