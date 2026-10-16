# ADR-0052: One-car sensor quantities and in-house sensor assembly

- **Status:** Accepted (owner quantities, locations and assembly choices; design and qualification pending)
- **Date:** 2026-10-12
- **Deciders:** Kunshpreet Baweja, through the instructions to fund one of each core PCB, retain all ten angle boards and fit two IMU and four corner-height boards, all sensors assembled in house
- **Traces to:** E-02, E-20, E-21, E-22, E-23, E-24, E-26, E-30, E-41, E-46, SYS-06, SYS-07, SYS-10, SYS-15, SYS-19, SYS-24, SYS-25, SYS-29, RSK-07
- **Supersedes:** ADR-0007, ADR-0050, ADR-0051
- **Related:** ADR-0008, ADR-0014, ADR-0019, ADR-0021, ADR-0044, ADR-0047, ADR-0034
- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-12

## Context

The previous funding model priced a populated spare PCB set, five ToF boards per car, and factory assembly for the IMU and ToF boards. The owner has confirmed all ten existing angle measurement locations, two IMUs and four corner-height ToFs, with in-house assembly for all three sensor families and JLC factory assembly retained for the carrier and motherboard. This is the final one-car build scope. Supplier minimum production quantities must be distinguished from installed quantities.

## Decision

1. Use **one installed Jetson carrier and one installed combined power/sync motherboard** for one car, retaining **JLC factory assembly for both core designs**. The carrier remains separate under ADR-0044. Show the costing scenario's minimum **two factory-populated boards per core design** separately: one installed and one minimum-run surplus. Include those minimum-run charges without describing them as a second car or a complete populated spare PCB set. Bare-board fabrication minimums may also leave unused boards; final quotes must identify installed, fabricated and populated quantities separately.
2. Retain **ten AS5047P-ATSM angle boards**: four wheel-angle boards under E-22, four suspension-angle boards under E-23 and two front-knuckle boards under E-24. ARC designs separate remote PCBs and assembles them in house. Retain direct wheel/suspension acquisition under ADR-0008 and the absolute steering references under ADR-0019; no angle measurement location is removed.
3. Fit **two ST ISM330DHCXTR boards**, one at the body/CG under E-20 and one at the front axle under E-21. ARC designs one reusable remote IMU PCB and assembles both boards in house. Retain SPI, dedicated chip selects, hardware-timestamped data ready and the existing sampling/qualification requirements.
4. Fit **four ARC-designed corner-height ToF boards**, one at each corner under E-26, assembled in house. Remove the fifth dedicated scale board beside E-25. VL53L4CDV0DH/1 remains the preferred costing candidate, with final sensor choice and installed optical qualification TBC. The custom PCB and quantity choices do not independently qualify that IC.
5. For **all sixteen sensor boards**, buy bare PCBs from JLCPCB and loose sensing ICs, connectors and reviewed support parts from a component distributor. JLC's PCBA-library prices do not constitute loose-component supply. Remove factory sensor setup, placement, fixtures and X-ray fees from this funding model; allow shared in-house consumables instead. The costing scenario fabricates **ten angle, five IMU and five ToF bare boards**, populating only **ten angle, two IMU and four ToF boards**. Final fabrication minimums, support BOMs, process and stock must be checked before ordering.
6. Keep direct acquisition at the motherboard's H723, with no corner sensor MCU or CAN node. The unchanged minimum cable conventions give **sixteen sensor runs / 106 conductors TBC: 90 used + sixteen spares**. Ten angle runs contribute 70 conductors, two IMU runs 16 and four ToF runs 20. Final connectors, extra returns, shields and reset/interrupt needs can increase this minimum.
7. Present the revised one-car PCB estimate in CAD, state the supplier currency and exchange-rate date, and apply the owner's 30% contingency once. Do not retain the earlier two-set headline or add the sensor ICs twice. The estimate remains a planning allowance subject to the final bare-PCB, core-PCBA and loose-component quotes.

## Consequences and open items

The selected sensor inventory is **ten angle + two IMU + four corner-height boards**, all assembled in house. The costing scenario leaves four unused bare sensor boards: three IMU and one ToF. No spare sensor ICs or populated sensor boards are included unless separately approved. Minimum-run factory surplus for the core boards is itemized separately; it does not double the installed sensor quantities. Wheel, suspension and knuckle measurement functions remain selected and still require qualification.

Removing the fifth ToF leaves the derivation of **height at the downward ground-speed camera from the four corner readings TBD**. Review camera location, chassis geometry, attitude, tire/suspension effects and surface response, then qualify ground-speed scale under SYS-10 and RSK-07. This quantity decision does not establish that the previous scale measurement or estimator performance is fulfilled.

In-house population of the IMU and optical ToF packages requires reviewed footprints, assembly/reflow instructions, ESD/MSL handling, inspection and power/interface tests. Complete the support BOMs and mounting/protection details, then qualify wheel magnet alignment, sensor timestamps, vibration/calibration, glossy-floor response and the installed SPI/I²C harness. No schematic, layout or fabrication package is released by this decision.

See the [sensor workspace](../../hardware/sensor-boards/README.md), [parts list](../bom/pcb-parts-selection.md), [wiring schedule](../bom/motherboard-wiring.md), [CAD funding estimate](../bom/jlc-pcba-cost-estimate.md) and [fab gate](../reviews/fab-gate.md). Requirements, firmware and generated interface schemas are unchanged; reconcile ground-camera scale and the implementation inventory before release.
