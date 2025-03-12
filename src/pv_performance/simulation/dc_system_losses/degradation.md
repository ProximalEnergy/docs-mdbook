# Degradation

## General

The Proximal platform automatically creates a model with warranted module degradation baked in.  This module degradation occurs on the DC side of the plant and therefore cannot be post-processed due to the effects of clipping.

The warranted module degradation curve is module specific and usually comes in the form of an initial degradation value at year zero and a linear or piecewise linear drop from year one on-wards.

Degradation in PV modules can occur in both current and voltage.  The Proximal model currently assumes a 50% degradation in current and 50% degradation in voltage for a given warranted degradation in power.

A different assumption can be used for a given PV module technology, if specified by the user.
