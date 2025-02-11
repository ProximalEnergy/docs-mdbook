# Trackers

## Description  
Tracker KPIs are used to monitor the performance of single-axis trackers on a project. The two KPIs track a row's position deviation from its setpoint, and a row's setpoint deviation from the project median. These KPIs are used to characterize the performance of each tracker row and can help identify offline trackers, errant zone controller setpoints, and communication issues.

## Filters  
To exclude time periods when trackers are not in use, any time intervals in which the <a href="https://en.wikipedia.org/wiki/Solar_zenith_angle" target="_blank">sun elevation</a> angle is below or equal to 0° are excluded from the calculation.

## Position Deviation from Setpoint  
1. Pull the tracker position and setpoint for each tracker every 5 minutes.  
2. For each 5-minute interval, compare the position to the setpoint. Average the absolute value of the difference between these values for the entire day.  
3. Aggregate each tracker row into blocks, averaging the values again. The output for each block is the mean deviation of position from setpoint across all trackers in that block for the day.

## Setpoint Deviation from Median  
1. Pull the tracker setpoint for each tracker every 5 minutes.  
2. Calculate the median tracker setpoint for the project.  
3. For each 5-minute interval, compare the setpoint to the project median. Average the absolute value of the difference between these values for the entire day.  
4. Aggregate each tracker row into blocks, averaging the values again. The output for each block is the mean deviation of the setpoint from the project median across all trackers in that block for the day.
