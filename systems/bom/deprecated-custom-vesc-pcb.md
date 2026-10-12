# Deprecated: custom VESC PCB parts reference

> [!WARNING]
> **Deprecated:** ARC buys A50S V2.3c controllers under ADR-0034. The custom VESC power-stage parts and old corner connectors below are historical references with no installed ARC quantity. They are excluded from the active [parts list](pcb-parts-selection.md) and must not be included in an orderable BOM.

The former custom-board option used the [official VESC 6 schematic](https://vesc-project.com/sites/default/files/Benjamin%20Posts/VESC_6.pdf). This preserves the earlier 2026-10-03 pricing/design snapshot; it does not approve those parts, ratings, footprints or firmware settings. The later local A50S FD proposal is also deprecated under ADR-0049; the active parts list uses UART. Independent full-stop braking remains required under ADR-0047.

## Deprecated custom power-stage options

| Function / choice | Exact manufacturer part | JLCPCB | Qty / VESC → car | USD each | Package / design notes |
| --- | --- | --- | --- | --- | --- |
| MCU — ⚪ **Historical option** | ST **STM32F405RGT6** | [C15742](https://jlcpcb.com/partdetail/Stmicroelectronics-STM32F405RGT6/C15742) | **0 installed; historical quantity only** | **$5.95** | LQFP-64, 10 × 10 mm; keep the frozen VESC pin map, clocks and flash layout. |
| Three-phase gate driver — ⚪ **Historical option** | TI **DRV8301DCAR** | [C98969](https://jlcpcb.com/partdetail/TexasInstruments-DRV8301DCAR/C98969) | **0 installed; historical quantity only** | **$3.37** | HTSSOP-56 EP; preserve gate-drive, buck, protection and decoupling design. |
| Local 3.3 V buck — ⚪ **Historical option** | TI **LM3671MF-3.3/NOPB** | [C201586](https://jlcpcb.com/partdetail/TexasInstruments-LM3671MF_3_3NOPB/C201586) | **0 installed; historical quantity only** | **$2.14** | SOT-23-5; feed from the local 5 V supply, **not raw 4S**; max operating input is 5.5 V. |
| Phase-current amplifiers — ⚪ **Historical option** | Analog Devices **AD8418AWBRZ-RL** | [C462197](https://jlcpcb.com/partdetail/AnalogDevices-AD8418AWBRZRL/C462197) | **0 installed; historical quantity only** | **$2.38**; $2.07 at 10+ | SOIC-8, gain 20; successor-family candidate for the reference AD8418. Confirm pinout, reference / output swing, bandwidth and ADC scaling before use. |
| Phase shunts — ⚪ **Historical option** | Milliohm **HoYLR2512E-3W-0.5mR-1%** | [C5375456](https://jlcpcb.com/partdetail/Milliohm-HoYLR2512E_3W_0_5mR_1/C5375456) | **0 installed; historical quantity only** | **$0.0610** | 2512, 0.5 mΩ, 3 W; Kelvin connections. Verify phase-current peak, pulse rating and thermal derating. |
| Original power MOSFET — ⚪ **Historical option** | Infineon **IRF7749L1TRPBF** | No verified JLC quote | **0 installed; historical quantity only** | **~$7.50** external 1+; ~$5.06 at 10+ | DirectFET Large; [DigiKey reference price](https://www.digikey.com/en/products/detail/infineon-technologies/IRF7749L1TRPBF/3990838). External sourcing / consignment required if retaining this footprint. |
| JLC power MOSFET — ⚪ **Historical option** | Infineon **BSC010N04LS** | [C501508](https://jlcpcb.com/partdetail/InfineonTechnologies-BSC010N04LS/C501508) | **0 installed; historical quantity only** | **$0.87**; $0.70 at 10+ | 40 V TDSON-8 FL, Standard PCBA. Requires a new power-stage footprint / copper / cooling design and checks of gate charge, losses, voltage margin and SOA. Not a DirectFET replacement on existing copper. |
| Classic command transceiver — ⚪ **Historical option** | NXP **TJA1051T/3/CM,118** | [C2671195](https://jlcpcb.com/partdetail/NXP-TJA1051T_3CM118/C2671195) | **0 installed; historical quantity only** | **$0.82** | SOIC-8; built-in MCU CAN for shared command bus. |
| Hardware drive-inhibit delay — ⚪ **Historical option** | TI **TLC555IDR** | [C6987](https://jlcpcb.com/partdetail/TexasInstruments-TLC555IDR/C6987) | **0 installed; historical quantity only** | **$0.3749** | SOIC-8; Current power-board cut timing and E-34 coordination are TBC (ADR-0034 / ADR-0047). This timer does not brake the car or guarantee standstill. |
| Gate-enable logic — ⚪ **Historical option** | TI **SN74LVC1G08DBVR** | [C7666](https://jlcpcb.com/partdetail/TexasInstruments-SN74LVC1G08DBVR/C7666) | **0 installed; historical quantity only** | **$0.0465** | SOT-23-5; combine valid hardware permission with MCU enable. Define pull-downs, thresholds and behavior with either supply absent. |

These rows preserve old custom-board information only. The gate-cut timer and enable logic do not establish a firmware-independent physical stop. Bought-controller sensing, calibration and power-stage parts come with the A50S; no ARC replacement power stage is selected.

## Deprecated corner-connector options

| Connector / choice | JLCPCB | USD each | Quantity / use |
| --- | --- | --- | --- |
| ⚪ **Historical option**, vertical JST **BM10B-GHS-TBT(LF)(SN)** | [C495732](https://jlcpcb.com/partdetail/506963-BM10B_GHS_TBT_LF_SN/C495732) | **~$0.5869 preorder; MOQ 16** | Not selected for the current A50S harness; exact deck FD connectors TBD. |
| ⚪ **Historical option**, right-angle JST **SM10B-GHS-TB** | [C2683602](https://jlcpcb.com/partdetail/Jst-SM10B_GHSTB/C2683602) | **$0.4390** | Legacy GH alternative; current pin numbering follows the revised ICD. |

The old 10-pin JST-GH FD corner interface is superseded by the deck-mounted A50S plan. Current corner phase/Hall connections and the A50S Pico-Clasp assignments follow [ICD-corner-connector](../icd/corner-connector.md); deck FD connectors remain TBD. These historical housings are not current harness selections.

## Historical component sources

[DRV8301](https://www.ti.com/lit/ds/symlink/drv8301.pdf), [AD8418A](https://www.analog.com/en/products/ad8418a.html), [TLC555](https://www.ti.com/lit/ds/symlink/tlc555.pdf), [gate logic](https://www.ti.com/lit/ds/symlink/sn74lvc1g08.pdf), [JST GH drawings](https://www.jst-mfg.com/product/index.php?lang=2&series=105).
