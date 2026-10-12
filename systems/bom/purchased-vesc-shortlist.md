# Purchased VESC shortlist

> [!WARNING]
> **Deprecated:** Historical sourcing/drive-FD comparison; current A50S telemetry is UART under ADR-0049. Retained prices are snapshots. Use the [main parts list](pcb-parts-selection.md) and [UART ICD](../icd/controller-telemetry.md); FD requirements in the historical body below are withdrawn.

- **Prices checked:** 2026-10-07

Scope: E-50, four single-motor drive controllers for E-51. Prices use the supplier's currency and exclude shipping, tax, import fees and ARC integration hardware. These are assembled controllers purchased from their manufacturers, rather than JLCPCB component prices.

> [!IMPORTANT]
> **Best small candidate to evaluate: TeamTriforceUK A50S V2.3c. No fully compliant, unmodified replacement was verified.** ADR-0034 mounts it on the lower deck; the historical body size screening fits the former corner target, but CAN-FD telemetry, independent drive inhibition and the firmware-independent full-stop path still need engineering and qualification. **A50S is selected in latest main under ADR-0034 and retained by the user.** Integration / qualification remain open; FD transport and full-stop guarantees follow ADR-0041 / ADR-0047.

## Buyable candidates

Supplier dimensions below are product data, not approved ARC envelopes. Current A50S placement is on the lower deck under ADR-0034; the historical corner screening target was **30 × 40 mm TBC**, with final dimensions and height **TBD** in the [mechanical brief](../mechanical/layout-brief.md). Connector clearance, cable bends, the nearby capacitor, cooling and any added interface board count toward the installed envelope.

