# Design Notes — Automatic Light-Controlled LED Circuit

## Objective and Provenance

**Course:** ENGR 098 — Circuit Analysis (Summer 2026). This repository preserves the original editable KiCad design artifacts, manufactured-prototype evidence, detailed resubmission report, and presentation.

## As-Built Electronic Architecture

**Supply:** 9 V DC, with a main SPST power switch.

| Functional block | As-built components | Explanation |
| --- | --- | --- |
| Photoresistor divider | R1 = 100 kΩ; R3 = GL5516-type LDR | Produces the SENSE voltage; darkness increases LDR resistance and SENSE voltage. |
| Transistor interface | Q1, Q2 = BC547 NPN; R6 = 100 kΩ pull-down | Darlington-style low-side switch with reduced sensing-network loading. |
| LED load | R2, R4, R5 = 1 kΩ; D1–D6 white LEDs | Three branches in parallel, each with two LEDs in series and an individual resistor. |

### Approximate analysis

Unloaded divider equation:

```text
V_SENSE = 9 V × R_LDR / (100 kΩ + R_LDR)
```

Assuming the paired transistors begin turning on around 1.2–1.4 V at the input, the corresponding **first-order estimated** LDR switching resistance is approximately **15.38–18.42 kΩ**.

LED branch current estimate:

```text
I_branch = (9 V - 2×V_LED - V_switch) / 1 kΩ
```

With the assumptions in the report, **1.6–2.2 mA per branch** and **4.8–6.6 mA total** are estimated, **not measured**, currents. More details—including modeled limitations, device-rating margins, Thévenin analysis, and battery sensitivity—are documented in [`technical_report.pdf`](technical_report.pdf).

## Implementation and Manufacturing

- KiCad board uses 16 placed footprints and four custom footprint definitions.
- Routing passed KiCad DRC with **zero errors** and **zero unrouted connections** before export.
- The PCB substrate was 3D printed with a **0.2 mm nozzle** to accommodate its small pad and trace geometry.
- Copper-foil tape provided the printed trace connections; insulated jumper wires crossed other routes.
- The ~78 mm circuit board was integrated into a modified lamp enclosure with an ~90 mm internal cavity.
- Fusion 360 enclosure screenshot appears in the technical report. A native Fusion 360 CAD file was **not** included in the source archive; the included `.step` is a KiCad export.

## PSpice Model Scope

The detailed report documents a PSpice netlist/setup with a parameterized resistance sweep from **100 Ω to 10 MΩ**. The legacy evaluation release completed the sweep, but the exported output did **not** contain the numerical trace table. Do not represent the analytical/predicted figure as a measured or exported PSpice dataset.

## External Design Credit

The lamp enclosure was adapted from the **Waves Designer Lamp** design available via [Printables](https://www.printables.com/model/1261597-waves-designer-lamp/comments). The project-specific Fusion 360 modification is shown in the accompanying report. No third-party enclosure geometry has been republished in this repository.

## Planned Improvements (Not Yet Implemented)

1. Insulate and strain-relieve both LDR leads to prevent intermittent opens.
2. Consider comparator hysteresis and an adjustable light-sensing threshold.
3. Improve pad geometry, soldering access, and trace fabrication consistency.
4. Add an LED diffuser and additional enclosure clearance if needed.
5. Collect direct voltage/current measurements during a controlled retest.
