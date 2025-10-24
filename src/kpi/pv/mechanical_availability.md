# Mechanical Availability

## Description

The **PV PCS Mechanical Availability KPIs** quantify the operational status of PV inverters (Power Conversion Systems) using two different approaches: a simple device-level metric and a sophisticated project-level analysis that accounts for communication issues and missing power.

Both KPIs are calculated daily and expressed as percentages (0-100%).

## KPI Type ID 1: Device-Level PV PCS Mechanical Availability

### Description

This KPI calculates the simple binary availability of each individual PV inverter based on whether it's producing power above zero.

### Methodology

1. **Retrieve time-series data** for:

   - PV inverter power data: Individual inverter power measurements
   - `poa_avg`: Plane of Array irradiance data for filtering daylight hours

2. **Filter the data**:

   - Filter time steps to periods where POA > 90 W/m² to focus on daylight hours

3. **Calculate binary availability**:

   - For each inverter at each time step: 1 if power > 0, 0 if power ≤ 0
   - Calculate the mean availability across all time steps for each inverter

4. **Compute Device-Level Availability**:

   - The formula is:

     $$
     \text{Device Availability}_i = \frac{\sum \text{Binary Status}_i}{\text{Total Time Steps}} \times 100\%
     $$

   - Where i represents each individual inverter

## KPI Type ID 5: Project-Level PV PCS Mechanical Availability

### Description

This KPI provides a sophisticated analysis of overall project inverter availability by accounting for communication failures and using meter power to identify potentially offline inverters that aren't directly observable.

### Methodology

1. **Retrieve time-series data** for:

   - `meter_active_power`: AC power delivered by the site (used as reference)
   - PV inverter power data: Individual inverter power measurements
   - `poa_avg`: Plane of Array irradiance data for filtering daylight hours

2. **Set operational thresholds**:

   - Low threshold: 2 kW (inverters below this are considered offline)
   - High threshold: 250 kW (used for identifying pegged/stuck values)
   - POA threshold: 90 W/m² (only analyze periods with sufficient irradiance)

3. **Clean and filter the data**:

   - Filter time steps to periods where POA > 90 W/m² to focus on daylight hours
   - Identify and replace "stuck" or pegged values with NaNs (values that don't change for 15+ consecutive 5-minute intervals)

4. **Classify inverter states**:

   - **Online**: Inverters producing power above the low threshold
   - **Offline**: Inverters producing power at or below the low threshold
   - **Non-communications**: Inverters with missing data (NaNs)

5. **Calculate missing power**:

   - Sum power from all online inverters and apply 98% efficiency factor
   - Compare total inverter power to meter power to identify "missing power"
   - Missing power represents potential offline inverters not clearly identified

6. **Estimate make-up inverters**:

   - Calculate how many average-performing inverters would be needed to account for the missing power
   - This helps identify additional offline inverters that may not be directly observable

7. **Impute final availability**:

   - Add estimated offline inverters to the directly observed offline count
   - Calculate mechanical availability as the ratio of online time to total time

8. **Compute Project-Level Availability**:

   - The formula is:

     $$
     \text{Project Availability} = \frac{\sum \text{Online Imputed Time}}{\sum \text{Total Time}} \times 100\%
     $$

## KPI Type ID 101: PV DC Combiner Mechanical Availability

### Description

This KPI calculates the mechanical availability of PV DC combiners by analyzing their current output during periods of sufficient irradiance and inverter operation.

### Methodology

1. **Retrieve time-series data** for:

   - `met_station_poa`: Plane of Array irradiance measurements
   - `pv_pcs_ac_power`: PV inverter AC power output
   - `pv_dc_combiner_current`: DC combiner current measurements

2. **Set operational thresholds**:

   - POA threshold: 50 W/m² (minimum irradiance for valid analysis)
   - PV PCS AC power threshold: 5 kW (minimum inverter output for valid analysis)
   - Combiner current threshold: 10 A (minimum current for combiner to be considered available)

3. **Establish device relationships**:

   - Map DC combiners to their parent PV inverters using device hierarchy paths
   - Group combiners by their associated inverter for analysis

4. **Filter valid periods**:

   - Identify time periods where both POA > 50 W/m² AND PV PCS AC power > 5 kW
   - This ensures analysis only occurs during productive daylight hours

5. **Calculate combiner availability**:

   - For each combiner at each valid time step: 1 if current > 10 A, 0 if current ≤ 10 A
   - Calculate availability as the ratio of available time to valid time for each combiner

6. **Compute Combiner Availability**:

   - The formula is:

     $$
     \text{Combiner Availability}_i = \frac{\sum \text{Available Time}_i}{\sum \text{Valid Time}_i} \times 100\%
     $$

   - Where i represents each individual DC combiner
   - Project-level availability is the weighted average across all combiners
