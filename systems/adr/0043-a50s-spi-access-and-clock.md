# ADR-0043: A50S local SPI access and a stock-clock FD interface

- **Status:** Superseded by ADR-0049 (withdrawn proposal; not implemented or qualified)
- **Date:** 2026-10-09
- **Deciders:** kunshpreet
- **Traces to:** SYS-07, SYS-19, SYS-24, SYS-29, E-50, ADR-0041, RSK-03, RSK-12
- **Would amend:** ADR-0041's inherited SPI speed target; its mandatory 5 Mbit/s CAN-FD requirement is retained
- **Issue:** [#100](https://github.com/Autonomous-Robotics-Carleton/arc-monorepo/issues/100)

> [!WARNING]
> **Deprecated:** Withdrawn by ADR-0049: A50S drive telemetry uses UART. No PA7 breakout, local SPI clock, PB10 IRQ or PB11 CS is required by the active design.

## Context

The A50S uses a 168 MHz system clock and an 84 MHz SPI1 peripheral clock. SPI1 divided by eight gives 10.5 MHz; divided by four gives 21 MHz, above MCP2518FD's 20 MHz rating. Changing the system PLL would introduce USB and motor-timing work. The [source audit](../reviews/a50s-fd-feasibility.md) also distinguishes real SPI1 MOSI, PA7, from the published TX/MOSI connector position mapped to PB10.

## Proposed decision

Use **SPI1 at 10.5 MHz with DMA**, retaining the stock system/USB/motor clock tree. The MCP2518FD retains its separate 40 MHz oscillator and **1 Mbit/s arbitration / exactly 5 Mbit/s data with BRS**. The local SPI clock and CAN signaling clock are separate; lowering SPI to a rated operating point does not lower the FD rate.

This is an explicit proposed amendment to the inherited SPI target. Until this ADR is accepted, ADR-0041's 20 MHz target remains the recorded requirement; the calculation below does not silently waive it.

| Adapter net | A50S MCU signal | Proposed implementation / condition |
| --- | --- | --- |
| FD_SCK | PA5, SPI1 AF5 | Published ADC1/SCK position after continuity confirmation; disable competing external-ADC application and configure digital SPI |
| FD_MISO | PA6, SPI1 AF5 | Published ADC2/MISO position after continuity confirmation |
| FD_MOSI | **PA7, SPI1 AF5** | Verify a connected pad/test point. For an actual STM32F405 LQFP64, PA7 is package pin 23; this is **not** a Pico-Clasp circuit number. If absent from the connector, a reviewed local PA7 breakout/rework is required. No connector-only implementation is established |
| FD_CS_N | PB11, GPIO output | Disable USART3/I2C2 applications on this pair; define inactive CS during startup and power-off |
| FD_INT_N | **PB10, EXTI10 input** | Repurpose the UART TX/SCL position after continuity/loading checks; configure active-low interrupt behavior according to MCP2518FD and the reviewed board circuit |

Preserve PC6/PC7/PC8 Halls, PB6 ESTOP, SWD, calibration EEPROM, classic command CAN and temperature sensing. PB10's EXTI10 uses EXTI15_10, separate from the existing Hall/encoder EXTI8 vector. `HAL_USE_EXT` is currently false: the driver must explicitly enable/configure the ChibiOS external-interrupt path or implement reviewed vector ownership, without defining a duplicate handler. Keep IRQ callbacks short; perform SPI and packet work in a lower-priority task.

SPI1's declared DMA2 stream 0 RX / stream 3 TX differ from the configured ADC streams. SDC also declares stream 3, but `HAL_USE_SDC` is false. Keep SDC disabled and verify the final enabled build, DMA buffers and priorities; source assignments alone do not prove motor-loop timing.

Grounding/isolation and the FD 3.3 V / transceiver 5 V supply design remain **TBD**, deferred by the owner. Do not parallel controller outputs or assume their spare capacity. This proposal does not release those parts of the schematic.

## Transfer-budget analysis

Under SYS-24 / ADR-0041, each controller produces at least 1,000 complete snapshots/s; the current budget reserves two 64-byte FD frames per snapshot. A transmit message object has an 8-byte header plus 64-byte payload. A CRC-protected RAM write adds two command/address bytes, one length byte and two CRC bytes: **77 SPI bytes per frame**, or **154 per snapshot**. Two conservative CRC-protected 32-bit FIFO-control writes add **18 bytes**, giving **172 bytes** for that transmit path.

Propose a **256 aggregate SPI-byte budget per 1 ms cycle**, including reads, transmit-event service and synchronization traffic. The remaining **84 bytes** are an allowance to validate against the actual driver, not measured overhead. At 10.5 MHz:

`256 × 8 / 10,500,000 = 195.05 µs` of wire time, or **19.50%** of a 1 ms period.

This demonstrates transfer-capacity feasibility for the stated workload. It excludes software scheduling, CS gaps, retries and CAN arbitration; it does not prove end-to-end latency, timestamp alignment or motor-loop safety. Measure those separately. Recalculate if the schema, overhead or acquisition rate exceeds this assumption; do not trim required telemetry to fit.

## Release conditions

1. Owner accepts the SPI-target amendment and the PA7 access/rework approach. Record the actual shipped revision, contact map, package and photographs/continuity evidence before making a harness.
2. Implement and review SPI/DMA/IRQ ownership, coherent source timestamps, CRC/sequence checks, buffering and overflow response. No firmware is changed or flashed by this ADR.
3. Resolve the deferred supply/ground interfaces before powering the adapter.
4. On one spare, measure SPI frequency/timing and total service time; prove Hall/ESTOP/recovery remain functional. Then qualify full snapshots at 1 kHz, actual CAN at exactly 5 Mbit/s, load <=70%, <=2 ms p99 sample-to-Orin latency, SYS-07 alignment and unchanged motor-loop deadlines under load.

**Remaining physical blocker:** a remote source audit cannot establish accessible PA7 copper on the supplied controller. If local breakout/rework cannot be qualified, this proposal cannot be released; a different owner-approved integration architecture is required.

Sources: [ST STM32F405 datasheet, pin/AF tables](https://www.st.com/resource/en/datasheet/stm32f405rg.pdf), [Microchip MCP2518FD datasheet, transmit objects and CRC SPI instructions](https://ww1.microchip.com/downloads/aemDocuments/documents/OTH/ProductDocuments/DataSheets/External-CAN-FD-Controller-with-SPI-Interface-DS20006027B.pdf), and the vendored source locations in the [audit](../reviews/a50s-fd-feasibility.md).
