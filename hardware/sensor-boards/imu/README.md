# ARC remote IMU PCB

- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-12

**Packaging selected under ADR-0051:** design our own remote IMU PCB, separate from the motherboard. Both measurement sites use **ST ISM330DHCXTR** under ADR-0050: one at the body/CG and one at the front axle. Reuse a common PCB design at the two locations; each populated board has its own cable, chip select and data-ready input. No schematic, layout or fabrication package is released.

| Item | Current plan / status |
| --- | --- |
| Installed quantity | **2 boards per car**: body/CG and front axle; E-20 and E-21 |
| Acquisition | Proposed **3.3 V sensor domain, SPI and DRDY** wired to the H723 on the motherboard; acquisition remains centralized, without a local MCU or CAN node |
| Timing | Retain ≥4 kHz output and hardware-timestamped data ready; configure and verify the selected part's bandwidth, filtering, FIFO and sample timing against SYS-06, SYS-07 and SYS-10 |
| PCB stackup / outline | Common **2-layer design is TBC**; exact stackup, outline, mounting and mechanical variants remain TBC/TBD |
| Connector / harness | **8-conductor minimum per board, TBC**: supply, GND, SCK, MOSI, MISO, CS, DRDY and spare; final connector contacts, extra returns, protection and pinout remain TBD |
| Assembly cost assumption | JLC **Standard PCBA**, with reviewed assembly rails/panelization; two-set funding scenario allows **5 fabricated / 4 populated boards**. In-house assembly was selected for AS5047P boards only, not for these IMU boards |

Before release, capture the exact supply/decoupling and interface circuit from the manufacturer guidance; review the land pattern, protection, mounting and connector assignment. Qualify power-up, installed SPI/DRDY timing, sustained acquisition, calibration, thermal drift, motor-on vibration and velocity-through-slip performance. A selected chip or board architecture does not establish achieved navigation performance.

See the [sensor-board overview](../README.md), [parts selection](../../../systems/bom/pcb-parts-selection.md), [wiring schedule](../../../systems/bom/motherboard-wiring.md), [sensor review](../../../systems/bom/sensor-candidate-review.md), [CAD funding estimate](../../../systems/bom/jlc-pcba-cost-estimate.md) and [fab gate](../../../systems/reviews/fab-gate.md). Grounding and signal returns remain subject to ADR-0042; all circuit and assembly details need review before ordering.
