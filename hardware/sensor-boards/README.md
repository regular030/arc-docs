# Remote sensor carrier PCBs

- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-12

**Updated 2026-10-12.** ARC designs all three remote sensor PCB families: **AS5047P angle boards**, **our own ISM330DHCXTR IMU PCB**, and **our own ground/ride-height ToF PCB** (ADR-0050 / ADR-0051). The custom-board ownership is selected; no schematic or layout is released. A common 2-layer design per family is the costing proposal; actual outlines, connectors, mechanical variants and stackups remain TBC/TBD.

| Board family | Installed per car | Assembly plan | Release work |
| --- | --- | --- | --- |
| AS5047P angle carrier | **10**: 4 wheel + 4 suspension + 2 front knuckle | JLC bare PCB; loose components bought separately; **in-house assembly** | Magnet/shaft alignment, local support circuit, locking connector, SPI/individual CS, installed-harness noise |
| [Custom ARC ISM330DHCXTR PCB](imu/README.md) | **2**: body/CG + front axle | ARC designs one reusable board; JLC Standard PCBA remains the cost assumption | Pinout/rails, separate CS and DRDY, mounts, timestamps, bias/thermal/vibration and slip qualification |
| [Custom ARC height/scale ToF PCB](tof/README.md) | **5**: 4 ride-height + 1 ground scale | ARC designs one reusable board; VL53L4CD remains preferred/TBC; JLC Standard PCBA costed with required X-ray for this recommendation | Optical aperture/recess, window crosstalk, mux-channel/reset/pull-up design |

The [wiring plan and diagram](../../systems/bom/motherboard-wiring.md) retain **17 sensor runs / 111 minimum conductors TBC**. A common PCB design is reused at different locations; it does not combine their signals or remove dedicated chip selects. These boards add **no corner MCU or CAN node** (ADR-0006 / ADR-0008). Motor Hall and built-in temperature go to each A50S (ADR-0046).

The dedicated [IMU](imu/README.md) and [height/scale](tof/README.md) folders are the workspaces for schematic capture and layout. In-house assembly is selected for AS5047P only; it has not been selected for the IMU/ToF boards.

## AS5047P in-house purchase and assembly plan

Per car, use ten small separate angle boards. The **two-PCB-set cost scenario** uses **20 bare PCBs / 20 populated boards**, plus **22 loose AS5047P chips**: 20 fitted + two rework spares. Final bare-board order quantity/panelization and any mechanical variants remain TBC. The sensors are not mounted on the motherboard.

| Buy / provide | Quantity basis | Source / budget basis | Design status |
| --- | --- | --- | --- |
| AS5047P-ATSM, TSSOP-14 | 1 per board; 22 chips in the batch scenario | [LCSC C962063](https://www.lcsc.com/product-detail/C962063.html), **USD $4.231 each at 10+**; chip purchase total USD $93.08 / **CAD $132.84** at the funding FX rate | Selected chip; stock must be rechecked before order |
| Bare sensor PCB | 20 in the batch scenario | JLCPCB bare-board fabrication; **USD $12–25 batch allowance**, outline/panel TBC | Proposed 2-layer common design; ten measurement locations per car |
| Local supply decoupling | Manufacturer 3.3 V example uses **100 nF per IC**; stock/rework extras TBD | Loose capacitors from LCSC, DigiKey Canada or Mouser Canada | Exact MPN, package, rating and any extra bulk capacitance completed in schematic |
| Locking PCB connector | 1 per populated board; mating harness contacts counted separately | Same distributors; within support-parts allowance | 7-conductor minimum cable TBC: supply, GND, SCK, MOSI, MISO, dedicated CS, spare. Contact count may grow with returns/protection |
| Reviewed straps, damping and protection parts | BOM quantities TBD | Same loose-component order; **USD $30–80 batch support-parts allowance** includes caps/connectors/support | Values/MPNs need signal-integrity and grounding review; do not order invented resistor/TVS values |
| Solder/flux, cleaning and optional stencil | Shared assembly supplies | **USD $20–40 batch allowance** | In-house consumables; no JLC angle-board assembly/setup fee or paid labour included |
| Diametric magnet and mounting hardware | 1 magnet per installed angle sensor | Supplier, geometry and quote TBD | Required to operate; **excluded from this PCB funding total** pending mechanical selection |

Pricing inspected 2026-10-12: LCSC listed 134 chips and USD $4.773 at 1+ / $4.231 at 10+; availability is a snapshot. [Mouser Canada](https://www.mouser.ca/ProductDetail/ams-OSRAM/AS5047P-ATSM?qs=cGEy3R83DS8fzNOVLHtOgg%3D%3D) is a backup quote source: its displayed CAD $11.25 at 1+ is higher and its returned listing showed stock 0. Requote if changing source. JLC's C962063 entry is a PCBA-library reference and **cannot supply loose chips for our in-house assembly**.

Follow the [manufacturer's AS5047P datasheet, v2-00, Figure 10](https://www.mouser.com/datasheet/2/588/asset_pdf_25493104-3419339.pdf): in 3.3 V mode tie VDD/VDD3V3 as specified and place local decoupling close to the IC; connect TEST to ground. The 5 V/LDO example differs. Review MCU-reset input states and installed cable protection; the manufacturer's minimum circuit is not a qualified remote harness design.

Assembly checks: inspect solder joints and shorts, confirm supply/current, read SPI diagnostics and absolute angle with a correctly aligned magnet, label location, and verify sustained ≥2 kHz reads in the actual loom. Keep signal returns separate from motor-current paths. ESD/MSL handling, magnet geometry and spare/contact policy must be in the final assembly record.

Funding and cost detail: [CAD estimate for KEFC](../../systems/bom/jlc-pcba-cost-estimate.md). Grounding remains subject to ADR-0042; all designs pass the [fab gate](../../systems/reviews/fab-gate.md) before ordering.
