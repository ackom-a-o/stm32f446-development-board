# Validation status

This page separates design evidence from physical test results. Last updated: 16 October 2026.

| Area | Available evidence | Still required |
|---|---|---|
| Schematic and PCB consistency | Recorded ERC, DRC and parity checks have zero active issues; seven ERC exclusions were previously reviewed | Recheck after future design changes; review exclusions before release |
| Component identity | Manufacturer sourcing fields synchronized with approved assembly substitutions | Confirm final purchased parts and assembly production files |
| Placement data | 65 intended parts selected; seven headers and U1/U2/U4 corrections checked after upload and fresh reload | Final assembler production placement approval |
| PCB stackup | KiCad models the intended JLC04161H-7628 construction | Quote has no specific stackup requirement; obtain exact fabricator confirmation before ordering |
| Power | Desktop review of USB protection, regulator and switched 3.3 V path | Startup, load, voltage-drop and thermal measurements |
| Debug and reset | SWD/reset connectivity reviewed; external probe planned | Verify probe adapter pin mapping, reset and connection on hardware |
| Crystal | 8 MHz crystal and loading screened from component data | Startup margin, frequency and drive-level assessment |
| USB | Device-side routing, protection and VBUS sensing reviewed | Firmware configuration, enumeration and communication testing |

## Documented limits and bring-up dependencies

- U3 cooling is not thermally qualified. The conditional 250 mA total design budget must be tested on the assembled board.
- A local SWDIO ground-reference gap remains a documented layout compromise; nearby ground stitching does not constitute measured signal-integrity validation. Begin SWD testing conservatively with short leads.
- The USB pair has a recorded length mismatch of approximately 2.34 mm. Nominal impedance calculations do not certify every pad, via or transition.
- Crystal and reset routing were reviewed but include longer paths than an ideal compact placement. Physical startup and reset testing remain necessary.
- Firmware must account for the reviewed STM32 errata. The intended VBUS-sensing arrangement requires the corresponding PA9 input configuration; firmware has not yet been implemented or tested.
- The custom SWD header requires an explicit adapter pin map. Verify target-reference voltage and power connections before connecting the probe; the probe is not assumed to power the board.

## Release evidence

Before creating a manufacturing release, include the approved stackup, a dated schematic PDF, selected PCB views, a reviewed BOM and placement file, final fabrication exports and test status. Remove supplier account identifiers, private quote links, delivery details and chat screenshots from public materials.

The first hardware release should remain marked as an unvalidated prototype until bench results justify changing that label.
