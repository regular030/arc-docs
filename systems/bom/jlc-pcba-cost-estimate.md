# PCB funding estimate for KEFC — CAD

- **Status:** Draft (TBC planning estimate)
- **Updated:** 2026-10-12
- **Currency:** CAD funding figures; USD supplier evidence
- **Traces to:** SYS-02, SYS-07, SYS-08, SYS-14, SYS-19, SYS-24, E-02, E-20, E-21, E-22, E-23, E-24, E-26, E-30, E-41, E-43, E-46, ADR-0044, ADR-0045, ADR-0046, ADR-0047, ADR-0049, ADR-0050, ADR-0051.
- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-12

The scope is **one car's PCB set plus one spare PCB set**, with JLC fabrication/assembly and in-house AS5047P assembly. It does not fund two complete cars or authorize an order. The owner requested **30% leeway** and Canadian-dollar figures for KEFC.

**KEFC working PCB estimate: CAD $3,600 before tax, including 30% contingency.** This is the rounded midpoint for **one car's PCB set plus one spare PCB set**. The midpoint is **CAD $2,770.12 base + $831.03 contingency = $3,601.15**, rounded to the nearest CAD $100. It includes the optional separate clamp PCB, loose sensor chips/support parts, in-house consumables and both shipping allowances. Complete-car equipment, magnets/mechanics and unresolved independent-brake hardware remain excluded.

The revised planning range is **CAD $2,224.96–3,315.27 base**, or **CAD $2,892.45–4,309.85 with 30%**. The $3,600 working figure is a midpoint allowance, not a maximum or a supplier quote. Actual Gerber/BOM/CPL quotes must be reconciled before ordering.

## Exchange rate and calculation

