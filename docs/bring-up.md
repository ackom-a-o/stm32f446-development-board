# First power-up and bench validation

This checklist is a plan, not a record of completed tests. All hardware results remain **not tested**. Record board revision, instruments, supply settings, firmware commit, readings and failures in the [test log](test-log.md).

## 1. Inspect before connecting power

- Compare the assembled board with the schematic and approved production files. Inspect MCU fine-pitch pins, regulator/load-switch footprints, USB pins and shield joints for bridges, opens and damage.
- Confirm U1/U2/U3/U4 pin 1, D1-D5 polarity, capacitor values/packages and the populated C29. Check header orientation and pin numbering from the PCB rather than assuming a preview model is authoritative.
- With power and probe disconnected, check for shorts between USB input, `VREG_3V3`, `+3V3` and GND. Capacitors can cause changing resistance readings; investigate persistent low resistance before applying power.
- Verify the custom debug adapter: J1 pin 1 = +3V3 target reference, 2 = SWDIO, 3 = GND, 4 = SWCLK, 5 = NRST. Confirm the probe's own connector mapping separately.

## 2. Apply controlled power

Disconnect expansion loads. Use a current-limited, regulated 5 V source through a suitable USB breakout, preserving the board's input protection. Do not connect an independently powered USB host or allow the SWD probe to supply a second rail at the same time.

Choose an initial current limit from the expected unloaded-board consumption and instrument behaviour; a limit that is too low may prevent startup. If the source remains in current limit, voltage collapses or anything heats unexpectedly, disconnect and investigate instead of raising the limit blindly.

Measure USB input, the protected input to U3, `VREG_3V3`, switched `+3V3` and the MCU supply pins. Compare rail tolerances and regulator headroom with the component datasheets. Check both STM32 VCAP nodes against the MCU requirements; they are internal regulator outputs and must not be used as external power rails. Record steady current and startup waveforms where available.

## 3. Establish SWD and reset

Connect a correctly mapped external SWD probe using short leads and the target's measured reference voltage. Start with a conservative SWD clock. Confirm device identification, connect-under-reset if needed, and repeat reset/power cycles. Read boot configuration and confirm expected boot behaviour before changing option bytes. Avoid irreversible protection settings during bring-up.

## 4. Run minimal firmware

Begin with the internal oscillator to separate basic programming/power issues from external-crystal startup. Use a known MCU pin and harmless LED load or test point for a simple GPIO toggle; check the actual pin mapping before driving it. Record the firmware commit and toolchain.

Then enable the external 8 MHz clock and verify startup/frequency through a suitable buffered clock output where possible. Direct probing can load a crystal and change its behaviour. Confirm reset, boot and repeated starts before adding USB or external devices.

## 5. Test USB and expansion

Implement the PA9 divider-based VBUS input configuration according to [ST AN4879](https://www.st.com/resource/en/application_note/an4879-introduction-to-usb-hardware-and-pcb-guidelines-using-stm32-mcus-stmicroelectronics.pdf); do not assume default dedicated VBUS sensing matches this circuit. Test full-speed enumeration, connect/disconnect cycles, data transfers and power cycling with versioned firmware. Record host logs and failures.

Verify GPIO, SPI, I²C, UART and analog headers one interface at a time. Check pull-ups, voltage compatibility and output slew. CAN testing requires an external 3.3 V logic-compatible transceiver, correctly placed termination and a suitable second node. Do not connect motors, 24 V signals or inductive loads directly to MCU headers.

## 6. Establish the operating envelope

After basic operation, increase load in controlled steps within component and USB supply constraints. Record rail droop, startup, load-step response and regulator temperature at known ambient temperature. The conditional 250 mA total design budget includes MCU and expansion loads; it is not a validated external-load allowance.

Repeat power/reset/USB tests at intended loads. Publish measurements, anomalies and corrective changes before describing the board as validated. Manufacturing review, a clean DRC and removal of right-angle bends do not replace these tests.
