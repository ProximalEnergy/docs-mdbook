# Tracker Availability

## Description

Tracker availability KPIs quantify how often each tracker row is correctly following its expected angle.  
Two daily reports are generated:

1. **Row vs Row Setpoint** – compares each row’s measured position to its own controller setpoint.
2. **Row vs Zone Median Setpoint** – compares each row’s position to the median setpoint of all rows in its zone.

A row is considered **available** when the absolute position error is within an acceptable tolerance. These KPIs surface drifted trackers, stow-logic faults, communication drop-outs, and other issues.

## Parameters (default values)

| Parameter                     | Purpose                                                                                                     | Default    |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------- | ---------- |
| Available Max (deg)           | Max position error allowed to count as “available”.                                                         | **5°**     |
| Irradiance Min (W/m²)         | Minimum plane-of-array irradiance required to evaluate availability.                                        | **0 W/m²** |
| Exclude Stow Periods          | If **TRUE**, stow periods are removed from the analysis. If a project lacks stow data this flag is ignored. | **TRUE**   |
| Maximum Setpoint Change (deg) | Quality gate that rejects 5-min setpoint jumps above this value.                                            | **60°**    |

## Filters

Before availability is calculated, any 5-minute interval is discarded if **any** of the following are true:

1. Tracker **position** _or_ **setpoint** is blank.
2. Plane-of-array irradiance ≤ _Irradiance Min_.
3. _Exclude Stow Periods_ is **TRUE** **and** the tracker zone is reported as stowed.
4. \|Position − Setpoint\| ≥ 120°.
5. \|Setpoint(t) − Setpoint(t-1)\| > _Maximum Setpoint Change_. (Rule 5 is skipped for the first sample of the day.)

## Method 1 – Row vs Row Setpoint

1. **Query** 5-minute _tracker position_, _tracker setpoint_, _met station poa_, and _tracker zone status_ for every row in the project.
2. **Apply filters** listed above to build the valid-data mask.
3. **Calculate position error**  
   \[
   \Delta\theta*{\text{row}}(t)=\left|\text{Position}*{\text{row}}(t)-\text{Setpoint}\_{\text{row}}(t)\right|
   \]
4. **Compute availability for each row**  
   \[
   A*{\text{row}}=\frac{\text{count}\left(\Delta\theta*{\text{row}}\le\text{Available Max}\right)}
   {\text{count(valid samples)}}
   \]
5. **Export** an Excel workbook containing:
   - **Parameters** – editable inputs & descriptions.
   - **Availability** – one value per row (percent).
   - **Difference** – per-sample \(\Delta\theta\_{\text{row}}\) table.
   - Raw data sheets: **Position**, **Setpoint**, **Stow**, **Irradiance**.

## Method 2 – Row vs Zone Median Setpoint

1. **Derive zone setpoints** by taking the **median** of all row setpoints within each zone at every 5-minute timestamp.
2. **Repeat Steps 2-5** from _Method 1_, replacing _Setpoint₍row₎(t)_ with _Setpoint₍zone median₎(t)_ in the error formula.
3. The workbook layout is identical; the **Setpoint** sheet now shows zone medians (prefixed “Zone”).

## Usage Notes

- All calculations occur in the exported Excel file via formulas, allowing users to adjust parameters post-hoc.
