# Trackers

## Description  
Tracker KPIs are used to monitor performance of <a href="https://pvpmc.sandia.gov/modeling-guide/1-weather-design-inputs/array-orientation/single-axis-tracking/" target="_blank">single-axis trackers</a>. Multiple KPIs are calculated for each project.

1. Row position deviation from setpoint.
2. Row setpoint deviation from the project median setpoint.
3. Row availability (when the prior two metrics are both less than 5°).

These KPIs can help identify offline trackers, errant zone controller setpoints, and communication issues.

## Filters  
To exclude time periods when trackers are not in use, any time intervals in which the <a href="https://en.wikipedia.org/wiki/Solar_zenith_angle" target="_blank">sun elevation</a> angle is less than or equal to 0° are excluded from the calculations.

## Position Deviation from Setpoint  
1. Query 5-minute position and setpoint data for each row.  
2. Compare position to setpoint for each 5-minute interval for each row. Average the absolute value of the difference between these values for the entire day.  
3. Aggregate each tracker row into blocks, averaging the values again.

## Setpoint Deviation from Median  
1. Query 5-minute setpoint data for each row.  
2. Calculate the median setpoint for the project.  
3. Compare setpoint to project median for each 5-minute interval for each row. Average the absolute value of the difference between these values for the entire day.  
4. Aggregate each tracker row into blocks, averaging the values again.

## Availability  
1. Query 5-minute position and setpoint data for each row.  
2. Compare position to setpoint for each 5-minute interval for each row.  
3. If both the position and setpoint deviations are less than 5° for a given interval, the row is considered available.  
4. Aggregate each tracker row into blocks, averaging the values again.

