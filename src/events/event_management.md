# Event Management

Events within a solar plant can occur at various levels of the system and often require careful analysis to determine their root cause. While an event may be initially detected at one component, the actual source of the issue could originate elsewhere in the system.

For example, an inverter may report a fault or performance issue, but the underlying cause could be:

- A problem in the DC combiner box (e.g., blown fuse, faulty connection)
- Issues in the DC field (e.g., damaged panels, soiling, shading)

Similarly, a full outage of an inverter may be detected, but in fact, the parent asset (MV transformer) in this case failed, so the inverter event should get assigned to the MVT event.

Proper event management involves:

1. Detection of failures in the plant (failure modes)
2. Analysis of failure modes and determining possible groupings
3. Root cause determination
4. Analysis of energy/revenue loss  
5. Documentation and tracking for future reference

This systematic approach helps ensure that issues are properly diagnosed and resolved, rather than just treating the symptoms where they are first detected.

Here's how it works in the system:

## System Components and Workflow

The event management system consists of several key Python scripts that work together:

### Core Scripts
- `manage_events.py` - Master script coordinating the workflow
- `create_flags.py` - Identifies offline devices based on power output and irradiance
- `identify_events.py` - Processes flag data to determine actual events
- `loss_accounter.py` - Calculates energy and revenue losses
- `max_tracking_loss.py` - Handles tracker-specific loss calculations
- `create_ppa_series.py` - Generates PPA pricing data

### Process Flow

#### 1. Event Detection (`manage_events.py`)
The master script runs on three different schedules:
- Case 0: Daily 3-day lookback (9:00 UTC)
- Case 1: 6-hourly 1-day lookback
- Case 2: 15-minute 1-hour lookback (no energy loss calculations)

The script:
1. Checks for ongoing processes using filelock
2. Retrieves existing events in the analysis window
3. Creates flags via `create_flags.py`
4. Identifies events via `identify_events.py`
5. Accounts for losses via `loss_accounter.py` (except Case 2)
6. Updates database and notifies subscribers of new events

#### 2. Flag Creation (`create_flags.py`)
Determines when devices are "offline" by:
1. Setting thresholds (POA at 150 W/m², offline at 1.5% of max power)
2. Processing sensor data (meter power, inverter power, combiner current, etc.)
3. Creating boolean timeseries marking offline periods
4. Handling existing events and irradiance conditions

#### 3. Event Identification (`identify_events.py`)
Processes flag data to:
1. Create device hierarchy
2. Detect event starts/ends
3. Handle overlapping events
4. Manage parent-child event relationships
5. Remove redundant events

#### 4. Loss Calculation (`loss_accounter.py`)
Calculates losses by:
1. Retrieving expected power values
2. Processing events hierarchically (meter → inverter → modules → combiners)
3. Accounting for parent-child relationships
4. Calculating tracking losses separately
5. Applying POI limits and PPA pricing

#### 5. Tracking Loss Calculation (`max_tracking_loss.py`)
Specialized calculations for tracker-related losses:
1. Processes weather and position data
2. Calculates theoretical optimal output
3. Determines losses from non-optimal tracking

### Future Enhancements
Planned improvements include:
- Adding fault code correlation for root cause detection
- Incorporating underperformance events
- Enhancing tracking loss calculations
- Implementing actual PPA data integration
