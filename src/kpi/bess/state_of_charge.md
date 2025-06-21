# State of Charge

## Description

State of Charge (SOC) KPIs monitor BESS performance and health. Two key metrics help identify utilization patterns and operational efficiency:

1. **Average SOC** - The mean state of charge over a time period
2. **Resting SOC** - The state of charge when the battery is idle

## Average SOC

### Why It Matters
- **Battery Health**: Extreme SOC levels (very high or very low) accelerate battery degradation
- **Longevity**: Operating within optimal SOC ranges extends battery lifespan
- **Performance**: Consistent extreme SOC can reduce capacity and power capability over time

### How It's Calculated
1. Collect SOC data at regular intervals
2. Calculate the arithmetic mean for the time period

## Resting SOC

### Why It Matters
- **Battery Stress**: Prolonged storage at extreme SOC levels causes chemical stress
- **Aging**: High resting SOC increases calendar aging, low resting SOC can cause capacity fade
- **Safety**: Extreme resting SOC levels may indicate potential safety concerns

### How It's Calculated
1. Collect SOC and power flow data
2. Identify periods with minimal power flow
3. Calculate mean SOC during these resting periods
