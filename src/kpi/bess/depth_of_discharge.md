# Depth of Discharge KPI

## Description

The **Depth of Discharge (DoD) KPI** quantifies the average depth to which a battery storage system is discharged during a given day. It is calculated as the inverse of the average State of Charge (SOC). A higher DoD indicates that the battery is being used more deeply, while a lower DoD suggests more conservative usage.

This KPI is useful for assessing operational behavior and wear-and-tear on battery storage systems. It is reported daily as a **percentage** between 0 and 1, where 1 means fully discharged on average, and 0 means fully charged on average.

## Methodology

1. **Retrieve time-series data** for:
   - `project_soc_percent`: The State of Charge (%) of the project’s battery storage system.

2. **Convert timestamp index**:
   - The index is converted to the project’s local timezone for consistent daily aggregation.

3. **Calculate average SOC**:
   - Take the mean of all [State of Charge (SOC)](./state_of_charge.md) readings for the day. SOC values are expected to be in the range [0, 1].

4. **Calculate Depth of Discharge**:
   - DoD is the complement of SOC:

     $$
     \text{DoD} = 1 - \text{SOC}
     $$

   - The result is rounded to four decimal places.

5. **Store the KPI result**:
   - The daily average DoD is inserted as a project-level KPI for the corresponding date.
