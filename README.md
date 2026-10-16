# STM32F446 Development Board

A custom STM32F446ZET6 development board designed in KiCad. This repository documents the hardware, design decisions, manufacturing review and eventual firmware and bench testing.

**Status: prototype — hardware validation pending.** The design has been prepared for a fabrication and assembly quote. No working-hardware result is claimed. The exact fabrication stackup and final assembler production files still need confirmation before ordering.

## Board overview

| Feature | Design |
|---|---|
| Microcontroller | STMicroelectronics STM32F446ZET6, LQFP-144 |
| PCB | 80 × 70 mm, four copper layers, nominal 1.6 mm |
| Intended stackup | JLC04161H-7628; manufacturer confirmation pending |
| Copper weight | Outer 1 oz, inner 0.5 oz |
| USB | USB-C connector carrying USB 2.0 full-speed device signals |
| Power | USB 5 V input, protection, TPS7A2533 3.3 V regulator and TPS22918 load switch |
| Debug | External SWD probe through a custom five-pin header |
| Interfaces | SPI, I²C, UART, analog and GPIO expansion headers |
| External clock | 8 MHz crystal |

The USB-C connector is intended for a USB device connection to a host. USB host operation and USB Power Delivery are outside the current scope. The TPS22918 is a load switch; it is not a programmable current limiter.

## Open the design

Use KiCad 10 and open [`stm32.kicad_pro`](stm32.kicad_pro). The project includes:

- [`stm32.kicad_sch`](stm32.kicad_sch): schematic.
- [`stm32.kicad_pcb`](stm32.kicad_pcb): board layout.
- [`stm32.kicad_dru`](stm32.kicad_dru): project design rules.
- `STM32_Custom.pretty`, `Library.pretty` and `STM32_Power.kicad_sym`: local footprint and symbol libraries.
- `fp-lib-table` and `sym-lib-table`: project-relative library paths.

Standard symbols and footprints require the normal KiCad libraries. Exact manufacturer part numbers are stored in the component sourcing fields. Fabrication outputs and assembler-specific placement files will be published as a reviewed release once the manufacturing review is closed.

## Design and review

Work on this board includes component selection, custom footprint checks, placement, decoupling, power distribution, signal routing, ground returns and preparation of assembly data. Peer feedback and AI-assisted reviews informed the iteration process. Bench results will be recorded separately from design calculations and software checks.

The latest recorded checks on 16 October 2026 reported zero active ERC violations, zero DRC violations, zero unconnected items and zero schematic parity issues. Seven previously reviewed ERC exclusions remain. These results describe the configured checks, not measured electrical performance.

The assembly preview revealed that assembler model rotations differed from KiCad footprint rotations. Corrections were incorporated into the placement file, reuploaded and checked after a fresh quote reload. KiCad footprint orientations were kept consistent with the schematic and PCB pad numbering.

### Remaining work

- Confirm the exact stackup with the fabricator; the saved quote currently specifies no stackup requirement.
- Review final manufacturing placement files, including pin 1, diode polarity and USB connector seating.
- Assemble and inspect the first prototypes.
- Measure power rails, startup current and regulator temperature.
- Establish SWD access, then test the clock, GPIO and USB with firmware.

The 250 mA total power budget used in the design review is conditional. It is not a measured continuous-output or thermal rating. Known layout compromises and bring-up dependencies are tracked in [`docs/validation.md`](docs/validation.md).

## Firmware and results

Firmware is not included yet. A successful build, debug connection or USB enumeration will only be marked complete when supporting test evidence exists. The initial test log is in [`docs/test-log.md`](docs/test-log.md).

## References

- [ST STM32F446ZE documentation](https://www.st.com/resource/en/datasheet/stm32f446ze.pdf)
- [TI TPS7A25 regulator datasheet](https://www.ti.com/lit/ds/symlink/tps7a25.pdf)
- [TI TPS22918 load-switch datasheet](https://www.ti.com/lit/ds/symlink/tps22918.pdf)

## License

A hardware and documentation license has not yet been selected. This repository does not currently grant an explicit open-source license. Third-party library assets remain subject to their respective licenses.
