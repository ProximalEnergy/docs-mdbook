# DC Amperage Report
## Overview
This report shows the normalized current at every combiner at the PV power plant filtered for clearsky conditions.  Combiner current is normalized based off of the nameplate capacity of each combiner.  This makes it so that combiners with different amounts of dc capacity can be compared against one another on an apples-to-apples basis.

Different combiners may have different DC capacity due to differences in the number of strings attached to each combiner, module bin class, etc.

## Filters
The analysis is generated with user input for clearsky data on a given day. Users may select thresholds for minimum POA, maximum POA derviative, and maximum POA derivative standard deviation. The input data is sampled at 5-minute intervals by default, which can be changed via the settings dropdown located in the filters card.
The maximum POA derivative and maximum POA derivative standard deviation thresholds may be turned off at the risk of reduced report accuracy.

POA derivative is the rate of change of POA with respect to time. A lower POA derivative indicates a more stable POA, correlating to fewer moving clouds over the array.
Proximal recommends a maximum POA derivative of 1 W/m^2^/minute.

POA derivative standard deviation is the standard deviation of the POA derivative over each 5-minute interval. A lower POA derivative standard deviation indicates that all sensors are moving in a similar way at the same time, indicating that the entire site is experiencing similar sky conditions.
Proximal recommends a maximum POA derivative standard deviation of 1 W/m^2^/minute.

Both POA derivative and POA derivative standard deviation are calculated using the selected sample rate of the data, then aggregated to a 1-hour moving average.

## Calculation
This report is calculated by pulling the 5-minute combiner current data, finding the nominal string current at the maximum voltage of the modules, then normalizing the actual current by the nominal string current. This normalization is then compared to the median combiner current for either the inverter associated with that combiner or the site as a whole. The intermediate calculations are available in the output XLSX file.

## Outputs
By default, this report shows the normalzied DC performance of each combiner compared to its peers on the same inverter. Via the settings dropdown, this normalization can be changed to compare to the combiner performance of the entire site.
Results are displayed as a normalized value, where 1.0 indicates that the combiner is operating at an equal level to the median combiner. Combiners colored in green are performing within 5% of the median combiner, while those colored in red are at least 5% below the median and those colored in pink are at least 5% above the median. This 5% threshold can be changed via the settings dropdown.

The combiner performance can be downloaded as an Excel file in the top-right corner of the page, while the raw POA and combiner current data can be downloaded as CSV files in the output settings dropdown.

## Caveats
- There must be at least one clearsky timestamp during the day for this report to be generated.
- If the system definition is incorrect, (for example if the DC capacity reported to Proximal during commissioning was incorrect) then the report will be incorrect for those combiners which are defined incorrectly since the combiner DC capacity is used in the normalization calculation.
