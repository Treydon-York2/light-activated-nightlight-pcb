# Hardware Verification and Fault Investigation

**Evidence source:** [`technical_report.pdf`](technical_report.pdf) and original presentation. This file records outcomes reported for the original built circuit; it is **not** a new test performed after repository creation.

## Test Observations

| Test or activity | Evidence-supported outcome | Notes |
| --- | --- | --- |
| Breadboard assembly | **Pass** | Covering the LDR turned the LEDs on; exposing it to light turned them off. |
| KiCad PCB DRC | **Pass** | Zero DRC errors and zero unrouted connections before fabrication. |
| Secure LDR connection on assembled device | **Functioning** | Dark-on/bright-off behavior shown in report photographs. |
| Repeated operation of assembled device | **Intermittent** | Automatic switching worked around half the time due to photoresistor connection issues. |
| Open LDR connection | **Observed fault consistent with model** | SENSE pulled toward 9 V; the circuit behaved like a manually switched light. |
| LDR short to ground | **Predicted only** | SENSE approaches 0 V, LEDs off; no separate physical fault-injection test recorded. |
| Direct measured branch current | **Not measured** | The report states a multimeter was unavailable. |
| PSpice DC sweep | **Setup/run documented; numerical output unavailable** | No claim of a measured or numerically exported PSpice switching threshold. |

## Actual Fault and Analysis

**Symptom:** The automatic behavior of the final assembly was not consistently repeatable. With an intermittent photoresistor connection, the lamp could remain on when controlled by the main switch rather than by illumination.

**Likely cause supported by the report:** The LDR leads were exposed and insufficiently strain-relieved. Intermittent contact produced an open-circuit-equivalent condition.

**Analytical explanation:** R1 = 100 kΩ pulls the SENSE node toward the 9 V supply when the LDR goes open, enabling the two-BC547 switching stage. This matches the observed switch-only condition.

## Photographic Evidence

- [Dark-condition LEDs on](../images/nightlight_dark_on.jpg)
- [Bright-condition LEDs off](../images/nightlight_bright_off.jpg)
- [Copper-foil layout before assembly](../images/copper_tape_traces.jpg)
- [Completed rear-side jumper wiring](../images/assembled_rear_wiring.jpg)

## Calculated Versus Observed

**Calculated:** 15.38–18.42 kΩ LDR switching region; 1.6–2.2 mA branch current; 4.8–6.6 mA total LED current. These are model-based approximations, not instrument readings.

**Observed:** Breadboard bright-off/dark-on operation; secure-connection assembled bright-off/dark-on operation; intermittent failure of the LDR connection in the final device.

**Not recorded:** Instrumented current measurements, scope traces, supply droop measurements, quantitative turn-on tolerance, and confirmed PSpice numerical traces.

## Recommended Next Test

After insulating and strain-relieving the LDR contacts, repeat multiple bright/dark cycles while logging sensor-node voltage and branch current with appropriate instruments. Record whether intermittent switching persists. This is **proposed future work**, not a completed test.
