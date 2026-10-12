# ARC remote ground-height sensor PCB

- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-12

**Packaging selected under ADR-0051:** design our own remote ground-height sensor PCBs, separate from the motherboard. Use one populated board at each measurement location: **four corner ride-height sites and one beside the downward ground-speed camera for scale**, five per car under E-26. A common PCB design is the current proposal; no schematic, layout or fabrication package is released.

| Item | Current plan / status |
| --- | --- |
| Sensing IC | **VL53L4CDV0DH/1 remains the preferred candidate and costing baseline**. The custom-board decision does not newly select or qualify the final ToF part |
| Installed quantity | **5 boards per car**: front left, front right, rear left, rear right and ground-speed scale |
| Acquisition | Proposed **3.3 V sensor domain and I²C**, one remote sensor per motherboard TCA9548 mux channel; acquisition remains centralized at the H723, without a local MCU or CAN node |
| PCB stackup / outline | Common **2-layer design is TBC**; exact stackup, outline, mounts, optical aperture and mechanical variants remain TBC/TBD |
| Connector / harness | **5-conductor minimum per board, TBC**: supply, GND, SCL, SDA and spare; final connector contacts, extra returns, pinout and protection remain TBD |
| Startup / reset | Per-channel pull-up placement and remote XSHUT/reset handling need review; extra reset or interrupt conductors are not included in the minimum harness budget |
| Assembly cost assumption | JLC **Standard PCBA** with reviewed rails/panelization; the preferred IC's catalog requires X-ray inspection. Two-set funding scenario allows **10 fabricated / 10 populated boards**. In-house assembly was selected for AS5047P boards only, not for these ToF boards |

Before release, confirm the sensor choice and capture its local support circuit, land pattern, address/reset behavior and connector assignment. Qualify sensor-to-floor clearance through full bump and droop, glossy tile response, optical aperture/recess and any cover-window crosstalk. Review the real I²C harness, mux reset/startup sequence and fault recovery; a minimum-range specification alone does not prove measurement accuracy.

See the [sensor-board overview](../README.md), [parts selection](../../../systems/bom/pcb-parts-selection.md), [wiring schedule](../../../systems/bom/motherboard-wiring.md), [CAD funding estimate](../../../systems/bom/jlc-pcba-cost-estimate.md) and [fab gate](../../../systems/reviews/fab-gate.md). Grounding and signal returns remain subject to ADR-0042; all circuit and assembly details need review before ordering.
