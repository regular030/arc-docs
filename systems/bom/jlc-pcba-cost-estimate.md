# PCB funding estimate for KEFC — CAD

- **Status:** Draft (TBC planning estimate)
- **Updated:** 2026-10-12
- **Currency:** CAD funding figures; USD supplier evidence
- **Traces to:** SYS-02, SYS-07, SYS-08, SYS-14, SYS-19, SYS-24, E-02, E-20, E-21, E-22, E-23, E-24, E-26, E-30, E-41, E-43, E-46, ADR-0042, ADR-0043, ADR-0046, ADR-0047, ADR-0034, ADR-0045.

The scope is **one car: one installed Jetson carrier, one installed combined power/sync motherboard, ten angle boards, two IMU boards and four corner-height ToF boards** under ADR-0045. **All sixteen sensor boards are assembled in house** from bare PCBs and separately bought loose components. JLC factory assembly is retained for the carrier and motherboard. No populated spare sensor set or spare sensor ICs are funded. The owner requested **30% leeway** and CAD figures for KEFC.

**KEFC working PCB estimate: CAD $3,000 before tax, including 30% contingency.** The working midpoint is **CAD $2,252.94 base + $675.88 contingency = $2,928.82**, rounded **up** to the next CAD $100. It includes the core designs' two-unit factory minimum costing scenarios, the optional separate clamp, loose sensor chips/support, one shared in-house consumables allowance and both shipping allowances. Complete-car equipment, magnets/mechanics and unresolved independent-brake hardware remain excluded.

The planning range is **CAD $1,816.53–$2,689.35 base**, or **CAD $2,361.49–$3,496.15 with 30%**. The $3,000 figure is a working allowance, not a maximum or a complete supplier quote. Retaining minimum-run core costs leaves one surplus populated carrier and one surplus populated motherboard; those are supplier-minimum surplus, not a second installed car. Final Gerber/BOM/CPL and loose-component carts must be reconciled before ordering.

## Exchange rate and calculation