| Candidate / manufacturer listing | Published size, mm | Input range | Listed price | Availability / assessment |
| --- | --- | --- | --- | --- |
| 🟡 **Preferred evaluation candidate:** [A50S V2.3c](https://teamtriforceuk.com/a50s-v2/) | **35.5 × 21 × 13.8**, without connectors | **6–52 V**, 12S version | **GBP £65** base; **£70** with populated signal connector; **£100** with connector + heatsink | Live listing showed stock and an enabled purchase button. Body fits the target when rotated; complete cooled envelope **TBC**. |
| 🟡 **Integration alternative:** [A50S V2.3c Module](https://teamtriforceuk.com/a50s-v2-3c-module/) | **41.6 × 23.9 × 13.8**, without connectors | **6–35 V**, listed 8S module | **GBP £80** | Live listing showed stock. Board-to-board Samtec connectors suit an ARC interface PCB; exceeds the target's long side before that PCB is added. Layout approval **TBC**. |
| 🟡 **Larger reference controller:** [VESC Minim 100V](https://www.vesclabs.com/product/vl-minim-100v/) | **72 × 43 × 23** on product page | **12–100 V** | **EUR €159.99** each; **€149.99** each at four | Listing showed stock. Larger than the corner target; operation at the lowest pack voltage / sag **TBC**. See dimensional discrepancy below. |
| 🔴 **Poor match to this layout / pack:** [Flipsky Mini FSESC6.7 PRO](https://flipsky.net/products/mini-fsesc6-7-pro-70a) | **67 × 39 × 18.7**, including heatsink | **14–60 V** | **USD $57** | Manufacturer offers ordering, but body exceeds target and its published minimum does not cover the proposed depleted/sagging 4S pack. Manufacturer also recommends retaining factory firmware 5.2. |

**Budget for the selected controllers:** **GBP £400** for four installed units with populated signal connector and heatsink, plus **£200** for two spares under ADR-0034 (six units: £600 at the retained snapshot). This excludes the FD interface, independent safety hardware, harness changes and shipping. Canada delivery time and landed cost are **TBD**; the [supplier shipping page](https://teamtriforceuk.com/shipping-returns/) gives no destination-specific quote.

The Minim's [October 2025 datasheet](https://usercontent.one/wp/www.vesclabs.com/wp-content/uploads/2025/08/vesc_minim_datasheet.pdf?media=1760515964) lists **28 mm height** in its physical-properties table, while its drawing and current product page indicate **23 mm**. Resolve with the supplier before using its dimensions in CAD.

## A50S versus ARC's requirements

🟢 = published capability matches this part of the design; 🟡 = qualification or information needed; 🔴 = missing / not established as supplied. Green rows do **not** mean the controller satisfies the whole system.

| ARC need and trace | Status | Evidence and remaining work |
| --- | --- | --- |
| Four independent sensored motor controllers — E-50, E-51, SYS-01 | 🟢 / 🟡 | A50S supports Hall-sensored FOC and torque/current control. Exact Castle motor detection, sensor pin mapping and bidirectional loaded operation remain **TBC**. |
| 4S battery — E-40, E-50, SYS-31 | 🟢 / 🟡 | Published input range covers the pack. Test sag and regenerative transients using the [power scenarios](../budgets/power-scenarios.md); battery-current and voltage settings remain **TBC**. |
| Phase current and cooling — E-50, RSK-02 | 🟡 | Supplier rates **20 A motor continuous without cooling, 40 A with heatsink, 80 A burst at 4S**. ARC motor-phase demand is **TBD**. Total battery current divided by four does not establish phase current. Qualify heatsink airflow and temperature in the actual deck assembly. |
| Speed — SYS-01 | 🟡 | Castle specifies a [four-pole 1010-4400kV motor](https://castlecreations.com/products/sensored-motor-4-pole-sensored-brushless-1010-4400kv-motor-060-0098-00?variant=46138175291580). ARC's proposed loaded shaft speed of 37,500 rpm therefore requires 75,000 electrical rpm. Upstream A50S v2.3 firmware permits a 200,000 electrical-rpm configuration limit; that is not a tested operating guarantee. Validate loaded FOC at the required speed on the exact supplied revision. |
| Command bus — ADR-0009, SYS-24 | 🟢 / 🟡 | CAN is exposed. Confirm classic CAN at **1 Mbit/s**, ARC message/config compatibility, and termination removal on intermediate nodes. A connector adapter must preserve the command trunk. |
| Separate timestamped FD telemetry — ADR-0011, ADR-0041, SYS-24, SYS-29 | 🔴 | No separate CAN-FD telemetry interface is documented. Requires added controller/transceiver hardware and firmware, meeting **≥1 kHz**, **mandatory 1 Mbit/s arbitration / exactly 5 Mbit/s data with BRS**, timestamps and bus-load limits. SPI pin availability, a usable interrupt pin and compatibility with simultaneous Hall sensing are **TBC**. Converting a slow classic-CAN stream into FD does not recover the missing samples or timestamps. |
| Motor / FET temperature — E-28, SYS-19 | 🟢 / 🟡 | Motor-temperature input is exposed; the upstream hardware definition includes motor and MOSFET temperature ADCs. Match the NTC curve and prove both appear in ARC telemetry. |
| Hardware drive inhibition — SYS-05, E-50, ICD-corner-connector | 🔴 | Published connector pinout does not expose a dedicated gate-enable / hardware-inhibit input. A software brake input or MCU reset is not evidence that retained PWM is safely overridden. A qualified independent path remains **TBD**. |
| Full stop with halted controller firmware — SYS-05, ADR-0047, E-34 | 🔴 | No manufacturer evidence establishes this behavior. The vehicle still needs E-34 independent braking that remains effective through drive inhibition. CAN braking, a watchdog or torque removal alone does not pass. |
| All-corner fault stop — SYS-25 | 🟡 | Implement and verify command/telemetry-loss detection and the coordinated stop in the ARC sync / drive firmware. A stock standalone timeout does not establish this vehicle behavior. |
| Corner swap and harness — SYS-13, ICD-corner-connector | 🟡 | The current deck-mounted A50S / corner phase-and-Hall harness follows ICD-corner-connector Rev F; its circuit numbering is provisional. Adapter, contact ratings, termination and servicing access require review. |

The [manufacturer connector diagram](https://cdn11.bigcommerce.com/s-4t55i5fv4j/images/stencil/original/image-manager/pinout-v2.3.png?t=1684596909) exposes several multiplexed functions; do not allocate SPI and safety pins from feature names alone. The module listing offers factory removal of its CAN termination resistor, which is useful for intermediate bus nodes. Confirm the standalone unit's termination arrangement separately.

The old screening observations used the [upstream A50S v2.3 hardware definition](https://github.com/vedderb/bldc/blob/master/hwconf/teamtriforceuk/a50s_v23/hw_a50s_v23_core.h). Latest main selects the **`a50s_v23c_12s`** target under ADR-0034; the matching vendored hardware definition is in `firmware/vesc/bldc/hwconf/teamtriforceuk/a50s_v23c/`. Confirm that the shipped hardware matches this target and audit its SPI / IRQ mux before flashing a spare. Do not transplant the historical custom-controller settings.

## CAN-FD integration — hard 5 Mbit/s requirement

The [drive FD implementation](../icd/drive-canfd-telemetry.md) gives the proposed circuit and firmware sequence: **A50S MCU → local SPI / CS / IRQ → MCP2518FD + 40 MHz crystal → separate FD transceiver → front or rear FD bus → H723 native FDCAN → Orin**. Keep the existing classic 1 Mbit/s command trunk separate. SPI / IRQ access and coexistence with Hall sensing on the exact shipped revision remain TBC; no connector pin assignment is approved.

ADR-0041 makes 5 Mbit/s data with BRS mandatory. Qualify ≥1 kHz full timestamped snapshots per controller, ≤70% bus load, ≤2 ms p99 sample-to-Orin driver latency and motor-loop timing using the actual interfaces / harness. UART, slower FD and reduced telemetry do not pass. This does not resolve the separate independent-brake and drive-inhibition gaps.

## Designer handoff

1. **Obtain the exact revision's schematic, firmware target and installed CAD envelope.** Resolve cooling size, available SPI / IRQ pins, independent drive-inhibit access and termination before freezing the bought-controller FD and safety integration.
2. **Evaluate one unit on the motor bench before specifying four for the car.** Check Castle Hall wiring, phase-current demand, full-speed FOC, motor/FET temperature, regen and cooling. Buying or passing a motor-spin test does not complete the safety qualification.
3. **Design and qualify the missing ARC functions.** FD telemetry requires genuine sample timestamps and scheduling that preserves motor control. E-34 must demonstrate chassis standstill under the controller-fault cases in ADR-0047 and the [verification plan](../verification/plan.md).
4. **Review the FD integration proposal.** ADR-0034 already selects the A50S. Review the proposed FD implementation under ADR-0041 and update its exact PCB / pin map after owner selection; retain main’s deck placement, 9 m/s cap and firmware target. Passing the [fab gate](../reviews/fab-gate.md) remains required under the team's spending rule.

**Recommendation: evaluate the A50S V2.3c as a purchased motor power stage. A complete purchased-only solution meeting every current requirement remains unverified.**

## Other small boards screened

| Board | Reason it did not improve the shortlist |
| --- | --- |
| [Holybro Mini FOC ESC](https://holybro.com/products/holybro-mini-foc-esc-based-on-vesc%C2%AE6) | Published **65 × 18 mm** board; manufacturer listing marked sold out. No CAN interface is identified in its published port list. |
| [Maytech MTSPF6.0M](https://maytech.cn/products/copy-of-maytech-new-72v-vesc-controller-mtsvesc7-5r-round-vesc-based-on-vesc6-0-hardware-for-electric-skateboard-longboard) | Published **79 × 18.5 × 7.5 mm**; no CAN port identified in the pin list. Long shape and unresolved command interface offer no advantage for this corner. |
| [MakerX Go-FOC M100](https://www.makerx-tech.com/products/go-foc-m100) | Manufacturer listing marked sold out; current MakerX catalog did not provide a buyable Mini-FOC replacement. |

Return to the [PCB parts and board map](pcb-parts-selection.md).
