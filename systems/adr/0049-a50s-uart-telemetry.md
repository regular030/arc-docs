# ADR-0049: A50S drive telemetry uses UART

- **Status:** Accepted (owner transport decision; firmware and harness qualification pending)
- **Date:** 2026-10-09
- **Deciders:** Kunshpreet Baweja, through the instruction “we don’t need can fd anymore, just uart” for A50S drive telemetry
- **Traces to:** SYS-07, SYS-19, SYS-24, SYS-25, SYS-29, E-30, E-50, RSK-03, RSK-12
- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-12
- **Supersedes:** ADR-0041 (drive FD requirement and local adapter proposal; the existing steering link is retained below), ADR-0043 (withdrawn A50S SPI/FD proposal)
- **Amends:** ADR-0034 (current UART baseline), ADR-0044 (motherboard interface scope)
- **Related:** ADR-0019 (moteus steering), ADR-0042 (grounding proposal), ADR-0047 (independent full stop)

## Context

Upstream ADR-0034 selected bought A50S controllers and one UART per controller. ADR-0041 subsequently required drive CAN-FD and proposed local MCP2518FD interfaces. The owner has now explicitly removed that drive-FD requirement and selected UART. The recent upstream UART baseline is 2 Mbit/s; its firmware endpoints are still stubs.

## Decision

1. Keep the four bought A50S V2.3c 12S controllers plus two spares, deck placement and existing speed constraints. **Drive commands remain on the separate 1 Mbit/s classic CAN trunk** under ADR-0009; UART carries drive status and link/clock traffic, not motor commands.
2. Use **four point-to-point UART links, one per A50S, at 2 Mbit/s, 8N1**, with the MAVLink 2 messages in `sync-link.xml`. Follow [controller-telemetry](../icd/controller-telemetry.md). Stream full status at ≥1 kHz per controller, retain sample timestamps, fault/state fields and receive-gap/reject reporting. Keep non-CAN load ≤50%, SYS-07 alignment and SYS-29 latency requirements.
3. Remove the front/rear drive CAN-FD trunks and all four A50S-local MCP2518FD/crystal/transceiver circuits from the active BOM, motherboard and harness. **No 5 Mbit/s drive telemetry requirement, PA7 breakout, local SPI target or FD IRQ/CS allocation remains.** Keep PB10 as A50S USART3 TX and PB11 as RX; actual connector contacts require arrival verification. Preserve Hall inputs, ESTOP, motor-temperature sensing, calibration EEPROM and SWD/USB recovery.
4. The existing **moteus-c1 steering CAN-FD link remains separate under ADR-0019**, at its recorded 1 Mbit/s arbitration / 5 Mbit/s data baseline. This A50S transport decision does not replace the steering actuator or its protocol. Its sync-side external MCP2518FD remains the BOM baseline; moving steering to a freed native H723 FDCAN is still an owner choice, not selected here.
5. Ground/reference topology, isolation, protection, power-off behavior and final motherboard UART pins remain **TBD**. Use 3.3 V UART logic; do not connect A50S supply outputs together or assume that spare H723 pins alone qualify the loaded harness. Resolve #94 / #115 using the UART-aware grounding review before layout release.
6. Keep the combined power/sync motherboard and separate Jetson carrier. **Independent physical standstill under ADR-0047 remains required**; UART, firmware braking and the delayed motor-bus cut do not implement the independent brake.

## Analysis and open implementation work

The unsigned full-length status frame is 47 payload bytes + 12 framing bytes. At 8N1 it occupies 590 wire bits, or **295 µs at 2 Mbit/s**; 1 kHz uses **29.5% per link**. One full-length unsigned link-status frame per second brings the nominal outbound load to **29.515%**, before extra traffic. This fits the ≤50% budget but is not measured acceptance. Signing, clock alignment and any added messages require a revised budget.

Both telemetry endpoints are stubs. Implement coherent acquisition, DMA/driver ownership, controller-to-sync clock mapping, explicit corner mapping (command IDs 1–4 to enum 0–3), protocol-version refusal, per-corner stale/fault handling, immediate forwarding and ROS publication. Preserve the existing command-timeout and latched-stop requirements. See the [UART readiness review](../reviews/a50s-uart-readiness.md) and [arrival/qualification procedure](../tests/a50s-arrival-and-fd.md).

The proposed harness counts **five conductors per controller, TBC**: TX/reference pair, RX/reference pair, plus one spare, or four runs / 20 conductors. Reference population and parallel-return current must be reviewed under ADR-0042; this is not a released pinout. The separate 111-conductor sensor budget is unchanged; neither subtotal is the complete car wire total.

## Consequences and verification

- Update the active requirements, BOM, wiring/topology, budgets, ICDs and EE handoff. Keep the abandoned FD audit/proposal marked deprecated; preserve accepted ADR decision bodies.
- On the installed harness, demonstrate four simultaneous ≥1 kHz full-status streams, ≤50% per UART, valid acquisition timestamps, required clock alignment and ≤2 ms p99 sample-to-Orin driver latency. Verify the ≤0.25 ms sync forwarding budget separately (ADR-0024).
- Qualify UART logic/ground margins under loaded motors and all relevant power states; inject corrupt/missing/version-mismatched frames using the reviewed fixture and verify SYS-19 / SYS-25 response. Steering CAN-FD is tested separately.
- This records the transport choice, not a firmware implementation, completed hardware test, interface sign-off or fab approval. No generated schema or firmware is changed by this documentation update.
