# XL4015 USB Buck Converter

**An amateur/prototype DC-DC buck converter that provides a 5 V USB-A output from an 8–30 V DC input.**

This project is a PCB prototype developed by an Electrical and Electronics Engineering student for learning, design practice, and open-source sharing. The XL4015-based circuit steps a higher DC input voltage down to a regulated 5 V level suitable for a USB-A power output.

> [!WARNING]
> This board is an **amateur prototype** and has not been certified for safety-critical, commercial, or unattended applications. ERC checks for the schematic and DRC checks for the PCB were completed with **0 errors**. However, this does not mean the design is complete or error-free: there may still be overlooked issues related to connectivity, footprints, component selection, thermal behavior, EMI, or manufacturing. Independently verify the schematic, manufacturing files, and real measurements for your own application before using the board. All use is at the user's own risk.

## Design summary

| Feature | Value / status |
| --- | --- |
| Converter type | Asynchronous buck (step-down) |
| Controller IC | XL4015 / XL4015E1 |
| Recommended input | 8–30 V DC |
| IC operating range | 8–36 V DC |
| Target output | 5 V DC, USB-A VBUS |
| Feedback setting | R1 = 3.3 kΩ, R2 = 1.1 kΩ → approximately 5.0 V |
| ERC / DRC | 0 errors during the design stage |
| Status | Prototype; do not claim a continuous-current rating until it is validated with real load, temperature, and ripple tests |

For the XL4015, 40 V is an **absolute maximum rating**, not an operating voltage. The intended input range for this design is therefore 8–30 V. In addition, the fitted **SS54 diode has a 40 V reverse-voltage rating**, so this component is not suitable for 36 V or 40 V input. If a higher input voltage is required, select all relevant components—starting with the diode—with adequate voltage margin.

## How the circuit works

The DC input enters through the J1 terminal. The input capacitors support transient current demand and help reduce switching noise. The XL4015 drives its internal switch at approximately 180 kHz. D1 is the freewheeling diode that provides a path for the inductor current when the switch turns off. L1 and the output capacitor filter the switching waveform to produce a 5 V USB output.

The R1/R2 feedback divider sets the output voltage:

`VOUT ≈ 1.25 V × (1 + R1 / R2)`

`VOUT ≈ 1.25 V × (1 + 3.3 kΩ / 1.1 kΩ) = 5.0 V`

## Components and why they are used

| Reference | Part / value | Purpose and selection rationale |
| --- | --- | --- |
| U1 | XL4015 / XL4015E1 | Wide-input, fixed-frequency buck regulator IC intended for higher-current applications. |
| J1 | 2-pin screw terminal | Provides a robust, convenient DC input connection. |
| J2 | USB-A female connector | Presents the regulated 5 V output through a USB connector. |
| L1 | 47 µH power inductor | Stores energy and filters output current/ripple. Its saturation-current rating should exceed the expected peak current, and its DCR should be low. |
| D1 | SS54 Schottky diode | Provides the freewheeling path in the asynchronous buck topology; the low forward voltage helps reduce loss. This choice limits the design to 8–30 V input; use a 60 V-or-higher diode for higher voltages. |
| C2 | 220 µF / 50 V electrolytic capacitor | Supports low-frequency input ripple and transient current demand. |
| C1 | 10 µF / 50 V | Provides additional input filtering/local bypassing. |
| C4 | 1 µF | Compensation capacitor for the XL4015 VC/compensation network. Confirm its value against the XL4015 application circuit being used. |
| C3 | 470 µF | Reduces output ripple and improves load-transient response. Its voltage rating must exceed the 5 V output. |
| R1 | 3.3 kΩ | VOUT-to-FB feedback resistor; sets the 5 V output together with R2. |
| R2 | 1.1 kΩ | FB-to-GND feedback resistor; sets the 5 V output together with R1. |

## Connections

### Input — J1

| J1 pin | Connection |
| --- | --- |
| 1 | Positive DC input, `VIN` |
| 2 | Input negative / `GND` |

Connect only **8–30 V DC** to the input. Before assembling or connecting power, always cross-check the physical connector orientation and `VIN/GND` markings against the schematic.

### Output — USB-A (J2)

| USB-A pin | Connection |
| --- | --- |
| 1 — VBUS | Regulated +5 V |
| 4 — GND | Common ground |
| 2 — D− | Not used in this design |
| 3 — D+ | Not used in this design |

This board supplies **5 V power** through USB-A. It does not implement BC 1.2, Apple, or USB-C PD charging identification on D+/D−, so phones and some other devices may limit the charging current they request.

## Known limitations and suggestions for the next revision

- The continuous output-current capability depends on inductor saturation current, diode loss, copper thickness, airflow, and IC temperature. 
- For operation near 30 V input, consider a TVS diode, fuse/PTC, and reverse-polarity protection for input transients.
- Keep high-current traces short and wide; make the switching loop between the XL4015, D1, and input capacitor as compact as possible.
- Keep the feedback divider away from the switching node and close to the output capacitor.
- Before fabrication, independently review Gerbers, drill files, footprints, polarities, and 3D mechanical clearances.

## Suggested test procedure

For first power-up, use a current-limited bench power supply. First verify that the no-load output is close to 5 V. Then use an electronic load in gradual steps to measure voltage regulation, ripple, input current, and the temperatures of the diode, inductor, and IC, as well as short-circuit behavior. Before connecting a USB device, check that there is no short circuit between VBUS and GND.


<img width="962" height="541" alt="gitpcb2" src="https://github.com/user-attachments/assets/cbf1722e-bdb7-49e3-82b1-00c6f024f6c3" />
<img width="1547" height="612" alt="gitpcb1" src="https://github.com/user-attachments/assets/4e497a38-737c-4e45-8a4c-36cc01a40b3c" />
<img width="422" height="607" alt="buckcon1" src="https://github.com/user-attachments/assets/0f71673f-76cf-4223-986d-cc5305563b6c" />
