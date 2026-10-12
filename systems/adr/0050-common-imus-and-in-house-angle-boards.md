# ADR-0050: Common ISM330DHCX IMUs and in-house AS5047P boards

- **Status:** Superseded by ADR-0052 (ten angle, two IMU and four corner-height boards; all sensors assembled in house)
- **Date:** 2026-10-12
- **Deciders:** Kunshpreet Baweja, through the instructions to use ISM330DHCXTR for both IMUs and assemble separate AS5047P PCBs in house
- **Traces to:** E-20, E-21, E-22, E-23, E-24, SYS-06, SYS-07, SYS-10, SYS-18, SYS-24, SYS-29
- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-12
- **Related:** ADR-0008, ADR-0021, ADR-0044
- **Amended by:** ADR-0051 (custom ARC IMU and ground/ride-height PCB ownership)

## Context

The parts review previously recommended a precision CG IMU and an ISM330DHCXTR at the front. Remote angle boards were only a proposal and the cost model allowed factory assembly. The owner has selected a common IMU part and separate angle PCBs with in-house assembly, and requested Canadian-dollar funding figures for KEFC.

## Decision

1. Use **ST ISM330DHCXTR for both E-20 and E-21: two per car**, at the existing body/CG and front-axle measurement locations. Use a common remote IMU PCB design, primary SPI and hardware-timestamped data ready. The existing ≥4 kHz output requirement remains; the part provides a 6.667 kHz setting. Mounting, filtering, calibration and achieved estimation accuracy remain subject to qualification.
2. Use **AS5047P-ATSM on separate remote sensor PCBs assembled in house**: four wheel, four suspension and two front-knuckle boards, ten per car. A common 2-layer design is the costing proposal; outline, connector, magnet geometry, protection and any mechanical variants remain TBC/TBD. The motherboard holds the interfaces, not these sensing ICs. No local sensor MCU or CAN node is added.
3. Order bare angle PCBs from JLCPCB. Buy the loose chips, capacitors, connector and reviewed support parts from a component distributor; **do not use JLC's PCBA-only parts library as a loose-parts shipment source**. LCSC C962063 is the current costing source; distributor availability and final BOM must be rechecked before ordering. The two-PCB-set scenario budgets 20 populated angle boards plus two spare loose chips.
4. Keep the parts, wiring and cost records in Markdown, with machine-readable budget CSVs. Present the KEFC estimate in **CAD**, identify the exchange-rate date, apply the owner's **30% allowance once**, and separate JLC assembly from in-house consumables and loose-parts shipping. The funding estimate is a TBC planning model, not an approved purchase or a complete car budget.

## Consequences and qualification

One IMU design replaces two different footprints/drivers. The previous SCH16T recommendation is removed from the active build; its review can remain as a historical alternative. Selecting ISM330DHCXTR does not establish equivalence to a precision navigation IMU: demonstrate calibration, temperature drift, vibration response, clipping, installed-harness timing and velocity-through-slip performance against SYS-06, SYS-07, SYS-10 and SYS-29.

In-house angle assembly requires the actual support BOM, ESD/MSL handling, solder inspection, power checks, magnet alignment and angle/diagnostic tests. Schematic capture and fabrication release remain open. The minimum sensor harness remains 17 runs / 111 conductors TBC; common IMU parts do not remove separate chip selects or data-ready inputs.

Sources and pricing: [ST datasheet](https://www.st.com/resource/en/datasheet/ism330dhcx.pdf), [AS5047P manufacturer datasheet, distributor mirror](https://www.mouser.com/datasheet/2/588/asset_pdf_25493104-3419339.pdf), [loose AS5047P chips](https://www.lcsc.com/product-detail/C962063.html), [parts selection](../bom/pcb-parts-selection.md), [sensor board workspace](../../hardware/sensor-boards/README.md), [CAD funding estimate](../bom/jlc-pcba-cost-estimate.md).
