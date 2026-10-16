# Validation status

This page separates design evidence from physical test results. Last updated: 16 October 2026.

## Check evidence

The [manifest](checks/manifest.json) identifies source hashes and report counts. The [DRC report](checks/DRC.json) and [ERC report](checks/ERC.json) are recorded KiCad outputs with all rule severities included: zero issues, zero ignored checks and zero current exclusions.

The four-sheet readability refactor was checked against the preceding schematic: all 68 physical references, values, sourcing fields, footprints and original component UUIDs were preserved. All 135 nets retain identical reference-and-pin membership, and the split MCU retains the same 144 pin definitions and electrical types. A structural PCB comparison permits only net names, schematic paths and symbol-unit metadata to differ; all geometry remains identical. See the [comparison record](checks/schematic-refactor.json).

The earlier [exclusions report](checks/ERC-exclusions.json) is retained as history. Its seven entries describe the previous schematic. After redrawing, those entries were removed and ERC was rerun with no exclusions; none of those notices recur. The manifest includes byte hashes and hashes normalized to LF to distinguish Git line-ending conversion from design changes.

| Area | Available evidence | Still required |
|---|---|---|
| Schematic and PCB consistency | ERC, DRC and parity checks have zero issues, ignored checks or current exclusions; before/after connectivity and PCB geometry comparison passed | Recheck after future design changes |
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
