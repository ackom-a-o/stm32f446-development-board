# Prototype fabrication and assembly package - revision 3

Prepared 22 October 2026 from the corrected saved KiCad design. **Unvalidated prototype:** no assembled-board performance is claimed. Final production files and placement confirmations require review before manufacture proceeds.

| File | Purpose |
|---|---|
| `Gerbers-drills.zip` | Four copper layers, masks, silkscreens, paste, board outline, drill files and Gerber job |
| `BOM.csv` | 66 fitted references with supplier part codes |
| `CPL.csv` | Top-side placement coordinates and supplier-adjusted rotations |
| `exact-parts.json` | Manufacturer, MPN and supplier identity for each fitted reference |
| `manifest.json` | Export hashes and saved-PCB identity |

The board is 80 × 70 mm, four layers, nominal 1.6 mm. The nominal JLC04161H-7628 construction is the documented design basis; confirm fabrication options with the selected manufacturer. Do not infer an impedance guarantee from nominal stackup values.

This revision includes the local C29 input bypass, revised J1 sourcing, the PG9 return-reference correction and routing-corner cleanup. See the [engineering review](../../docs/engineering-review.md), [current check reports](../../docs/validation.md) and [source manifest](../../docs/checks/manifest.json).

The CPL uses millimetres with the export's existing origin and signed Y coordinates. It includes seven header centroid adjustments and supplier-specific orientation corrections for headers and U1/U2/U4. Do not apply those corrections a second time or assume a different assembler uses the same rotation convention. Compare pin 1, diode polarity, connector holes and all placements against that assembler's final production files. J1 is the actual C55629812 header even if its preview model is absent.

Three PCB test points are not assembly BOM items. Earlier local export folders are not part of this public package. Check the manifest before reuse and regenerate/review exports after any hardware change.

Source manifests include both original-byte and LF-normalized hashes because Git may convert KiCad source line endings. Manufacturing exports and recorded check reports are stored without line-ending conversion to preserve their original hashes.
