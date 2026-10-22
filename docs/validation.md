# Validation status

Last updated: 22 October 2026. Hardware remains **not bench-tested**.

## Current source and check evidence

The [manifest](checks/manifest.json) records source and report hashes, including LF-normalized source hashes for Git line-ending conversion. The [DRC report](checks/DRC.json) and [ERC report](checks/ERC.json) report zero violations/issues, zero unconnected items and zero schematic parity issues, with no ignored checks or current exclusions. Reports were recorded for the corrected saved source, not inferred from a screenshot.

| Check | Result | Scope |
|---|---|---|
| ERC | 0 issues | Schematic electrical rules and configured pin types |
| DRC/parity | 0 violations, 0 unconnected items, 0 parity issues | Configured PCB constraints and schematic consistency |
| [Corner audit](checks/bend-audit.json) | 0 ordinary 90° track bends | 1178 straight segments; same-net/layer endpoint joins, 1° tolerance, endpoints rounded to 1 µm; pads/vias/branches separate |
| [Ground projection](checks/ground-screen.json) | 0 missing samples on checked routes | Outer-layer centrelines of USB D+/D-, SWDIO, SWCLK and PG9; ≤0.05 mm steps, drilled neighbourhood exclusions |
| [Fabrication export verification](checks/export-verification.json) | Archive/source comparison passed | Current Gerbers and drill outputs; 294 plated holes/slots and 2 non-plated holes |
| BOM/CPL inventory | 66 fitted references, 31 part types | Exact selected part inventory; three PCB test points are not assembly items |

ERC/DRC do not validate firmware, component selection in every operating condition, power integrity, electromagnetic compatibility or real signal behaviour. Ground projection and corner checks are geometric screens, not electromagnetic simulations. Drill-coordinate comparisons include expected export rounding; see the report for tolerances.

## Closed design-review items

- Added C29 as local TPS22918 input bypass; synchronized value, package, sourcing and assembly data.
- Moved the local PG9 crossing to F.Cu and removed obsolete transition vias, restoring the identified ground reference beneath SWDIO after zone refill. Ground stitching remains.
- Corrected the identified PF6 right-angle bend and checked the saved board again. Pad entries and branch junctions are not represented as ordinary bends.
- Corrected the USB-specific heat-pour clearance rule to use the current net names.
- Obtained fabricator confirmation of the nominal JLC04161H-7628 construction used in the layout/calculation.
- Updated J1 sourcing to C55629812 and checked selected components and supplier placement corrections after a quote reload.

The [engineering review](engineering-review.md) explains the evidence, component guidance and routing decisions.

## Open validation work

| Area | Still required |
|---|---|
| Manufacturing | Review final production files and component placement confirmations; inspect delivered boards |
| Power | Rail startup, input headroom, current draw, load-step and thermal measurements; establish the usable total current budget |
| Debug/reset | Verify custom J1 adapter mapping, probe reference voltage, repeatable connection and reset |
| Clock | Check crystal startup, frequency and drive without disturbing the oscillator |
| USB | Correct PA9/VBUS firmware configuration, enumeration and repeated transfer/power-cycle testing; no channel impedance certification is claimed |
| GPIO/expansion | Validate header mapping and basic I/O with harmless loads; external CAN transceiver required |
| Robustness | Application-specific protection, EMC/ESD testing, environmental and industrial suitability remain unqualified |

The 250 mA total power budget is conditional. Inner-layer fills include signal crossovers and are not wholly uninterrupted planes. USB escapes, pad transitions, unequal via counts and actual channel behaviour remain relevant despite the nominal 90 Ω straight-section calculation. Crystal/reset placement still has compromises. No firmware or hardware functionality has yet been demonstrated in this repository.

## Historical evidence

The [16 October schematic-refactor comparison](checks/schematic-refactor.json) preserved the then-current 68 physical references and all 135 net memberships, with unchanged PCB geometry for that refactor. C29 was added later. The old [ERC exclusions report](checks/ERC-exclusions.json) is retained as historical context; it does not activate exclusions in the current reports.

The [prototype package](../manufacturing/prototype-r3/) contains the reviewed exports and hashes. Follow the [bring-up checklist](bring-up.md), recording actual measurements and firmware versions in the [test log](test-log.md). A clean design-rule report does not turn this prototype into a production-qualified design.
