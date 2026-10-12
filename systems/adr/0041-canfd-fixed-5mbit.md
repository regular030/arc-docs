# ADR-0041: CAN-FD uses a mandatory 5 Mbit/s data phase

- **Status:** Superseded by ADR-0049
- **Date:** 2026-10-08
- **Deciders:** Kunshpreet Baweja, through the explicit instruction that 5 Mbit/s is a hard requirement
- **Traces to:** SYS-07, SYS-19, SYS-24, SYS-25, SYS-29, E-30, E-50, RSK-03, RSK-12
- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-12
- **Supersedes:** ADR-0036 (steering physical layer; lower-rate fallback no longer permitted)
- **Amends:** ADR-0034 (drive telemetry transport only), ADR-0011 (reintroduced front / rear FD topology)
- **Related:** ADR-0019 (dedicated steering bus), ADR-0047 (independent full-stop path)

> [!WARNING]
> **Deprecated:** Drive-FD requirement and A50S adapter proposal superseded by ADR-0049. Drive telemetry uses UART; the existing moteus steering CAN-FD link is retained. Historical decision text is preserved.

## Context

ADR-0011 originally assigned commands to classic CAN and full drive telemetry to separate front and rear CAN-FD buses. ADR-0034 replaced drive FD with UART; ADR-0036 retained steering FD and permitted a lower-rate fallback. The user now requires 5 Mbit/s. Reducing the rate or the required status stream cannot close the telemetry requirement.

Latest main accepts bought A50S V2.3c controllers on the lower deck, 9 m/s top speed and UART telemetry under ADR-0034. During this chat the user required drive CAN-FD at 5 Mbit/s, then explicitly retained the A50S when this conflict was identified. Retain the bought controllers, deck placement, speed cap, firmware target and fault-stop / reset decisions. Replace the UART transport plan with the proposed local FD integration below; exact pins and implementation still need owner review.

## Accepted requirement and proposed implementation

The mandatory operating rate and retained A50S choice are user decisions. The circuit, MCU peripheral allocation and PCB placement below are a reviewable implementation proposal; they remain TBC until approved by the electrical / firmware owners.

1. **Every ARC CAN-FD link operates at 1 Mbit/s arbitration and exactly 5 Mbit/s data, with bit-rate switching enabled.** This includes both drive-telemetry buses and the dedicated steering bus. The classic command bus remains at 1 Mbit/s. Other FD rates may be exercised for diagnostic experiments; they are not compliant operating modes or acceptance results.
2. **Keep commands and telemetry separate.** One classic trunk serves all four drive controllers. The front FD bus serves FL + FR; the rear FD bus serves RL + RR. Motor commands never travel over these telemetry buses. E-30 uses its three native FDCAN peripherals for those three links and its populated MCP2518FD for steering (ADR-0019).
3. **Keep one local FD interface per drive controller:** MCP2518FD, its own 40 MHz crystal, and a separate FD-capable SO-8 transceiver. The existing parts-list recommendation is TJA1462AT with 5 V VCC and 3.3 V VIO; the exact suffix and footprint are checked before assembly. For the bought A50S, four interface circuits on the ARC main/interface PCB beside the controllers are proposed. Each A50S MCU controls its own FD controller over short local SPI / CS / IRQ. Placement on the sync PCB versus an adjacent daughterboard, accessible pins, power, Hall coexistence and installed envelope remain TBC. No chassis-length SPI run or replacement custom VESC power stage is selected.
4. **Use the daisy-chain and termination arrangement from the physical-layer design.** Each drive FD pair runs sync board → near deck interface → far deck interface, passed through the local interface PCB on the deck. Terminate at the sync end and the far-end harness pigtail, using split termination (2 × 60 Ω plus capacitor, final capacitor TBC). Intermediate interfaces have no populated termination. Keep the transceiver tap local to the connector (1–2 cm target), route the pair over continuous ground away from switching nodes, and qualify the final routing / harness at 5 Mbit/s. The steering link retains ADR-0036’s point-to-point topology and end termination / rework options, with no lower-rate fallback.
5. **Retain physical-layer rework options:** split-termination footprint behind a jumper, common-mode choke bypassed with 0 Ω by default, and TVS/ESD footprints. If qualification fails, use SIC transceivers, correct grounding, shorter taps or tuned termination. A one-FD-bus-per-controller arrangement remains a last hardware fallback requiring extra sync-side controllers and owner review; it must still run at 5 Mbit/s and carry the full required stream.
6. **Generate telemetry inside the drive firmware.** Acquire a coherent full-status snapshot at ≥1 kHz per controller; carry sample time, per-corner sequence and fragment identity. The current budget reserves two 64-byte FD frames per snapshot; the final field layout / DBC remains TBD. Preserve the motor-control ISR: SPI and transmit work use DMA, buffering and a lower-priority task. ADR-0011's inherited SPI requirement is ≥20 MHz and the MCP2518FD maximum is 20 MHz: **20 MHz is the common operating point to qualify**. The drive MCU clock / divider setup is TBC; do not assume stock firmware can produce it or overclock the part. Any change to that inherited requirement needs owner review. Use receive / transmit controller timestamps for clock alignment, while retaining the actual sample timestamp (SYS-07, SYS-29).
7. **Acceptance includes the whole path:** ≤70% load on each CAN bus, no missing full-status snapshots in the specified soak, ≤2 ms p99 sampling-to-sync-MCU-driver latency on Orin (ADR-0024), and the required motor-loop timing. Per-corner missing data / health triggers SYS-25. Bench operation of an FD breakout alone does not qualify the A50S firmware integration.
8. **UART is outside this accepted design.** It cannot substitute for the required 5 Mbit/s CAN-FD link without an explicit future owner decision changing the requirement. A mainboard classic-CAN-to-FD bridge cannot recreate samples or timestamps absent from the upstream stream.

## Consequences and verification

- Update SYS-24, the corner ICD, parts handoff, topology page and RSK-03 test to state the mandatory operating rate.
- The implementation and remaining pin / firmware work are recorded in [drive CAN-FD telemetry](../icd/drive-canfd-telemetry.md).
- Run the [RSK-03 procedure](../tests/rsk-03-canfd-bench.md) at 5 Mbit/s with the actual final harness, two transmitters per bus, full traffic and a loaded motor. An error-free lower-rate test is a failure to meet this decision.
- Verify sample-time alignment and end-to-end latency separately from bus-clock accuracy. Preserve fault, sequence-gap, TEC/REC and bus-off logging.
- Existing UART firmware stubs and UART-only ICD details remain migration references, not qualified implementations of this requirement. The DBC / FD message schema and firmware migration are open; no generated firmware code is changed by this documentation PR.
- The independent brake E-34 and hardware drive inhibition remain mandatory under ADR-0047; this data interface does not implement them.

## Sources

[MCP2518FD datasheet](https://ww1.microchip.com/downloads/aemDocuments/documents/OTH/ProductDocuments/DataSheets/External-CAN-FD-Controller-with-SPI-Interface-DS20006027B.pdf), [TJA1462 datasheet](https://www.nxp.com/docs/en/data-sheet/TJA1462.pdf), [A50S listing and connector functions](https://teamtriforceuk.com/a50s-v2/). Component capability is evidence for implementation, not a substitute for the car's qualification tests.