Use **USD 1 = CAD 1.4271**, the [Bank of Canada daily rate for 2026-10-09](https://www.bankofcanada.ca/rates/exchange/daily-exchange-rates/), latest published rate available when checked on 2026-10-12. This is an indicative budgeting conversion; actual card/distributor fees and payment-date rates can differ. Refresh before submission/payment.

`Working USD base = (low USD base + high USD base) ÷ 2`; `CAD base = USD base × 1.4271`; `CAD with contingency = CAD base × 1.30`. Sum unrounded source amounts, apply the 30% once, then round to cents. Round the buffered midpoint up to the next CAD $100 for the funding headline. Source model: **USD $1,272.8836–1,884.4836 base**, with **USD $1,578.6836** as the working midpoint. Supplier evidence stays in USD below; funding figures are CAD.

## Scope and quantities

- **Core boards:** one carrier and one combined power/sync motherboard installed. The retained JLC costing scenarios fabricate five bare boards and populate a minimum two boards for each design. One populated board per design is installed and one is minimum-run surplus; three further bare boards remain unused. These are not two complete installed PCB sets.
- **AS5047P boards:** ten installed separate angle boards: four wheel, four suspension and two front knuckle. All are assembled **in house**. Fabricate ten bare boards and buy **ten loose AS5047P-ATSM chips**, without spare chips. One common 2-layer design remains a TBC fabrication assumption; mechanical variants and exact support BOM remain open.
- **IMU boards:** two installed [custom ARC IMU PCBs](../../hardware/sensor-boards/imu/README.md), body/CG and front, both using **ISM330DHCXTR**. Fabricate five bare boards, buy two loose chips and populate two in house. Three bare boards remain unused; no bought breakout or JLC sensor assembly is charged.
- **ToF boards:** four installed [custom ARC corner-height PCBs](../../hardware/sensor-boards/tof/README.md), one at each corner. Fabricate five bare boards, buy four loose chips and populate four in house. One bare board remains unused. **VL53L4CDV0DH/1 remains preferred/TBC**, with installed optical qualification pending. The fifth dedicated ground-camera scale board is removed under ADR-0045; deriving camera height from the four corners remains TBD.
- **Remote sensors:** three reusable design families, **16 installed and in-house-populated sensor boards**, with **20 bare sensor PCBs** fabricated. No sensor factory setup, placement, fixtures or X-ray fees apply to this model. In-house assembly instructions, inspection and electrical/optical qualification remain required.
- **Optional separate clamp:** one installed if needed; retained minimum two JLC-populated and five bare total costing scenario. Omit this row and reprice motherboard population if integrated. The clamp is separate from the E-34 independent brake.
- Across the optional-clamp scenario: **19 installed boards**, **35 bare PCBs fabricated**, **6 factory-populated core/clamp boards** and **16 in-house-populated sensors**. The **22 populated boards** include three factory-minimum surplus units. Drive telemetry remains four UARTs (ADR-0034); steering's existing MCP2518FD/transceiver baseline remains included.

## KEFC cost table

| Assembly / allowance | Installed / bare / JLC populated / in-house populated | Base range, CAD | Working base, CAD | Working with 30%, CAD |
| --- | --- | --- | --- | --- |
| Jetson carrier | 1 / 5 / 2 / 0 | $638.84–$881.45 | $760.14 | $988.19 |
| Combined power/sync motherboard | 1 / 5 / 2 / 0 | $773.27–$1,130.05 | $951.66 | $1,237.16 |
| AS5047P angle boards | 10 / 10 / 0 / 10 | $89.49–$124.60 | $107.05 | $139.16 |
| Custom ARC ISM330DHCXTR IMU boards | 2 / 5 / 0 / 2 | $26.66–$32.36 | $29.51 | $38.36 |
| Custom ARC corner-height ToF boards | 4 / 5 / 0 / 4 | $31.39–$42.81 | $37.10 | $48.23 |
| Shared in-house sensor assembly consumables | One shared allowance | $21.41–$42.81 | $32.11 | $41.74 |
| Optional separate bus-clamp PCBs | 1 / 5 / 2 / 0 | $92.76–$164.12 | $128.44 | $166.97 |
| JLC shipping allowance | One shipment allowance | $114.17–$214.07 | $164.12 | $213.35 |
| Loose-component shipping allowance | One shipment allowance | $28.54–$57.08 | $42.81 | $55.66 |
| **Total including optional clamp and both shipping allowances** | **19 installed / 35 bare / 6 JLC / 16 in-house** | **$1,816.53–$2,689.35** | **$2,252.94** | **$2,928.82** |

**Carrier and motherboard only:** CAD $1,711.81 working base / $2,225.35 with 30%, excluding optional clamp, shipping and remote sensors. **All sixteen sensor boards, including shared consumables:** CAD $205.76 working base / $267.49 with 30%; base range CAD $168.95–$242.58. The table keeps the optional clamp and shipping separate.

The total uses unrounded source inputs. Summing individually rounded rows can differ by a few cents. The [CSV model](../budgets/jlc-pcba-cost.csv) distinguishes `installed_qty`, `fabricated_qty`, `jlc_assembled_qty` and `in_house_assembled_qty`, and keeps USD inputs, dated FX, endpoints and working midpoints. Calculated CSV amounts are snapshots; recalculate from source input columns when quantities or allowances change.

## Bare PCB, parts and assembly cost breakdown

These are **working midpoint costs in CAD, before tax and contingency**, using the same purchased quantities and raw USD inputs as the table above. Purchased parts include sensing ICs and support-component allowances. JLC assembly covers factory setup and population for the carrier, motherboard and optional clamp; sensor boards have **$0 factory assembly** because ARC assembles them in house.

| Board design / populated purchase quantity | Bare PCB fabrication, CAD | Purchased parts, CAD | JLC assembly, CAD | Design base subtotal, CAD |
| --- | --- | --- | --- | --- |
| Jetson carrier (2 factory populated) | $153.63 | $299.69 | $306.83 | $760.14 |
| Combined power/sync motherboard (2 factory populated) | $116.81 | $528.03 | $306.83 | $951.66 |
| 10 AS5047P angle boards | $18.12 | $88.92 | $0.00 | $107.05 |
| 2 custom ARC IMU boards | $7.42 | $22.09 | $0.00 | $29.51 |
| 4 custom ARC corner-height ToF boards | $7.42 | $29.68 | $0.00 | $37.10 |
| Optional separate bus-clamp PCBs (2 factory populated) | $24.97 | $42.81 | $60.65 | $128.44 |

The funding total includes the shared in-house consumables and shipping below. These are the **same costs regrouped by category**, so they are already included in the KEFC estimate.

| Cost category | Working base, CAD | Working with 30%, CAD |
| --- | --- | --- |
| Bare PCB fabrication — all 35 boards | $328.38 | $426.89 |
| Purchased electronic parts — all populated boards | $1,011.22 | $1,314.59 |
| JLC factory assembly — 6 core/clamp boards | $674.30 | $876.60 |
| Shared in-house sensor consumables — charged once | $32.11 | $41.74 |
| JLC shipping allowance | $164.12 | $213.35 |
| Loose-component shipping allowance | $42.81 | $55.66 |
| **Total** | **$2,252.94** | **$2,928.82** |

The exact buffered midpoint remains **CAD $2,928.82**, rounded up to the **CAD $3,000 funding allowance**. The fabrication, parts and service amounts retain their TBC quote status; independently rounded subtotals can differ by a few cents.

## Loose sensor component purchase basis

The following [LCSC](https://www.lcsc.com/) loose-component listings were checked on **2026-10-12**, in **USD**. They permit **minimum 1, multiple 1, sales unit piece**; their full-reel packaging information does not require purchasing a reel. Quantities match fitted boards exactly; no spare sensor ICs are included.

| Selected / preferred IC | Loose fitted quantity | Correct unit-price tier, USD | Chip purchase total, USD | Chip purchase total, CAD | Stock snapshot |
| --- | --- | --- | --- | --- | --- |
| [AS5047P-ATSM, C962063](https://www.lcsc.com/product-detail/C962063.html) | 10 | $4.231 at 10+ | $42.3100 | $60.38 | 134 |
| [ISM330DHCXTR, C2655101](https://www.lcsc.com/product-detail/C2655101.html) | 2 | $5.7392 at 1+ | $11.4784 | $16.38 | 657 |
| [Preferred/TBC VL53L4CDV0DH/1, C3178291](https://www.lcsc.com/product-detail/C3178291.html) | 4 | $3.1988 at 1+ | $12.7952 | $18.26 | 11,040 |
| **All sixteen fitted sensing ICs** | **16** | | **$66.5836** | **$95.02** | Not reserved |

These are distributor prices for loose stock, not JLC PCBA-library prices. Recheck availability and price before buying. All ICs are already included in the sensor rows; **do not add them again**. Magnets, mounts and harness mating parts remain outside this PCB budget.

Sensor support allowances are **USD $10–30 for ten angle boards, $2–6 for two IMU boards and $4–12 for four ToF boards**: **USD $16–48 total**, or $1–3 per populated sensor board TBC. They cover local decoupling, board connectors and reviewed straps/pull-ups/damping/protection where required. Exact MPNs, package sizes and quantities remain TBD in schematics. The [sensor purchase/assembly plan](../../hardware/sensor-boards/README.md#as5047p-in-house-purchase-and-assembly-plan) records circuit and assembly obligations; these allowances do not select resistor or protection values.

One shared **USD $15–30** allowance covers solder/flux, cleaning and an optional stencil across all three sensor families. It is charged once, assumes existing workshop tools and volunteer labour, and does not fund new equipment. LGA/optical packages require reviewed footprints and an appropriate in-house reflow/inspection process. The separate **USD $20–40 loose-component shipping allowance** covers one consolidated chip/support order; it is TBC until a cart and shipping service are chosen.

## Fabrication evidence and design assumptions

Supplier evidence here is **USD**. Observed on JLCPCB's [public calculator](https://cart.jlcpcb.com/quote/) on **2026-10-12**, without uploading design files or saving/purchasing an order:

| Scenario | Calculator quantity / dimensions | Observed bare-board total, USD | Limits |
| --- | --- | --- | --- |
| Carrier | 5 boards, 8 layers, **130 × 70 mm one-up panel**, 1.6 mm, green, ENIG, 1 oz outer / 0.5 oz inner copper | **$107.65** | $99 engineering + $7.60 board + $1.05 production-file confirmation. Body reference 120 × 60 mm (ADR-0043); proposed 5 mm rails give the costing panel. Final rails, stackup and routing TBC |
| Motherboard | 5 boards, 6 layers, **150 × 100 mm**, 1.6 mm, green, ENIG, 2 oz outer / 0.5 oz inner copper | **$81.85** | $33 engineering + $38.10 outer-copper charge + $9.70 board + $1.05 confirmation. Layer/copper choices are costing assumptions, not qualified current/thermal ratings |
| Small sensor reference | 5 boards, 2 layers, **40 × 30 mm**, 1.6 mm, green, lead-free HASL, 1 oz, single PCB, deburring enabled, production-file confirmation disabled | **$5.20 web / CAD $7.42** | $4.00 base + $1.10 finish + $0.10 deburring. Bare fabrication only; chips, support, in-house assembly and shipping separate. Final outline/finish TBC |

**Screenshot evidence, captured 2026-10-12:** [five small bare PCBs and selected options](pricing/2026-10-12/sensor-pcb-5.jpg), [published JLC assembly fee schedule](pricing/2026-10-12/jlc-assembly-fees.jpg). The $5.20 screenshot covers **five bare PCBs**, not populated sensors. The factory assembly fee screenshot applies to **core/clamp assembly assumptions only**; all sensor boards are assembled in house.

For two five-board IMU/ToF lots, retain **USD $5.20 each** as the small-board reference. For ten angle boards, **USD $10.40–15.00 is a TBC fabrication allowance**: its lower endpoint is `2 × $5.20`, a planning proxy from the five-board evidence, **not an observed ten-board quote**. The total remote fabrication allowance is **USD $20.80–25.40**. Final quantity pricing, layer counts, finish, outlines and any mechanical variants must be quoted from CAD. Bare-board pricing does not impose JLC's Standard PCBA panel minimum on these in-house sensor boards.

No Gerbers/BOM/CPL were uploaded and no order placed; there is no final complete-PCBA cart screenshot. Prices exclude coupons, assembly, components, shipping and tax unless explicitly included elsewhere. Larger boards, HDI, different vias/impedance or a changed stackup require a revised quote. The motherboard envelope and cooler remain subject to ADR-0043.

## Core assembly and component allowance basis

The carrier, motherboard and optional clamp retain the previous **two factory-populated units per design** cost scenario, with one installed and one minimum-run surplus under ADR-0045. Component and assembly ranges are planning allowances pending final schematics/BOM/CPL; reducing the installed car count does not remove those retained supplier-minimum charges.

The [JLC assembly fee schedule](https://jlcpcb.com/help/article/pcb-assembly-price), checked on 2026-10-12, gives single-side Standard setup/stencil **USD $33.77**, feeder loading **$1.53 per distinct part type**, placement **$0.0016 per joint**, and **$16.42 for two rigid-PCB fixtures** at prototype quantities. Core rows retain their double-sided **USD $180–250 assembly allowances**; optional clamp assembly retains **$35–50 TBC**. JLC's [assembly capabilities](https://jlcpcb.com/capabilities/pcb-assembly-capabilities) list a **70 × 70 mm minimum for Standard single-board assembly**; the carrier costing panel is 130 × 70 mm. These service rules relate to factory-assembled core/clamp designs, not sensor fabrication.

The ICs' JLC Standard-only/X-ray catalog conditions no longer create sensor factory fees: we buy loose distributor chips and assemble all sixteen boards in house. Retained core component price references include STM32H723 USD $11.05, LTC6810 USD $10.73 estimated/preorder and TPS26630 USD $3.59 estimated/preorder with MOQ 3. Exact core reference-designator counts, passives, magnetics, connectors, protection, stock and attrition remain TBD/TBC in the final quote. No precision IMU or alternate selected sensor is added.

## Exclusions and quote gates

Excluded: Jetson module, SSD, Wi-Fi/antennas, cooling, cameras/lenses, LiDAR, Ethernet switch, bought A50S/moteus, motors, batteries, wiring/mating crimps, sensor magnets/mechanics, external dump resistors/heatsinks, E-34 independent brake and its unselected drive-inhibit hardware, tax/import/brokerage, paid design/assembly labour, new workshop equipment and functional validation. This is a **PCB funding request**. Unselected safety hardware needs its own funding line once chosen. The 30% does not guarantee a respin or major scope change is covered.

Before ordering: finish schematics and connector/grounding/power decisions, repair and validate the [carrier Altium conversion](../../hardware/orin-carrier/README.md), freeze stackups/panels and installed/minimum-run populations, export Gerbers/BOM/CPL, obtain actual core PCB/PCBA and loose-parts carts, confirm stock/shipping and the in-house sensor assembly process, then apply the [fab gate](../reviews/fab-gate.md). Reconcile ground-camera scale under ADR-0045. Recalculate if quantities, source, FX or scope changes. No order has been placed.
