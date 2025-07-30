# Project Cycle Count KPI

## Description

The **Project Cycle Count KPI** estimates the number of full charge-discharge cycles a battery storage system performs in a single day. This KPI is essential for understanding battery utilization and degradation over time.

The KPI is calculated daily and reported as a floating-point number, where 1.0 represents one full cycle (100% discharge followed by 100% recharge, or equivalent cumulative partial cycles).

## Methodology

1. **Retrieve time-series data** for:
   - `project_soc_percent`: The State of Charge (%) of the project’s battery system over the day.

2. **Convert timestamp index**:
   - The index is standardized to ensure consistent time stepping.

3. **Calculate absolute changes in SOC**:
   - Take the absolute difference in SOC between each pair of consecutive time steps.

     $$
     \Delta \text{SOC}_i = \left| \text{SOC}_{i} - \text{SOC}_{i-1} \right|
     $$

4. **Sum the absolute changes and normalize by 2**:
   - The total cycle count is estimated by summing all absolute SOC changes and dividing by 2. This accounts for the fact that a full cycle consists of both discharge and charge.

     $$
     \text{Cycle Count} = \frac{1}{2} \sum_{i} \left| \text{SOC}_{i} - \text{SOC}_{i-1} \right|
     $$

5. **Round and store the KPI result**:
   - The daily cycle count is rounded to four decimal places and stored as a project-level KPI for the given date.
