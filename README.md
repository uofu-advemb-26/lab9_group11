# Lab 9 Documentation
## Invariants
- No trains are in the intersection when the arms are up.
- No trains are in the intersection when the alarms are off.
- The arriving and departing sensors are placed sufficiently far from the railroad crossing that the arms will have lowered by the time the train reaches the intersection.
- The train won't be moving fast enough to break the 10 second deadline between the lights flashing and the arms lowering.
- The departing sensor is triggered by the tail end of the train (when weight is no longer detected) rather than the front end of the train (so that long trains don't remain in in the intersection when the departing sensor is triggered).
- Trains are spaced with sufficient distance such that a second train on the same track won't trigger an arriving signal before the prior train has triggered the departing signal.
- The sensors are reliable and won't be disrupted by environmental conditions.
- Passersby will heed the alarms and not bypass the arms.
- An approach signal will always precede a depart signal.
- The power will never be interrupted.
- Trains on each track only run in one direction.

## Varying Invariants (On Example FSM)
- The invariant "an approach signal will always preceed a depart signal" would be broken in the following sequence of events:
  1. On reset, we enter the `idle` state
  2. An nb_approach is received to enter the `ringing, arms up` state.
  3. In this state or the next two states (`ringing, arms up`, `ringing, arms down`, or `ringing, arms_up (2nd time)`), a southbound train approaches and triggers the `sb_approach` signal.
  4. The northbound train exits the intersection and triggers the `nb_depart` signal.
  5. The southbound train exits the intersection and triggers the `sb_depart` signal. From the perspective of this FSM, an invariant has been violated - a depart signal arrived before an arrival signal - because the FSM missed the arrival signal.

## Check Your Work
### FSM #1
![FSM 1](./images/fsm1.png "FSM1")
  - A counterexample for this FSM occurs when it's in the `idle` state and both a northbound and a southbound train arrive at the same time (meaning `nb_approach` and `sb_approach` are present simultaneously). If this happens, the FSM will lose track of one of the trains, the arms will raise when the train it's tracking departs, and then, when the second train departs, a depart signal will arrive before an arrival signal from the perspective of the FSM since it lost track of the corresponding arrival signal.

### FSM #2
![FSM 2](./images/fsm2.png "FSM2")
  - We we unable to find a counterexample for this FSM.

## Prove It
| number | arms_down | alarm_on | northbound_present | southbound_present | *north_approach* | *south_approach* | *north_depart* | *south_depart* | *time_elapsed* | safety_hazard |
|--------|-----------|----------|--------------------|--------------------|------------------|------------------|----------------|----------------|----------------|---------------|
| 0      | 0         | 0        | 0                  | 0                  | 6                | 5                | 17             | 17             | 27             |               |
| 1      | 0         | 0        | 0                  | 1                  |                  |                  |                |                |                | 17            |
| 2      | 0         | 0        | 1                  | 0                  |                  |                  |                |                |                | 17            |
| 3      | 0         | 0        | 1                  | 1                  |                  |                  |                |                |                | 17            |
| 4      | 0         | 1        | 0                  | 0                  | 6                | 5                | 24             | 24             | 0              |               |
| 5      | 0         | 1        | 0                  | 1                  | 7                | 21               | 24             | 18/19 & 16     | 13             |               |
| 6      | 0         | 1        | 1                  | 0                  | 21               | 7                | 18/19 & 16     | 24             | 14             |               |
| 7      | 0         | 1        | 1                  | 1                  | 21               | 21               | 18/19 & 16     | 18/19 & 16     | 15             |               |
| 8      | 1         | 0        | 0                  | 0                  |                  |                  |                |                |                | 28 & 29       |
| 9      | 1         | 0        | 0                  | 1                  |                  |                  |                |                |                | 17 & 29       |
| 10     | 1         | 0        | 1                  | 0                  |                  |                  |                |                |                | 17 & 29       |
| 11     | 1         | 0        | 1                  | 1                  |                  |                  |                |                |                | 17 & 29       |
| 12     | 1         | 1        | 0                  | 0                  |                  |                  |                |                |                | 28            |
| 13     | 1         | 1        | 0                  | 1                  | 15               | 21               | 24             | 4              | 27             |               |
| 14     | 1         | 1        | 1                  | 0                  | 21               | 15               | 4              | 24             | 27             |               |
| 15     | 1         | 1        | 1                  | 1                  | 21               | 21               | 13             | 14             | 27             |               |
  - **Key**
    - Italicized headers are inputs that either cause a state transition to the next state or to the invariant they cause to be violated.
    - An entry in the `safety_hazard` column means that entire state violates an invariant: the number listed is the invariant violated. In these states, the system has already failed, so it's assumed that no recovery is possible. As such, any inputs while in these states are ignored.

| number | invariant |
|--------|-----------|
| 16      | No trains are in the intersection when the arms are up. |
| 17      | No trains are in the intersection when the alarms are off. |
| 18      | The arriving and departing sensors are placed sufficiently far from the railroad crossing that the arms will have lowered by the time the train reaches the intersection. |
| 19      | The train won't be moving fast enough to break the 10 second deadline between the lights flashing and the arms lowering. |
| 20      | The departing sensor is triggered by the tail end of the train (when weight is no longer detected) rather than the front end of the train (so that long trains don't remain in in the intersection when the departing sensor is triggered). |
| 21      | Trains are spaced with sufficient distance such that a second train on the same track won't trigger an arriving signal before the prior train has triggered the departing signal. |
| 22      | The sensors are reliable and won't be disrupted by environmental conditions. |
| 23      | Passersby will heed the alarms and not bypass the arms. |
| 24      | An approach signal will always precede a depart signal. |
| 25      | The power will never be interrupted. |
| 26      | Trains on each track only run in one direction. |
| 27      | Timing signals are only received 10 seconds after the alarms start and 10 seconds after the arms raise |
| 28      | The arms are only down when a train is present. |
| 29      | The arms only lower 10 seconds after the alarms have started. |
