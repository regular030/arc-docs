# A50S FD integration audit

> [!WARNING]
> **Deprecated:** A50S drive-FD proposal withdrawn by ADR-0049. Use the active [UART ICD](../icd/controller-telemetry.md) and [UART readiness review](a50s-uart-readiness.md). The body below is historical, not an active layout or acceptance requirement.

- **Date:** 2026-10-09
- **Owner:** kunshpreet
- **Status:** Deprecated / historical drive-FD reference after ADR-0049
- **Issue:** [#100](https://github.com/Autonomous-Robotics-Carleton/arc-monorepo/issues/100)
- **Traces to:** SYS-05, SYS-07, SYS-13, SYS-19, SYS-24, SYS-29, ADR-0034, ADR-0041, RSK-11, RSK-12, RSK-20

## Finding

Keep the bought A50S V2.3c 12S controller selected by ADR-0034. Four local FD circuits may be placed on the main/interface PCB, but a complete high-speed SPI connection through its published signal connector has **not** been verified. ADR-0043 proposes the clock operating point and interrupt allocation; physical PA7 access and the final enabled firmware remain unqualified.

This audit compares the vendored `a50s_v23c_12s` hardware definition with ST's alternate-function tables and the manufacturer connector illustration. Firmware declarations are evidence of the intended MCU assignment, not a continuity test or a board schematic. No upstream firmware or generated code is changed here.

## Proposed resolution, 2026-10-09

ADR-0043 proposes **stock SPI1 / 8 = 10.5 MHz**, real **PA7 MOSI** through a verified pad/local breakout, **PB11 CS** and **PB10 EXTI10 IRQ**. Its conservative 256-byte SPI cycle needs 195.05 µs at 1 kHz, before scheduling/gaps/retries; CAN-FD retains its separate 40 MHz source and mandatory 5 Mbit/s data phase. This is a quantified proposal, not measured acceptance or a released connector pinout. Owner acceptance would amend the inherited 20 MHz SPI target. Physical PA7 access remains the outstanding pin question; grounding and supply choices are deferred TBD.

## Pins and resource conflicts

| Function | Vendored MCU assignment | Connector / resource consequence |
| --- | --- | --- |
| SPI1 clock | PA5 | Also external ADC1; connector bottom-row SCK/ADC1 is a candidate after continuity confirmation. GPIO initialization currently sets PA5 to analog mode. Reconfigure it for SPI and disable the external-ADC application using it. |
| SPI1 input | PA6 | Also external ADC2; bottom-row MISO/ADC2 is a candidate after confirmation. Same analog-mode/application conflict. |
| SPI1 output | PA7 | Firmware declares PA7. The published bottom-row TX/SCL/MOSI position corresponds to PB10 in the UART definition. ST lists PB10 as SPI2_SCK, not SPI1_MOSI. **Do not wire that connector label to MCP2518FD MOSI by assumption.** Confirm the actual PCB connection / accessible PA7 pad with the supplier or continuity measurement. |
| Software chip select | PB11 | Also USART3 RX / I2C2 SDA. A GPIO chip select is possible in principle; disable competing UART/I2C applications. Physical circuit number remains provisional. |
| FD interrupt | PB10 / EXTI10 proposed in ADR-0043 | Requires verified PA7 MOSI access and disabled USART3/I2C2 use. The source declares HAL_USE_EXT false; enable/configure the interrupt path and verify one EXTI15_10 handler and its priority in the final build. No physical contact allocation is released. |
| Motor Halls | PC6, PC7, PC8 | Preserve all three. Do not reuse the top-row Hall/SPI labels for FD while Hall-sensored operation is required. |
| ESTOP / PPM | PB6 | Preserve as the proposed ESTOP input; electrical polarity / pull-down / board loading remain subject to the stop design. |
| SWD | Published SWDIO / SWCLK contacts | Preserve recovery access. Do not use SWD as spare FD I/O. |
| Calibration EEPROM | PC13 / PB2, software I2C | Preserve; these are not spare interrupt pins. |

Source locations: [hardware pin definitions](../../firmware/vesc/bldc/hwconf/teamtriforceuk/a50s_v23c/hw_a50s_v23c_core.h), [GPIO and ADC initialization](../../firmware/vesc/bldc/hwconf/teamtriforceuk/a50s_v23c/hw_a50s_v23c_core.c), [clock / DMA configuration](../../firmware/vesc/bldc/hwconf/mcuconf.h), [encoder implementation](../../firmware/vesc/bldc/encoder/encoder.c). The encoder's mode labelled hardware SSC still constructs a software SPI configuration; its name is not evidence of an implemented high-speed FD driver.

## Clock blocker

The target selects a 25 MHz external clock, PLL M = 25, N = 336, P = 2, Q = 7, APB1 divided by 4 and APB2 divided by 2. Calculation from the checked configuration:

| Clock | Calculated frequency | Consequence |
| --- | --- | --- |
| System | 168 MHz | Current target clock; not changed by this PR |
| APB2 / SPI1 | 84 MHz | Divider 4 gives 21 MHz; divider 8 gives 10.5 MHz |
| APB1 / SPI2 and SPI3 | 42 MHz | Divider 2 gives 21 MHz; divider 4 gives 10.5 MHz |

MCP2518FD's maximum SPI clock is 20 MHz. **21 MHz is outside its rating, and the stock clock tree cannot produce exactly 20 MHz through the ordinary power-of-two SPI prescalers.** The inherited proposed ≥20 MHz SPI target in ADR-0041 therefore has no qualified stock-clock operating point.

Owner review must choose a supported implementation: a clock-tree change that preserves USB, ADC/PWM, timing and motor-control behavior, or an explicit revision of the proposed SPI target with a measured throughput/latency justification. A lower SPI clock is not a lower FD data rate; the CAN-FD data phase remains **exactly 5 Mbit/s**, with full ≥1 kHz snapshots and the SYS-24 / SYS-29 limits. No clock change or SPI-target relaxation is selected here. Bit-banging is not a verified substitute for DMA-driven SPI.

SPI1's configured DMA streams are DMA2 stream 0 RX and stream 3 TX, distinct from the configured ADC streams 4/2/1. SDC also declares stream 3 but HAL_USE_SDC is false. ADR-0043 requires keeping SDC disabled and checking the final enabled drivers, buffer ownership, priorities and motor-loop interference; declared stream numbers alone do not establish availability.

## Main/interface PCB conditions

- One MCP2518FD circuit is controlled by each A50S MCU. The sync MCU must not replace the missing source samples by polling stock classic CAN.
- Keep each SPI link local to its controller and reference it to that controller's return domain. Do not join four A50S 3.3 V / 5 V outputs on a shared PCB. Supply capability, extra FD load, regulator choice and power sequencing remain TBC.
- A shared interface PCB needs an explicit return map under the [grounding proposal](../adr/0042-grounding-and-isolation.md). Firmware/connector mux, PCB access, clock and rail loading must all be resolved before routing this circuit.
- Retain Hall inputs, ESTOP, classic command CAN, motor-temperature sensing, calibration EEPROM and SWD. FD does not supply the independent brake required by ADR-0047.

## UART update review, 2026-10-09

Checked against upstream `67392fa`. The recent UART changes are documentation/schema work: `3b2ee68` sets the legacy link to **2 Mbit/s**, `419cd5e` adds controller braking state and command-loss behavior, and `84a6806` defines receive-gap counting and the version-mismatch response. They do not implement the controller telemetry path. ADR-0041 still requires direct drive CAN-FD at **1 Mbit/s arbitration / exactly 5 Mbit/s data with BRS**; the UART plan is deprecated as the operating transport.

### Rate and PCB consequences

| Check | Source evidence / result | EE implication |
| --- | --- | --- |
| A50S baud rate | USART3 uses APB1 at 42 MHz. The stock serial configuration clears `CR1`, and the USARTv1 driver writes `BRR = PCLK1 / speed`. At 2 Mbit/s, `BRR = 21`; this is an exact nominal divisor with 16× oversampling. Clock tolerance and installed-harness operation remain unqualified | Retain 2 Mbit/s only as the legacy bring-up baseline; changing the baud setting alone does not produce ARC telemetry |
| UART load, SYS-24 | The current `ARC_MOTOR_STATUS` payload is 47 bytes. An unsigned, full-length MAVLink 2 frame is 59 bytes; 8N1 uses 10 wire bits per byte. `59 × 10 / 2,000,000 = 295 µs`, or **29.5%** per controller at 1 kHz. A full-length unsigned `ARC_LINK_STATUS` is 30 bytes at 1 Hz, bringing the nominal controller-to-sync load to **29.515%**, before any additional traffic | Nominal bandwidth fits the legacy ≤50% ceiling. This is a calculation for four separate UARTs, not measured timing or FD acceptance |
| A50S pin ownership | USART3 TX is **PB10**, RX is **PB11**. ADR-0043 proposes those same pads as **FD IRQ** and **FD CS** respectively | UART and the proposed FD circuit cannot operate simultaneously on this pair. Do not connect an active UART transmitter/receiver alongside the FD circuit; any service-mode isolation, switching or alternative pin access remains TBD |
| Motherboard pin tally | The old H723 feasibility example includes four controller UARTs plus debug, but predates the final FD/steering assignment | Required debug UART is separate from optional A50S service access. Recheck the full pin map; spare H723 UARTs do not remove the A50S pin conflict |
| UART electrical interface | The legacy ICD specifies 3.3 V logic and a reference conductor; ground-shift and supply/power-off behavior have not been qualified | No 5 V logic assumption or released service connector. Grounding/isolation stays TBD under ADR-0042; optional service wiring is excluded from the current operating harness totals |

Baud evidence: [A50S pin definitions](../../firmware/vesc/bldc/hwconf/teamtriforceuk/a50s_v23c/hw_a50s_v23c_core.h), [clock configuration](../../firmware/vesc/bldc/hwconf/mcuconf.h), [stock UART application](../../firmware/vesc/bldc/applications/app_uartcomm.c) and [serial driver](../../firmware/vesc/bldc/ChibiOS_3.0.5/os/hal/ports/STM32/LLD/USARTv1/serial_lld.c). The stock application uses VESC packet processing; it is not the proposed MAVLink status publisher. Its baud rate is configurable, so its compiled 115200 default is not a newly selected ARC operating rate.

### Implementation gaps

| Stage | Current repository implementation | Still required |
| --- | --- | --- |
| A50S acquisition/transmit | [`arc_telemetry_init()`](../../firmware/vesc/arc/src/telemetry.c) only returns `0`; its [header](../../firmware/vesc/arc/include/arc/telemetry.h) explicitly calls it a stub. Stock `HAL_USE_UART` and `STM32_UART_USE_USART3` are false; the serial driver owns USART3 | Coherent sampling, source timestamps, chosen transport driver, DMA/IRQ ownership and loaded motor-loop validation. A successful init return does not prove streaming |
| Sync MCU controller receive | [`controller_links_init()`](../../firmware/sync-mcu/app/src/controller_links.c) logs “not implemented yet” and returns `0`; the [`arc_sync` board](../../firmware/sync-mcu/boards/arc/arc_sync/arc_sync.dts) has no released peripheral/pin map | FD receive/reassembly, per-corner loss/reject counters, clock mapping and coordinated fault response; optional UART bring-up would need its own configuration |
| Sync-to-Orin path | [`sync_link.c`](../../firmware/sync-mcu/app/src/sync_link.c) exchanges link status under socket configuration and sleeps 10 ms per loop. [`arc_backend_car`](../../ros/src/arc_backend_car/src/backend.cpp) currently handles link-status discovery | Motor-status forwarding and ROS publication, protocol-mismatch command refusal, plus measurements against the ≤0.25 ms forwarding / ≤2 ms p99 end-to-end limits (ADR-0024 / SYS-29). Existing discovery is not evidence of those limits |
| Message schema and IDs | [`sync-link.xml`](../icd/sync-link.xml) defines the status/fault/state fields but its motor-status descriptions still say UART. `ARC_CORNER` is **0–3**, while command-bus controller IDs are **1–4** | Explicit corner-ID mapping and transport-independent timestamp semantics. FD IDs/fragment layout and controller-to-sync clock alignment remain TBD; do not use the command ID directly as the corner enum |
| Tests | [VESC host tests](../../firmware/vesc/arc/tests/test_stubs.c) check stub return values; the [sync-link test](../../firmware/sync-mcu/tests/sync_link/src/main.c) round-trips link status | These do not test UART baud/harness, motor telemetry, clock alignment, version refusal, stale-corner stopping or the mandatory FD path |

The [firmware overview](../../firmware/README.md), [sync MCU README](../../firmware/sync-mcu/README.md), [VESC README](../../firmware/vesc/README.md) and [software architecture](../software/architecture.md) still describe the UART scaffold. Treat those transport statements as migration references where they conflict with ADR-0041. Migrate the schema and software documentation with the eventual implementation; schema changes must go through `tools/gen-interfaces.sh` and regenerate both firmware and ROS outputs. This review changes no firmware, generated code or accepted ADR decision text.

## Arrival and supplier evidence still required

Use the [arrival procedure](../tests/a50s-arrival-and-fd.md). Obtain the shipped revision / schematic or an approved continuity map; verify the provisional connector orientation, MOSI, interrupt and output-rail capacity. The current supplier page describes Aux input as 6–52 V, while the inspected connector illustration / existing ICD records 12–48 V. Resolve that discrepancy before selecting an Aux feed; neither range is newly baselined here.

Also confirm the selected motor's compatible built-in temperature sensor and curve (ADR-0046), termination arrangement, supplied capacitor, loaded motor current / speed / temperature and heatsink envelope. These are pending measurements, not completed #100 acceptance.

## Primary references

- [A50S manufacturer page](https://teamtriforceuk.com/a50s-v2/) and [connector illustration](https://cdn11.bigcommerce.com/s-4t55i5fv4j/images/stencil/original/image-manager/pinout-v2.3.png?t=1684596909).
- [ST STM32F405/407 datasheet](https://www.st.com/resource/en/datasheet/stm32f405rg.pdf), pin definitions / alternate functions: PA7 and PB10.
- [Microchip MCP2518FD datasheet](https://ww1.microchip.com/downloads/aemDocuments/documents/OTH/ProductDocuments/DataSheets/External-CAN-FD-Controller-with-SPI-Interface-DS20006027B.pdf), SPI rating and clock requirements.
