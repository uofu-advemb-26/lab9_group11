# Lab 9 Documentation
## Invariants
- The arriving and departing sensors are placed sufficiently far from the railroad crossing that the arms will have lowered by the time the train reaches the intersection.
- The train won't be moving fast enough to break the 10 second deadline between the lights flashing and the arms lowering.
- The departing sensor is triggered by the tail end of the train (when weight is no longer detected) rather than the front end of the train (so that long trains don't remain in in the intersection when the departing sensor is triggered).
- Trains are spaced with sufficient distance such that a second train on the same track won't trigger an arriving signal before the prior train has triggered the departing signal.
- The sensors are reliable and won't be disrupted by environmental conditions.
- Passersby will heed the alarms and not bypass the arms.
