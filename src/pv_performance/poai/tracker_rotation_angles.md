# Tracker Rotation Angles

## Terms and Acronyms
- SCADA:  Supervisory Control and Data Acquisition
- PCS:  Power Conversion System (Inverter)


## General

The tracker rotation angle is the angle (theta) about the torque tube axis.  The general convention is that the torque tube exists as a right handed coordinate system where the thumb points towards the equator.  The curl of the fingers represents positive rotation about the axis.  In the northern hemisphere the trackers will rotate positively towards the West.  In the southern hemisphere the trakcers will rotate positively towards the East.

The tracker rotation algorithm takes the following inputs:
- Ground Coverage Ratio:  The spacing of the trackers in the cross-torque-tube-axis direction.
  - Please note that the physical ground coverage ratio may not be the same as the algorithm set point ground coverage ratio.  Tracker manufacturers may increase the algorithm set point ground coverage ratio relative to the physical ground coverage ratio to account for the effects of the trackers being mounted on an uneven surface and with non-zero pile height construction tolerances.


## Data Sources

The tracker rotation angles are calculated from the following data sources:

- GIS Data
  - The physical ground coverage ratio can be calculated from GIS data if that data is provided by the project owner.  The GIS data must match the location of the trackers and the SCADA tags.
  - The GCR is then calculated at each PCS by filtering tracker locations for a single row and then ordering the trackers by their position from east to west.  The GCR is then calculated by dividing the tracker width by the row-to-row distance.
