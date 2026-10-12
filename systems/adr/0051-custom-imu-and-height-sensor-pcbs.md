# ADR-0051: ARC designs its own IMU and height-sensor PCBs

- **Status:** Accepted (owner PCB ownership/packaging choice; schematics and layouts pending)
- **Date:** 2026-10-12
- **Deciders:** Kunshpreet Baweja, through the instruction to use our own IMU PCB and our own ground-height sensor PCBs
- **Traces to:** E-20, E-21, E-26, SYS-06, SYS-07, SYS-10, SYS-15, SYS-24, SYS-29, RSK-07
- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-12
- **Amends:** ADR-0050 (explicit ownership of remote IMU/height boards; existing part and angle-assembly choices retained)
- **Related:** ADR-0007, ADR-0008, ADR-0021, ADR-0044

## Context

The preceding parts and funding records already allowed common remote IMU and ToF carrier designs, but some wording still allowed supplier modules or left PCB ownership only proposed. The owner has explicitly selected ARC-designed PCBs for these sensors.

## Decision

1. Design **our own remote IMU PCB**, reused at the existing body/CG and front-axle sites: **two populated boards per car**. Both use the selected **ISM330DHCXTR** from ADR-0050. These sensing ICs and local support circuits sit on the remote boards; acquisition/timestamping remains at the motherboard's H723 through SPI and dedicated chip-select/data-ready signals.
2. Design **our own remote ground/ride-height ToF PCB**, with **five populated boards per car**: four corner ride-height sites and one ground-speed scale site beside E-25. A common reusable design is the cost assumption; mechanical variants remain TBC. VL53L4CDV0DH/1 remains the preferred costing recommendation, not a newly confirmed sensor-IC selection from this packaging instruction. The final variant, optics and mechanical mounting still require review against E-26.
3. Add no local sensor MCU or CAN node. Retain the existing direct sensor interfaces, proposed I²C mux arrangement, 17-run / 111-conductor sensor-harness minimum TBC, and one combined power/sync motherboard with a separate Jetson carrier. Layout/stackup, connector contacts, protection, reset/sync wiring and exact support BOMs remain TBC/TBD.
4. **In-house assembly remains the selected process for AS5047P angle boards only.** JLC Standard PCBA for the custom IMU and ToF boards remains the existing cost assumption; final production process is confirmed with the released BOM/panel quote. This packaging decision does not extend in-house assembly to the LGA IMU or optical ToF package.
5. Retain the current CAD funding model and 30% contingency: it already prices custom sensor ICs, support parts, bare PCB fabrication and assembly. No bought breakout-module cost is added. Requote if final outlines, stackups, mechanical variants, population or assembly process change.

## Design work and verification

The [IMU workspace](../../hardware/sensor-boards/imu/README.md) and [height/scale workspace](../../hardware/sensor-boards/tof/README.md) are separate from the motherboard and contain design briefs, not released CAD. Complete support schematics, reviewed PCB/mount envelopes, return/protection design and connector/net schedules before layout/fabrication.

IMUs retain the sample-rate, timestamp, calibration, vibration and slip-estimation qualification from ADR-0050. ToFs require minimum-distance checks at full compression, glossy-tile/cover-window testing, optical clearances, independent mux addressing and reviewed startup/reset handling. Existing fab and interface review gates apply.

See [board ownership and parts](../bom/pcb-parts-selection.md), [wiring schedule](../bom/motherboard-wiring.md), [CAD funding estimate](../bom/jlc-pcba-cost-estimate.md), and [sensor hardware overview](../../hardware/sensor-boards/README.md).