Use **USD 1 = CAD 1.4271**, the [Bank of Canada daily rate for 2026-10-09](https://www.bankofcanada.ca/rates/exchange/daily-exchange-rates/), latest published rate available when checked on 2026-10-12. This is an indicative budgeting conversion; actual card/distributor conversion fees and payment-date rates can differ. Refresh before submission/payment if the rate changes.

`Working USD base = (low USD base + high USD base) ÷ 2`; `CAD base = USD base × 1.4271`; `CAD with contingency = CAD base × 1.30`. Sum unrounded source amounts, apply the 30% once, then round to cents. The headline rounds the midpoint with contingency to the nearest CAD $100. Source model: **USD $1,559.08–2,323.08 base**, with **USD $1,941.08** as the working midpoint. Supplier evidence stays in USD below; funding figures are CAD.

## Scope and quantities

- Two Jetson carriers and two combined power/sync motherboards assembled by JLC, with five bare boards fabricated for each core design. Spare bare boards are not populated sets. Power and sync are one PCB per set.
- **AS5047P boards:** 10 separate angle boards per set, 20 across the batch, assembled **in house**. JLC fabricates bare boards only. Buy **22 loose AS5047P-ATSM chips** separately: 20 fitted plus two rework spares. One common 2-layer layout is the TBC cost assumption; mechanical variants and exact support BOM remain open.
- **IMU boards:** both body/CG and front use **ISM330DHCXTR** under ADR-0050 on [our own ARC IMU PCB](../../hardware/sensor-boards/imu/README.md) under ADR-0051. One shared custom design, five bare boards and four JLC-populated boards for two sets. ICs, local support, fabrication and assembly are already included; no bought breakout module or SCH16T is purchased.
- **ToF boards:** [our own ARC height/scale PCB](../../hardware/sensor-boards/tof/README.md) under ADR-0051, five per set and ten bare/populated across the batch. One reusable custom design; VL53L4CD remains the preferred/TBC costing IC. ICs/support/fabrication/assembly are already included. IMU and ToF remain costed as JLC Standard PCBA; the preferred ToF catalog requires X-ray inspection. In-house assembly has only been selected for AS5047P.
- **Remote sensors:** three reusable design families and 17 populated measurement-site boards per set; **34 populated boards total**, including 20 assembled in house. Layer count, outlines and panelization remain TBC, not approved CAD.
- Optional separate clamp: five bare PCBs and two JLC-populated. Omit the row and reprice motherboard population if integrated. The clamp is separate from the E-34 independent brake.
- Drive telemetry remains four UARTs (ADR-0049). Steering's existing MCP2518FD/transceiver baseline remains included; no A50S-local FD adapters are priced.

The model prices custom IMU/height PCBs rather than bought modules. This revision uses narrower, fee-based Standard assembly allowances for those two sensor designs and presents the midpoint as the working estimate. Core component, fabrication and shipping ranges are retained pending the final BOM and panel quotes.

## KEFC cost table

| Assembly / allowance | Bare / JLC populated / in-house populated | Base range, CAD | Working base, CAD | Working with 30%, CAD |
| --- | --- | --- | --- | --- |
| Jetson carrier | 5 / 2 / 0 | $638.84–$881.45 | $760.14 | $988.19 |
| Combined power/sync motherboard | 5 / 2 / 0 | $773.27–$1,130.05 | $951.66 | $1,237.16 |
| Angle sensor carriers | 20 / 0 / 20 | $221.31–$339.76 | $280.54 | $364.70 |
| Custom ARC ISM330DHCXTR IMU PCBs | 5 / 4 / 0 | $150.99–$226.62 | $188.81 | $245.45 |
| Custom ARC height/scale ToF PCBs | 10 / 10 / 0 | $205.07–$302.12 | $253.60 | $329.67 |
| Optional separate bus-clamp PCBs | 5 / 2 / 0 | $92.76–$164.12 | $128.44 | $166.97 |
| JLC shipping allowance | Shipment allowance | $114.17–$214.07 | $164.12 | $213.35 |
| Loose-component shipping allowance | Shipment allowance | $28.54–$57.08 | $42.81 | $55.66 |
| **Total including optional clamp and both shipping allowances** | **50 bare / 20 JLC / 20 in-house** | **$2,224.96–$3,315.27** | **$2,770.12** | **$3,601.15** |

**Core boards only:** CAD $1,711.81 working base / $2,225.35 with 30%, excluding shipping and remote sensors. **Remote sensor boards:** CAD $722.94 working base / $939.82 with 30%. Their base ranges are CAD $1,412.12–$2,011.50 and CAD $577.38–$868.50, respectively.

The total is calculated from unrounded source amounts. Summing separately rounded rows can differ by a few cents. Allocated per PCB set: **CAD $1,800.57 working / $1,446.23–2,154.92 range with 30%**. A one-set order retains setup/minimum charges and must be requoted. The [CSV model](../budgets/jlc-pcba-cost.csv) keeps USD inputs, CAD endpoints, dated FX, separate factory/in-house quantities and explicitly labelled `working` midpoint columns. CSV calculated amounts are snapshots; recalculate from the input columns when changing allowances.

## AS5047P loose-parts purchase basis

[LCSC C962063](https://www.lcsc.com/product-detail/C962063.html) was inspected on 2026-10-12: **USD $4.231 each at 10+**, with 134 listed in stock. The planned 22-chip order costs **USD $93.08 / CAD $132.84** after rounding. This uses the 10+ tier; it does not assume the cheaper 30+ tier. Availability is not reserved.

Angle-row breakdown in USD: **$93.08 chips + $30–80 support parts + $12–25 bare PCB fabrication + $20–40 in-house consumables/optional stencil**. No JLC angle-board assembly fee is charged; volunteer labour and new workshop equipment are not priced. The loose-parts shipping row covers a separate distributor shipment, avoiding a false assumption that JLC's PCBA parts can be collected as loose stock. [Purchase/support checklist](../../hardware/sensor-boards/README.md#as5047p-in-house-purchase-and-assembly-plan) lists decoupling, connector, protection and magnet obligations; exact support MPNs remain TBD.

The selected installed state-sensor ICs alone cost **USD $69.56 / CAD $99.27 per car**: `2 × $5.70 ISM330DHCXTR + 10 × $4.231 AS5047P + 5 × $3.17 ToF`. Two sets use USD $139.12 before the two loose spare angle chips, support parts, PCB and assembly. These chips are already included in the sensor rows; do not add them twice.

## Fabrication evidence and design assumptions

Supplier evidence in this section is **USD**. Observed on JLCPCB's [public calculator](https://cart.jlcpcb.com/quote/) on 2026-10-12, without uploading design files or saving/purchasing an order:

| Scenario | Calculator quantity / dimensions | Observed bare-board total, USD | Limits |
| --- | --- | --- | --- |
| Carrier | 5 boards, 8 layers, **130 × 70 mm one-up panel**, 1.6 mm, green, ENIG, 1 oz outer / 0.5 oz inner copper | **$107.65** | $99 engineering + $7.60 board + $1.05 production-file confirmation. The body reference is 120 × 60 mm (ADR-0045); proposed 5 mm rails on all four edges give the costing panel. Rails and final stackup/routing remain TBC |
| Motherboard | 5 boards, 6 layers, **150 × 100 mm**, 1.6 mm, green, ENIG, 2 oz outer / 0.5 oz inner copper | **$81.85** | $33 engineering + $38.10 outer-copper charge + $9.70 board + $1.05 confirmation. Six layers and copper weights are cost assumptions, not a selected stackup or demonstrated current/thermal rating |
| Small sensor reference | 5 boards, 2 layers, **40 × 30 mm**, 1.6 mm, green, lead-free HASL, 1 oz, single PCB, deburring enabled, production-file confirmation disabled | **$5.20 web / CAD $7.42** | $4.00 base + $1.10 finish + $0.10 deburring. Bare fabrication only; components, assembly and shipping are separate. This costing scenario does not select the final sensor finish, outline or assembly panel |

These exclude assembly, components, shipping and tax. Calculator prices can change and are not binding on the eventual Gerbers. No coupon or JLCONE desktop discount is counted. The motherboard growth case and cooler remain subject to ADR-0045; a larger board, HDI, finer vias or a different controlled-impedance stackup must be requoted.

**Screenshot evidence, captured 2026-10-12:** [five small bare PCBs and selected options](pricing/2026-10-12/sensor-pcb-5.jpg), [published JLC assembly fee schedule](pricing/2026-10-12/jlc-assembly-fees.jpg). The $5.20 screenshot covers five bare boards, not populated sensors or the whole batch. The sensor fabrication ranges also allow for batch quantities and Standard assembly rails/panels. No Gerbers/BOM/CPL were uploaded and no order was placed; there is no complete-PCBA cart screenshot yet.

JLC's [assembly capabilities](https://jlcpcb.com/capabilities/pcb-assembly-capabilities) list a **70 × 70 mm minimum for Standard single-board assembly**. The carrier costing panel is 130 × 70 mm. Both [ISM330DHCXTR](https://jlcpcb.com/partdetail/Stmicroelectronics-ISM330DHCXTR/C2655101) and [VL53L4CD](https://jlcpcb.com/partdetail/STMicroelectronics-VL53L4CDV0DH1/C3178291) are catalogued **Standard Only**, with required X-ray listed for the ToF. The earlier Economic sensor-service assumption is replaced. IMU/ToF allowances therefore include Standard service and a reviewed ≥70 × 70 mm rail/panel solution; actual array/rails and resulting bare-board quantities must be quoted from CAD. Angle boards are bare-board fabrication for in-house assembly and do not incur JLC PCBA setup.

## Assembly and component allowance basis

The [JLC assembly fee schedule](https://jlcpcb.com/help/article/pcb-assembly-price), rechecked on 2026-10-12, gives single-side Standard setup/stencil **USD $33.77**, feeder loading **$1.53 per distinct part type**, placement **$0.0016 per joint**, and **$16.42 for two rigid-PCB fixtures** at prototype quantities. Core rows retain their double-sided Standard allowances. The revised sensor assembly allowances are **USD $65–90 for four IMU boards** and **USD $85–110 for ten ToF boards**; fabrication and components are separate.

The sensor allowances assume **6–10 distinct BOM types TBC per design**, single-side population, placement/packing/confirmation and required fixtures. Ten ToF inspections add **USD $16.40**; four IMU inspections add **USD $6.56 provisionally**, subject to JLC's treatment of the LGA package. Optional storage, extra manual work and actual panel handling depend on the quote. The **$14.93 handling fee** is a Standard-versus-Economic price-difference top-up when both services are available, not an automatic charge on every job. These are fee-based planning allowances; final BOM/CPL counts remain TBD.

Retained JLC unit-price basis from earlier 2026-10-12 inspection: **ISM330DHCXTR USD $5.70** (exact 1+ $5.7037 / 10+ $4.9154), **VL53L4CD USD $3.17**, STM32H723 USD $11.05, LTC6810 USD $10.73 estimated/preorder and TPS26630 USD $3.59 estimated/preorder with MOQ 3. No SCH16T or alternative IMU is added. Final stock, MOQ/attrition, part counts, populated sides and process must be checked against the exported BOM. Carrier/motherboard component figures include presently unenumerated passives, magnetics, connectors and protection; exact reference-designator totals remain TBD.

## Exclusions and quote gates

Excluded: Jetson module, SSD, Wi-Fi/antennas, cooling, cameras/lenses, LiDAR, Ethernet switch, bought A50S/moteus, motors, wiring/mating crimps, sensor magnets/mechanics, external dump resistors/heatsinks, E-34 independent brake and its unselected drive-inhibit hardware, tax/import/brokerage, paid design/assembly labour, new workshop equipment and functional validation. This is a **PCB funding request**, not a complete build budget. Unpriced safety hardware needs a separate funding line once selected. The 30% does not guarantee a respin or major scope change is covered.

Before ordering: finish schematics and connector/grounding/power decisions, repair and validate the [carrier Altium conversion](../../hardware/orin-carrier/README.md), freeze stackups/rails/panels and populations, export Gerbers/BOM/CPL, get actual PCB/PCBA and loose-parts carts, confirm shipping/stock and apply the [fab gate](../reviews/fab-gate.md). Recalculate if quantities, source, FX or final scope changes. No order has been placed.
