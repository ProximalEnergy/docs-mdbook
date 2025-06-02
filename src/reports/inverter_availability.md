# Inverter&nbsp;Availability

## Description

Inverter availability KPIs measure how often each inverter (and, when present, its internal power-conversion modules) is producing at least a minimum power output under valid irradiance conditions.

One daily report is produced containing:

1. **Inverter Availability** – percentage of daylight intervals in which each inverter’s AC power exceeds a user-defined threshold.
2. **Module Availability** – same calculation applied to individual inverter-module channels (only shown if the project reports module-level power).

This report helps identify offline or under-performing inverters, balance-of-plant outages, and data-quality issues.

## Parameters (default values)

| Parameter                        | Purpose                                                                                     | Default    |
| -------------------------------- | ------------------------------------------------------------------------------------------- | ---------- |
| Available&nbsp;Min&nbsp;(MW)     | Minimum inverter (or module) AC power required to count as “available”.                     | **0 MW**   |
| Irradiance Min (W/m²)            | Minimum plane-of-array irradiance at which availability is evaluated.                       | **0 W/m²** |
| Inverter Availability (%)        | Workbook formula – average of all inverter availabilities.                                  | _(output)_ |
| Inverter Module Availability (%) | Workbook formula – average of all module availabilities (shown only if module data exists). | _(output)_ |

## Filters

An individual 5-minute interval is **excluded** from availability calculations if:

1. Plane-of-array irradiance ≤ _Irradiance Min_.

All other samples form the **valid daylight set** for the day.

## Method – Inverter Availability

1. **Query** 5-minute _pv_pcs_ac_power_ and _met_station_poa_ for every inverter in the project.
2. **Validate irradiance** – keep only timestamps where POA > _Irradiance Min_.
3. **Evaluate power threshold** – for each inverter, flag samples where  
   \[
   P\_{\text{inv}}(t) > \text{Available Min}
   \]
4. **Compute availability** per inverter  
   \[
   A*{\text{inv}}=\frac{\text{count}\left(P*{\text{inv}}> \text{Available Min}\right)}
   {\text{count(valid daylight samples)}}
   \]
5. **Export** an Excel workbook containing:
   - **Parameters** – editable inputs & live output KPIs.
   - **Inverter Availability** – % available for each inverter.
   - **Inverter Power** – raw 5-minute power time-series.
   - **Irradiance** – POA irradiance (mean + individual sensors).

Availability formulas live in Excel, so users can adjust _Available Min_ or _Irradiance Min_ and see KPIs update instantly.

## Module Availability (when reported)

If the project provides _PV PCS Module AC Power_:

1. **Repeat Steps 1-4** using module-level power.
2. **Add sheets** – **Module Availability** and **Module Power** – mirroring the inverter sheets.
