# Tracker KPIs

## Description
Tracker KPIs are a set of KPIs that are used to monitor the performance of trackers on a project. The two KPIs track a row's position deviation from its setpoint, and a row's setpoint deviation from its zone controller's median. These are used to characterize the performance of each tracker row, and can be used to identify offline trackers, errant zone controller setpoints, and communication issues.

## Position Deviating from Setpoint
1. Pull tracker position and setpoint for each tracker on a 5-minute basis.
2. For each 5-minute interval, compare position to setpoint. Average the absolute-value difference between these values for the entire day.
3. Aggregate each tracker row into blocks, averaging the values again. The output by block is the mean deviation of position from setpoint across all trackers on that block on that day.

## Setpoint Deviating from Median
1. Pull tracker setpoint for each tracker on a 5-minute basis.
2. Calculate the median tracker setpoint for each zone controller.
3. For each 5-minute interval, compare setpoint to zone controller median. Average the absolute-value difference between these values for the entire day.
4. Aggregate each tracker row into blocks, averaging the values again. The output by block is the mean deviation of setpoint from NCU median across all trackers on that block on that day.
