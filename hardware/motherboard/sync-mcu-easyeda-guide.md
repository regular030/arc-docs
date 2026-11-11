# ARC motherboard | EasyEDA Pro design guide

**Working design reference · 2026-11-11 · 14 A3 sheets**

Use this while drawing the combined ARC motherboard in EasyEDA Pro. Sheets **00, 01 and 03–06** cover the power tree, switching, protection and shutdown. Sheet **01** includes the former sheet **02**, and later sheet labels keep their existing numbers. Sheets **07–14** cover the Sync MCU and the vehicle interfaces. The working scope is four drive A50S UARTs, selectable UART or PWM steering hardware, direct Ethernet to the Orin, eight angle ports, two IMU ports, and four ToF ports. The direct ToF draft uses the pull-ups already on each remote board.

This scope differs from the current [drive ICD](../../systems/icd/controller-telemetry.md), [steering ADR](../../systems/adr/0019-steering-off-the-shelf-controller.md), [sensor ADR](../../systems/adr/0045-vehicle-state-sensor-boards.md), and [network ICD](../../systems/icd/network.md). Use this sheet to draft the changes. Review and record the revised interfaces before releasing the PCB or its firmware pinout.

Copy a code from the **JLC / LCSC code** column into the EasyEDA parts search. The links in the last column open the datasheet, wiring reference, or catalog separately. Check the manufacturer part and footprint before placing it. Counts are per motherboard unless marked off-board. **TBD** means the exact part or value still needs to be chosen.

| Start here | Jump to a sheet |
| --- | --- |
| **Power and safety** | [00 Overview](#00--system-overview) · [01 Input protection, ORing and power rails](#01--input-protection-oring-and-power-rails) · [03 Drive and steering power](#03--drive-and-steering-power) · [04 E-stop and watchdog](#04--e-stop-and-watchdog) · [05 Monitoring](#05--battery-and-rail-monitoring) · [06 Compute shutdown](#06--compute-shutdown) |
| **Sync and I/O** | [07 MCU](#07--sync-mcu) · [08 Ethernet](#08--ethernet-to-the-orin) · [09 Drive UARTs](#09--four-a50s-drive-uarts) · [10 Steering UART and PWM](#10--steering-uart-and-pwm) · [11 Angle and IMU](#11--angle-and-imu-ports) · [12 ToF](#12--tof-ports) · [13 Camera and Orin timing](#13--camera-and-orin-timing) · [14 Lighting and status](#14--lighting-status-and-controls) |
| **References** | [STM32 pin sheet](#stm32-pin-sheet) · [Capture and review](#capture-and-review) · [ARC candidate BOM](../../systems/bom/pcb-component-breakdown.md) |

**Open while drawing:** [STM32H723 datasheet](https://www.st.com/resource/en/datasheet/stm32h723zg.pdf) · [LQFP144 top view, Figure 6](https://www.st.com/resource/en/datasheet/stm32h723zg.pdf#page=55) · [ST NUCLEO-H723ZG reference schematic](https://www.st.com/resource/en/schematic_pack/mb1364-h723zg-e01_schematic.pdf) (sheets 3, 4 and 6) · [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html) · [EasyEDA device placement](https://prodocs.easyeda.com/en/schematic/place-device/) · [EasyEDA Footprint Manager](https://prodocs.easyeda.com/en/schematic/tools-footprint-manager/)

## STM32 pin sheet

**U1 candidate: STM32H723ZGT6, LQFP-144, 20 × 20 mm.** STM32 pin names and pad numbers below refer to this package. They are separate from connector contact numbers. Treat assignments as TBC until the EasyEDA symbol, alternate functions, clocks, DMA/interrupts, and firmware mapping agree.

### Peripheral pins

The number in parentheses is the physical **U1 pad**. AF is the STM32 alternate function. UART directions are from U1. SPI directions are from the controller. The port names are draft net groups.

| Sheet / net group | Peripheral | U1 pins and pads | AF |
| --- | --- | --- | --- |
| 08 PHY clock and management | ETH RMII | REF_CLK **PA1 (35)**, MDIO **PA2 (36)**, MDC **PC1 (27)** | 11 |
| 08 PHY receive | ETH RMII | CRS_DV **PA7 (43)**, RXD0 **PC4 (44)**, RXD1 **PC5 (45)** | 11 |
| 08 PHY transmit | ETH RMII | TX_EN **PG11 (126)**, TXD0 **PG13 (128)**, TXD1 **PB13 (74)** | 11 |
| 09 DRIVE_FL | USART1 | TX **PA9 (101)**, RX **PA10 (102)** | 7 |
| 09 DRIVE_FR | USART2 | TX **PD5 (119)**, RX **PD6 (122)** | 7 |
| 09 DRIVE_RL | UART4 | TX **PD1 (115)**, RX **PD0 (114)** | 8 |
| 09 DRIVE_RR | UART7 | TX **PE8 (59)**, RX **PE7 (58)** | 7 |
| 10 STEER_UART | UART8 | TX **PE1 (142)**, RX **PE0 (141)** | 8 |
| 10 STEER_PWM_MCU | TIM17_CH1N | **PB7 (137)** | 1 |
| 07 DEBUG_UART (optional) | USART3 | TX **PD8 (77)**, RX **PD9 (78)** | 7 |
| 07 UART_SPARE (optional) | UART9 | TX **PG1 (57)**, RX **PG0 (56)** | 11 |
| 11 ANGLE_1…4 | SPI1 | SCK **PA5 (41)**, MISO **PA6 (42)**, MOSI **PB5 (135)** | 5 |
| 11 ANGLE_5…8 | SPI2 | SCK **PD3 (117)**, MISO **PB14 (75)**, MOSI **PB15 (76)** | 5 |
| 11 IMU_CG | SPI3 | SCK **PC10 (111)**, MISO **PC11 (112)**, MOSI **PC12 (113)** | 6 |
| 11 IMU_FRONT | SPI4 | SCK **PE12 (65)**, MISO **PE13 (66)**, MOSI **PE14 (67)** | 5 |
| 12 TOF_1 | I2C1 | SCL **PB8 (139)**, SDA **PB9 (140)** | 4 |
| 12 TOF_2 | I2C2 | SCL **PB10 (69)**, SDA **PB11 (70)** | 4 |
| 12 TOF_3 | I2C3 | SCL **PA8 (100)**, SDA **PC9 (99)** | 4 |
| 12 TOF_4 | I2C4 | SCL **PF14 (54)**, SDA **PF15 (55)** | 4 |
| 07 PWR_I2C | I2C5 | SCL **PF1 (11)**, SDA **PF0 (10)** | 6 |
| 07 CELL_SPI (conditional) | SPI5 | SCK **PF7 (19)**, MISO **PF8 (20)**, MOSI **PF9 (21)** | 5 |
| 11 IMU_CG_DRDY | TIM8_CH2 or GPIO input | **PC7 (97)** | 3 for timer |
| 11 IMU_FRONT_DRDY | TIM8_CH3 or GPIO input | **PC8 (98)** | 3 for timer |
| 13 FRAME_TRIGGER_STEREO | TIM4_CH1 | **PD12 (81)** | 2 |
| 13 FRAME_TRIGGER_QUAD | TIM4_CH2 | **PD13 (82)** | 2 |
| 13 EVENT_SYNC_FRONT | TIM4_CH3 | **PD14 (85)** | 2 |
| 13 EVENT_SYNC_DOWN | TIM4_CH4 | **PD15 (86)** | 2 |
| 13 TRIGGER_SPARE | TIM3_CH1 | **PC6 (96)** | 2 |
| 13 PPS_SYNC | TIM2_CH1 | **PA0 (34)** | 1 |
| 07 Clock reserves | HSE and optional LSE | HSE_IN **PH0 (23)**, HSE_OUT **PH1 (24)**, LSE_IN **PC14 (8)**, LSE_OUT **PC15 (9)** | Dedicated |
| 07 Debug and boot | SWD, reset, boot | SWDIO **PA13 (105)**, SWCLK **PA14 (109)**, optional SWO **PB3 (133)**, **NRST (25)**, **BOOT0 (138)** | 0 for SWD/SWO |

### Chip selects

Use one GPIO per active-low CS. Set the idle state and boot-time bias to keep all devices deselected. Angle and IMU buses are separated so each can use its own SPI mode.

| Net | U1 pin (pad) | Net | U1 pin (pad) |
| --- | --- | --- | --- |
| ANGLE_1_CS_N | PE9 (60) | ANGLE_5_CS_N | PD4 (118) |
| ANGLE_2_CS_N | PE10 (63) | ANGLE_6_CS_N | PD7 (123) |
| ANGLE_3_CS_N | PE11 (64) | ANGLE_7_CS_N | PD10 (79) |
| ANGLE_4_CS_N | PE15 (68) | ANGLE_8_CS_N | PD11 (80) |
| IMU_CG_CS_N | PG2 (87) | IMU_FRONT_CS_N | PG3 (88) |
| SENSOR_CS_SPARE_N | PG6 (91) | CELL_CS_N (conditional) | PF6 (18) |

### Control, status and power reserves

These are STM32-side reservations. Polarity, levels, bias and protection depend on the connected circuit.

| Sheet / net group | U1 pins and pads | Use |
| --- | --- | --- |
| 08 PHY_NRST / PHY_IRQ | **PE2 (1)** / **PE3 (2)** | Reset output. PE3 is a reserve with PHY-driven REF_CLK and MDIO polling. |
| 07 ESTOP_STATUS | **PF2 (12)** | Conditioned input on EXTI2 |
| 07 PWR_GOOD / PWR_ALERT_1 / PWR_FAULT_1 | **PE4 (3)** / **PE5 (4)** / **PE6 (5)** | Inputs. EXTI4/5/6 available. Confirm final fault count and source identification with the power designer. |
| 13 ORIN_SHUTDOWN_REQ / ORIN_HALTED | **PG8 (93)** / **PG12 (127)** | Request output, halt input. Confirm the physical interface and power-removal meaning first. |
| 14 LIGHT_RING_EN / LIGHT_GRAZE_EN / LIGHT_VCSEL_EN | **PF3 (13)** / **PF4 (14)** / **PF5 (15)** | Driver enable reserves |
| 14 LED_ESTOP / LED_WATCHDOG / LED_PWR_FAULT | **PG4 (89)** / **PG5 (90)** / **PG7 (92)** | Status output reserves. Population and polarity TBD. |
| 07 BAT_V_AIN / BAT_I_AIN (conditional) | **PB0 (46)** / **PB1 (47)** | ADC1/2 INP9 / INP5. Condition the signals and review range, reference and input impedance before enabling. |

### Pin-map checks before routing

- **Package and AF.** Compare the proposed pads with [ST's package pin file](https://github.com/STMicroelectronics/STM32_open_pin_data/blob/7d1f1514ed5583ec5007ad91236b4e1d377295b1/mcu/STM32H723ZGTx.xml) and [alternate-function table](https://github.com/STMicroelectronics/STM32_open_pin_data/blob/7d1f1514ed5583ec5007ad91236b4e1d377295b1/mcu/IP/GPIO-STM32H72_gpio_v1_0_Modes.xml). Power and analog package pins are listed in [DS13313, section 5](https://www.st.com/resource/en/datasheet/stm32h723zg.pdf#page=55). This worksheet covers assigned signal pads.
- **Five I²C buses.** Four direct ToFs plus the dedicated power monitor use I2C1–I2C5. The pinned Zephyr v4.4.2 [STM32H7 base](https://github.com/zephyrproject-rtos/zephyr/blob/v4.4.2/dts/arm/st/h7/stm32h7.dtsi) and [H723 include](https://github.com/zephyrproject-rtos/zephyr/blob/v4.4.2/dts/arm/st/h7/stm32h723.dtsi) declare I2C1–I2C4, while the pinctrl data has I2C5 pins. Bring up the I2C5 peripheral node, clock/reset/IRQ and driver before freezing this map. For a mux fallback, one ToF I²C plus one separate power I²C can use the declared controllers.
- **Ethernet clock.** ToF I2C3 uses both PA8 and PC9, the MCO1 and MCO2 pins. Use a reviewed PHY-driven or external 50 MHz RMII reference on PA1 with this map.
- **Timing and DMA.** TIM4 channels share a timer counter. Confirm the output-compare schedule can generate each camera's required period and pulse width. Decide whether the two DRDY inputs use timer capture or GPIO EXTI, then check interrupt-line ownership. Five full-duplex UARTs plus five full-duplex SPI peripherals would need 20 RX/TX streams if all use dedicated DMA, versus 16 DMA1/2 streams. Plan where to use IRQs or scheduled transfers, and check I2C4's separate BDMA path if using DMA. Test concurrent traffic and buffer placement on the [production board target](../../firmware/sync-mcu/boards/arc/arc_sync/arc_sync.dts), which is currently a scaffold. [ST DMA request mapping](https://github.com/STMicroelectronics/stm32h7xx-hal-driver/blob/master/Inc/stm32h7xx_ll_dmamux.h)
- **Servo timing.** PB7 uses the complementary output of its own timer. Configure TIM17 channel 1's N output and verify positive pulses and idle-low behavior at the header. TIM4 stays available for the four camera channels.

## 00 — System overview

Draw the block diagram and label the interfaces. Keep the power and Sync MCU sections on one PCB. Show the Orin carrier, motor controllers, cameras and 14 sensor boards at the ends of their harnesses. The overview page has no BOM placements. [Motherboard architecture](README.md) · [Internal power/sync contract](../../systems/icd/power-sync-stack.md)

| From → to | What to show on the overview |
| --- | --- |
| Within 01 | The battery/wall ORed feed and its return connect the input section to the rail stages. The wall input feeds this path only. |
| 01 → 03 | Battery-only motor feed and rated return. 03 switches the drive and steering branches separately. |
| 01 → loads and 07 | Protected compute, sensor, 5 V and 3.3 V rails. Distinguish the proposed 12 V LiDAR branch from the low-voltage remote sensor rails. Label source, switched load and sensing point. |
| 04 → 03, 07 and A50S | Independent delayed drive/steering cut controls, conditioned **ESTOP_STATUS** to the MCU, and hardwired **ESTOP** to all four drive controllers. |
| 05 → 07 | **PWR_I2C**, identifiable fault/alert signals, and conditional **CELL_SPI** or analog fallback. These are internal PCB nets. |
| 07 / 13 → 06 and Orin | Shutdown request to the Orin, verified halt/power-removal permission back to the cutoff circuit, and separate timing signals. |
| 07 → 08–14 | One direct Orin Ethernet link, four drive UARTs, steering UART or PWM, eight angle and two IMU SPI ports, four ToF I²C ports, timing and controls. |

## 01 — Input protection, ORing and power rails

Draw this power flow left to right across the A3 sheet. The diagram shows the connections between blocks. Use the linked application schematics for connections within each block.

```text
BAT_IN  (4S, 12.0–16.8 V) → battery input protection ─┬→ battery-only feed → sheet 03
                                                      └→ battery ideal diode ─┐
WALL_IN (19 V planned)    → wall input protection  ────→ wall ideal diode ────┴→ ORed LV feed

ORed LV feed → rail conversion and distribution → COMPUTE, SENSOR (proposed 12 V),
                                                  LOGIC_5V, LOGIC_3V3
```

1. Draw both input connectors, fuses, surge protection, rated returns, and the battery loop key/anti-spark interface. Use one LM74700 and its MOSFET per input. Join their outputs at the ORed feed. Check MOSFET orientation, reverse current into the battery, and handover under load.
2. Draw each rail as a named block with its input, output, and return. Show the branch fuse, switch or enable, shunt location, and test points. Size the stages from the load budget, including compute startup and LiDAR inrush. Confirm the compute voltage and each stage’s input source before setting feedback or wiring the branches. [Power rails ICD](../../systems/icd/power-rails.md) · [Power budget](../../systems/budgets/power-scenarios.md)

| Function | Qty / exact candidate | JLC / LCSC code | Wiring reference or selection check |
| --- | --- | --- | --- |
| Ideal-diode controllers | 2 × **LM74700QDBVRQ1** | `C2941042` | One per input. Follow the two-input circuit for the MOSFET and sense connections. [TI ORing circuit, Fig. 10-18](https://www.ti.com/lit/ds/symlink/lm74700-q1.pdf#page=22) · [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-LM74700QDBVRQ1/C2941042) |
| ORing and converter MOSFETs | 8 × **CSD18540Q5B**, provisional | `C86513` | Two for ORing, four for the 12 V stage, and two for the 5 V stage. Check body diode, gate drive, loss and transient stress for each use. [TI pinout and ratings](https://www.ti.com/lit/ds/symlink/csd18540q5b.pdf) · [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-CSD18540Q5B/C86513) |
| Surge TVS | **SMCJ24A**, SETsafe / SETfuse, qty TBD | `C2996490` | Check the clamp against every part on the protected node. The catalog gives 38.9 V at the specified pulse, and the INA226 operating bus limit is 36 V if it sees this node. [Catalog and datasheet](https://jlcpcb.com/partdetail/SETsafe_SETfuse-SMCJ24A/C2996490) |
| Input connectors, fuses and anti-spark support | TBD | TBD | Set contact and fuse ratings from the current budget. Choose bypass, gate components and bulk capacitance for the reviewed topology. |
| Proposed 12 V buck-boost | 1 × **LM5176PWPR** | `C442493` | Draw the four switches, inductor, input/output caps, sense and compensation. Size these for ARC's load. [TI 12 V application schematic, Fig. 8-1](https://www.ti.com/lit/ds/symlink/lm5176.pdf#page=21) · [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-LM5176PWPR/C442493) |
| Main 5 V buck | 1 × **LM5146RGYR** | `C3188679` | Draw the two external switches, inductor, feedback and compensation. Recalculate values for ARC's input and load. [TI 5 V application schematic, Fig. 9-5](https://www.ti.com/lit/ds/symlink/lm5146.pdf#page=36) · [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-LM5146RGYR/C3188679) |
| Main 3.3 V buck | 1 × **TPS54360DDAR** | `C44377` | Use the example for the catch diode, inductor and feedback connections. The example sets 5 V. Calculate feedback and support values for 3.3 V. [TI application schematic, Fig. 34](https://www.ti.com/lit/ds/symlink/tps54360.pdf#page=25) · [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-TPS54360DDAR/C44377) |
| Quiet sensor supply | Up to 2 × **TPS7A2033PDBVR**, provisional | `C2862740` | Place here or on sheet 11 and count once. Check input headroom, current, thermal load and capacitance. [TI LDO application circuit, Fig. 7-4](https://www.ti.com/lit/ds/symlink/tps7a20.pdf#page=30) · [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-TPS7A2033PDBVR/C2862740) |
| Magnetics, passives and branch protection | TBD | TBD | Size from worst-case voltage/current, ripple, loss, transient response and startup. Include gate/bootstrap networks, fuses, switches, test points and rated returns. |

## 03 — Drive and steering power

Draw the battery-fed drive-bus switch to the four A50S controllers and a separate fused steering output. The drive switch opens when its control is unpowered. Review the drive switch's inrush, faults and thermal limits, and place the regen path so braking energy can still reach the clamp when the drive switch opens. Route motor current through rated power returns and keep it out of UART and sensor references. Confirm whether A50S Aux power is needed and which range applies to the actual hardware. [Power and clamp open items](../../systems/handoff/electrical.md) · [Power rails ICD](../../systems/icd/power-rails.md)

| Function | Qty / exact candidate | JLC / LCSC code | Wiring reference or selection check |
| --- | --- | --- | --- |
| Steering eFuse | 1 × **TPS26630RGET**, conditional | `C2864391` | Check peak/inrush, thermal limits and reverse/regen behavior before choosing it. [TI datasheet](https://www.ti.com/lit/ds/symlink/tps2663.pdf) · [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-TPS26630RGET/C2864391) |
| Drive switch, driver, fuse, inrush network and high-current contacts | TBD | TBD | Set ratings using branch current, faults, inrush and the proposed delay. Include the rated return and gate/coil bias that produces a safe unpowered state. |
| Optional clamp comparator | 1 × **TLV3201AIDBVR**, provisional | `C105188` | [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-TLV3201AIDBVR/C105188) |
| Optional clamp reference | 1 × **REF3325AIDBZR**, provisional | `C69039` | [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-REF3325AIDBZR/C69039) |
| Optional clamp gate driver | 1 × **TC4427EOA**, provisional | `C636891` | [Catalog](https://jlcpcb.com/partdetail/MicrochipTech-TC4427EOA/C636891) |
| Optional clamp dump MOSFET | 1 × **CSD18540Q5B**, provisional | `C86513` | [TI datasheet](https://www.ti.com/lit/ds/symlink/csd18540q5b.pdf) · [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-CSD18540Q5B/C86513) |
| Optional clamp supply | 1 × **TPS54360DDAR**, provisional | `C44377` | [TI application schematic](https://www.ti.com/lit/ds/symlink/tps54360.pdf#page=25) · [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-TPS54360DDAR/C44377) |

Populate the clamp parts only if the clamp goes on this PCB. Size the dump resistor, threshold, hysteresis, heatsink and transient protection from the braking model. [Clamp reference and candidate datasheets](../../systems/bom/pcb-parts-selection.md#motor-bus-clamp)

## 04 — E-stop and watchdog

Draw the latching normally closed button input, independent brake request to all four A50S controllers, delayed drive and steering cut controls to sheet 03, and conditioned **ESTOP_STATUS** to sheet 07. The working timing is drive cut at **2.8 s TBC**, then steering **1 s later TBC**. Verify both cuts with the H723 reset/unpowered and check re-arm behavior. The firmware watchdog and UART command timeout remain separate safety functions. [Power sequencing](../../systems/icd/power-rails.md#operating-state-contract) · [E-stop timing decision](../../systems/adr/0057-estop-delay-and-mass.md)

| Function | Qty / exact candidate | JLC / LCSC code | Wiring reference or selection check |
| --- | --- | --- | --- |
| Delay timer | 1 × **TLC555IDR**, candidate | `C6987` | Steering delay candidate. Review its protected supply, tolerance, reset, and behavior on power loss. [TI datasheet](https://www.ti.com/lit/ds/symlink/tlc555.pdf) · [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-TLC555IDR/C6987) |
| Physical button | **DFRobot FIT0156**, off-board pricing candidate | — | Verify the actual latching NC contact block. [ARC electrical BOM](../../systems/bom/electrical.md) |
| Input conditioning, brake driver, drive timer, steering timer network and connector | TBD | TBD | Review polarity, default state, opens/shorts, timing limits and independent hardware cut. Confirm the A50S stop input on the actual controllers. |

## 05 — Battery and rail monitoring

Draw the protected 4S balance tap input, pack shunt, branch shunts, and monitor alerts. Connect the bus monitors to **PWR_I2C**. The LTC6810 candidate needs a reviewed SPI/isoSPI implementation on **CELL_SPI** rather than a host I²C hookup. Agree on a single cell monitor implementation and verify the per-cell **≥100 Hz, ±10 mV** requirement, tap protection and off-state drain. [Internal power/sync contract](../../systems/icd/power-sync-stack.md) · [System requirements](../../systems/requirements/system.md)

| Function | Qty / exact candidate | JLC / LCSC code | Wiring reference or selection check |
| --- | --- | --- | --- |
| Pack/branch voltage and current | 6 × **INA226AIDGSR**, provisional | `C49851` | Pack, compute, sensor, 5 V, 3.3 V and steering. Draw Kelvin connections, filter, bypass, address straps and identifiable alerts. [TI datasheet](https://www.ti.com/lit/ds/symlink/ina226.pdf) · [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-INA226AIDGSR/C49851) |
| Pack shunt | 1 × **HoLRS3920-5W-1mR-1%**, candidate | `C558628` | Check continuous/pulse dissipation and the board's copper and thermal limits. [Catalog and datasheet](https://jlcpcb.com/partdetail/Milliohm-HolRS3920_5W_1mR_1/C558628) |
| Cell monitor | 1 × **LTC6810IG-1#3ZZPBF**, candidate | `C689559` | BOM reference. Verify sourcing. Use the datasheet's 4S wiring, unused-input connections, tap filtering and supply scheme. [ADI datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/ltc6810-1-6810-2.pdf) |
| Cell monitor alternate | **LTC6810IG-2#3ZZTRPBF**, conditional replacement | `C689562` | Review the different interface/addressing and pin connections before substitution. [ADI datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/ltc6810-1-6810-2.pdf) · [Catalog](https://jlcpcb.com/partdetail/AnalogDevices-LTC6810IG_23ZZTRPBF/C689562) |
| Analog current-sense alternate | **INA240A1PWR**, conditional replacement per selected channel | `C93965` | Needs ADC, reference and separate bus-voltage sensing if required. [TI datasheet](https://www.ti.com/lit/ds/symlink/ina240.pdf) · [Catalog](https://jlcpcb.com/partdetail/TexasInstruments-INA240A1PWR/C93965) |
| Branch shunts, balance connector and tap/logic support | TBD | TBD | Size each branch separately. Choose the tap protection, level/interface parts and passives after deciding the cell-monitor circuit. |

## 06 — Compute shutdown

Draw the proposed sequence **lowest-cell warning → Orin shutdown request → verified halt/power-removal permission → compute switch off**. Route the carrier signals through sheet 13 and show the independent hardware undervoltage backstop. Resolve what signals actual halt on both Orin builds. Define the behavior on timeout or loss of the H723. [Carrier handshake](../../systems/icd/carrier-sync.md) · [Shutdown proposal](../../systems/adr/0022-power-board-telemetry-and-shutdown.md)

| Function | Qty / exact part | JLC part number | What needs a decision |
| --- | --- | --- | --- |
| Compute rail switch and driver | TBD | TBD | Rail, hold-up, inrush, sense point and cutoff behavior. Coordinate the switch with sheet 01. |
| Shutdown request and halt feedback | TBD | TBD | Actual carrier signal, direction, translation/bias, connector contacts and timeout. |
| Hardware undervoltage backstop | TBD | TBD | Threshold, hysteresis, supervisor/comparator and safe state. |

## 07 — Sync MCU

1. Place all units of **U1 STM32H723ZGT6**, LQFP-144, in ST's internal LDO configuration. Connect **VDD/VDDLDO pads 17, 30, 39, 52, 62, 72, 84, 108, 121 and 144**, **VBAT pad 6** and **PDR_ON pad 143** to `LOGIC_3V3`. Connect **VSS pads 16, 38, 51, 61, 83, 94, 107, 120 and 130** to `GND`. Tie **VSSA pad 31** to the quiet ground return. Feed **VDDA pad 33** (`VDDA_MCU`) and **VREF+ pad 32** (`VREF_MCU`) from `LOGIC_3V3` through the reference links, with local analog bypass. Connect **VDD33USB pad 95** to `LOGIC_3V3` with local bypass as in the reference. Join **VCAP pads 71 and 106** on `MCU_VCAP` and fit **one local 2.2 µF per pad** to GND. [ST pinout](https://www.st.com/resource/en/datasheet/stm32h723zg.pdf#page=55) · [ST Nucleo power circuit](https://www.st.com/resource/en/schematic_pack/mb1364-h723zg-e01_schematic.pdf#page=4)
2. Draw a 25 MHz HSE crystal between `MCU_HSE_IN` PH0 and `MCU_HSE_OUT` PH1, using the Nucleo oscillator circuit as the starting point. Put each load capacitor to GND and size the pair for the exact crystal and board stray capacitance. Set firmware HSE to 25 MHz when that crystal is populated. Reserve PC14/PC15 for LSE if needed. [Nucleo clocks and reset](https://www.st.com/resource/en/schematic_pack/mb1364-h723zg-e01_schematic.pdf#page=3) · [ST crystal design guide](https://www.st.com/resource/en/application_note/an2867-guidelines-for-oscillator-design-on-stm8afals-and-stm32-mcusmpus-stmicroelectronics.pdf)
3. Pull `MCU_BOOT0` low through 10 kΩ and expose it on a service pad. Draw `MCU_NRST` with the reference 100 nF to GND and route it to SWD. On a keyed **1.27 mm Cortex Debug** footprint, use pins **1** VTref/`LOGIC_3V3`, **2** `MCU_SWDIO`, **3/5/9** GND, **4** `MCU_SWCLK`, **6** optional `MCU_SWO`, **7** key, **8** NC, **10** `MCU_NRST`. Add test pads for the 3.3 V console `DEBUG_UART_TX`, `DEBUG_UART_RX`, and `GND`. [ST debug and boot guidance](https://www.st.com/resource/en/application_note/dm00663674.pdf)
4. Add the ports from the [STM32 pin sheet](#stm32-pin-sheet). Bring `PWR_I2C_SCL`, `PWR_I2C_SDA`, `ESTOP_STATUS`, `PWR_GOOD`, `PWR_ALERT_1` and `PWR_FAULT_1` to the conditioned power-side circuits. Add optional `CELL_SPI_*` and analog sense nets if sheet 05 uses them. Export the exact `ETH_*`, `DRIVE_*`, `STEER_*`, `ANGLE_*`, `IMU_*` and `TOF_*` labels specified below. [Power and Sync contract](../../systems/icd/power-sync-stack.md)

| Place | Qty | JLC / LCSC code |
| --- | --- | --- |
| STM32H723ZGT6, LQFP-144 | 1 | `C730146` |
| 100 nF X7R, 0603 | One at each VDD plus reset and supply bypass per ST | `C14663` |
| 2.2 µF X7R, 10 V, 0603, Yageo CC0603KRX7R6BB225 | 2 for VCAP | `C513691` |
| 10 µF X7R bulk, 1 µF analog bypass, 0 Ω supply links | Local digital bulk, analog bypass and analog links per ST reference | `C6105339`, `C307479`, `C21189` |
| 10 kΩ, 0603 | 1 for BOOT0 | `C25804` |
| NDK NX2016SA-25MHZ-STD-CZS-2, 8 pF load, 2.0 × 1.6 mm | 1 HSE candidate plus two C0G load capacitors. 5.6 pF is a tuning part, verify against calculated loading. | `C843258`, `C2347399` |
| Samtec FTSH-105-01-L-DV-007-K, Cortex Debug 2×5, 1.27 mm, pin 7 omitted | 1 keyed SWD candidate | `C5296739` |

## 08 — Ethernet to the Orin

Draw the STM32 ports, LAN8742A PHY, 10/100 magnetics and a shielded RJ45 marked **TO ORIN**. Use the discrete magnetics circuit from the ST and Microchip references. In EasyEDA, search for the RCH **RC00234** jack by its catalog code or manufacturer part number. Use the footprint for whichever jack you place and check its board-edge fit. The cable runs from this RJ45 to the Orin's Ethernet port. Confirm the LiDAR/service path and IP plan because the [network ICD](../../systems/icd/network.md) allocates a switch. [ST Nucleo Ethernet schematic](https://www.st.com/resource/en/schematic_pack/mb1364-h723zg-e01_schematic.pdf#page=6) · [Microchip schematic checklist](https://ww1.microchip.com/downloads/aemDocuments/documents/OTH/ProductDocuments/SupportingCollateral/LAN8742AQFNRevASchematicChecklist.pdf)

| Net | U1 STM32 pin (pad) | LAN8742A pad |
| --- | --- | --- |
| `ETH_REF_CLK` | PA1 (35), input | **14** REFCLKO → U1, 33 Ω series near PHY |
| `ETH_MDIO` | PA2 (36), bidirectional | **12**, 1.5 kΩ pull-up to `LOGIC_3V3` |
| `ETH_MDC` | PC1 (27), output | **13** |
| `ETH_CRS_DV` | PA7 (43), input | **11** / MODE2, 10 Ω series near PHY |
| `ETH_RXD0` | PC4 (44), input | **8** / MODE0, 10 Ω series near PHY |
| `ETH_RXD1` | PC5 (45), input | **7** / MODE1, 10 Ω series near PHY |
| `ETH_TX_EN` | PG11 (126), output | **16** |
| `ETH_TXD0` | PG13 (128), output | **17** |
| `ETH_TXD1` | PB13 (74), output | **18** |
| `PHY_NRST` | PE2 (1), output | **15**, active low |

1. Wire PHY pad **5** to crystal pad **1** and PHY pad **4** through **499 Ω** to crystal pad **3** for Microchip's 100 µW circuit. Ground crystal pads **2/4**. Place one C0G load capacitor from each crystal terminal to GND. **5.6 pF** is the Nucleo starting value. Verify loading and startup on the ARC layout. Use distinct labels on both sides of each series resistor in EasyEDA.
2. Use **10 kΩ pulldowns** on PHY pads **2, 3 and 10** for REFCLKO mode, the internal regulator and PHY address **0**. Use **10 kΩ pull-ups** on PHY pads **7, 8 and 11** on the PHY side of their series resistors for auto-negotiation. Pad 14 drives **50 MHz** to PA1. Reserve PE3 and poll link through MDIO. Use PE2 to drive `PHY_NRST`, with a **4.7 kΩ pulldown** as the startup bias candidate. Assert reset for **≥100 µs** and release it **≥25 ms** after the PHY rails reach operating voltage. Keep the MAC outputs high-impedance until the PHY is powered. [Microchip LAN8742A datasheet](https://ww1.microchip.com/downloads/aemDocuments/documents/OTH/ProductDocuments/DataSheets/DS_LAN8742_00001989A.pdf)
3. Feed **pad 9** with `LOGIC_3V3` and **pads 1/19** with ferrite-filtered `ETH_3V3A`. Add **10 nF** locally to GND at each of those three pads, and **10 µF** to GND on each side of the ferrite as local bulk. Ground the exposed pad. Put **12.1 kΩ ±1%** from `PHY_RBIAS` **pad 24** to GND. With the internal regulator enabled, bypass **pad 6 `PHY_VDDCR`** with **1 µF + 470 pF** to GND. Place the 470 pF closest to the pin.
4. With **KMS-1102NL**, route the pairs in this table. Add one **49.9 Ω ±1% to `ETH_3V3A`** on each of the four PHY-side lines. Tie PHY-side center taps **2 and 7** to `ETH_3V3A` with a **22 nF** bypass to board GND. Place the USBLC6-4SC6 in parallel on the PHY side. Its **pads 1/6** connect to `ETH_RX_P/N`, **3/4** to `ETH_TX_P/N`, **2** to board GND and **5** to `ETH_3V3A` with a local **100 nF** bypass.
5. Bring KMS cable-side center taps **15 and 10** through **75 Ω each** to `ETH_CABLE_TERM`, then through **1 nF rated at least 2 kV** to `ETH_SHIELD`. For this non-PoE link, join RJ45 contacts **4+5** and **7+8** separately and bring each pair through its own **75 Ω** to `ETH_CABLE_TERM`. Connect both RJ45 shield tabs to `ETH_SHIELD` and keep the cable-side isolation boundary on the PCB. A different transformer or MagJack requires its own pin and termination map. [Microchip termination checklist](https://ww1.microchip.com/downloads/aemDocuments/documents/OTH/ProductDocuments/SupportingCollateral/LAN8742AQFNRevASchematicChecklist.pdf#page=4)
6. Route `ETH_TX_P/N` and `ETH_RX_P/N` as **100 Ω differential pairs**. Keep `ETH_REF_CLK` short and verify the 50 MHz clock, MDIO response at address 0, and Orin link before starting network tests.

| PHY-side net | PHY pad | KMS PHY-side pad → cable-side pad | Cable-side net | RJ45 contact |
| --- | --- | --- | --- | --- |
| `ETH_TX_P` | 21 | 1 → 16 | `ETH_CABLE_TX_P` | 1 |
| `ETH_TX_N` | 20 | 3 → 14 | `ETH_CABLE_TX_N` | 2 |
| `ETH_RX_P` | 23 | 6 → 11 | `ETH_CABLE_RX_P` | 3 |
| `ETH_RX_N` | 22 | 8 → 9 | `ETH_CABLE_RX_N` | 6 |

| Place | Qty / candidate | JLC / LCSC code |
| --- | --- | --- |
| LAN8742AI-CZ-TR, VQFN-24-EP | 1 PHY | `C621425` |
| NDK NX2016SA-25MHZ-STD-CZS-2 | 1 PHY crystal, separate from the MCU crystal. Check drive level, ESR and load against Microchip's Section 5.7 circuit. | `C843258` |
| KMS-1102NL transformer | 1 discrete transformer. Check its operating temperature rating before routing. | `C19725172` |
| RCH RC00234 shielded RJ45 | 1 plain 8P8C right-angle through-hole jack. Match contacts 1–8 and both shield tabs to the [manufacturer drawing](https://datasheet.lcsc.com/datasheet/pdf/1248e73397f1f66c0d85200f9d831325.pdf?productCode=C708601) in Footprint Manager. | `C708601` |
| Amphenol RJHSE5380 shielded RJ45 | Alternate jack with the same contact assignments. Use its own [footprint drawing](https://cdn.amphenol-cs.com/media/wysiwyg/files/drawing/rjhsex380.pdf). | `C464586` |
| USBLC6-4SC6 | 1 Ethernet ESD array in the ST circuit | `C111212` |
| BLM18AG601SN1D, 600 Ω at 100 MHz | 1 PHY analog-supply bead | `C19330` |
| 49.9 Ω, 12.1 kΩ, 1.5 kΩ, 33 Ω, 10 Ω, 10 kΩ | MDI ×4, RBIAS ×1, MDIO ×1, clock series ×1, PHY receive outputs ×3, straps ×6 | `C1859327`, `C3956831`, `C22843`, `C23140`, `C22859`, `C25804` |
| 10 nF, 10 µF, 100 nF, 1 µF, 470 pF, 22 nF, 1 nF/2 kV | Local supply/ESD/core/center-tap/cable-side bypass | `C57112`, `C6105339`, `C14663`, `C307479`, `C1620`, `C21122`, `C9196` |
| 75 Ω, 499 Ω, 5.6 pF C0G | Cable termination ×4, XTAL2 series ×1 for the 100 µW circuit, crystal load tuning candidate ×2 | `C4275`, `C478882`, `C2347399` |
| Reset bias | 4.7 kΩ startup pulldown candidate | `C23162` |

## 09 — Four A50S drive UARTs

Place four identical **7-way JST XH 2.50 mm** motherboard ports, marked FL/FR/RL/RR. The harness adapts from XH at the motherboard to the **20-way Molex Pico-Clasp** on each A50S V2.3c. Use **3.3 V, 2 Mbit/s, 8N1**, one independent link per controller. Directions and labels below are from the motherboard.

| Port | TX net → U1 pad | RX net → U1 pad |
| --- | --- | --- |
| `J_DRIVE_FL` | `DRIVE_FL_TX` → PA9 (101) | `DRIVE_FL_RX` → PA10 (102) |
| `J_DRIVE_FR` | `DRIVE_FR_TX` → PD5 (119) | `DRIVE_FR_RX` → PD6 (122) |
| `J_DRIVE_RL` | `DRIVE_RL_TX` → PD1 (115) | `DRIVE_RL_RX` → PD0 (114) |
| `J_DRIVE_RR` | `DRIVE_RR_TX` → PE8 (59) | `DRIVE_RR_RX` → PE7 (58) |

| XH contact | Motherboard signal | A50S Pico-Clasp contact, **provisional** |
| --- | --- | --- |
| 1 | `DRIVE_<corner>_TX` | **12**, A50S RX / PB11 |
| 2 | TX reference, `GND` | **2**, signal reference |
| 3 | `DRIVE_<corner>_RX` | **14**, A50S TX / PB10 |
| 4 | RX reference, `GND` | **20**, signal reference |
| 5 | `A50S_ESTOP_N` from the independent sheet 04 stop driver | **3**, PPM / PB6 active-low stop input in ARC firmware |
| 6 | Stop reference, `GND` | **1**, signal reference |
| 7 | Spare, NC on motherboard | Spare, park/insulate at controller |

Check the mating-face orientation against the actual controller before crimping. With the provisional numbering, BEC outputs read **5 V on circuit 9** and **3.3 V on circuit 10**. Then identify TX and RX directly and check continuity end to end. Pair each UART signal with its reference. Carry motor current on its rated XT30 return, keep the loom away from phase leads, and verify the signal ground and backfeed behavior with each side unpowered. Leave the Pico-Clasp BEC and Aux power contacts out of this signal harness. Preserve SWD service access. [Triforce connector drawing](https://cdn11.bigcommerce.com/s-4t55i5fv4j/images/stencil/original/image-manager/pinout-v2.3.png?t=1684596909) · [ARC connector ICD](../../systems/icd/corner-connector.md)

Use separate precrimped pigtails and strain-relieved splices where the two ends use the specified crimps. Their approved insulation diameters differ. The XH contact takes **0.9–1.9 mm** insulation OD, while the selected Pico-Clasp terminal takes **0.4–0.8 mm**. Keep each UART signal paired with its reference through the splice area. [JST crimp range](https://www.jst-mfg.com/product/pdf/eng/eXH.pdf) · [Molex crimp range](https://www.molex.com/content/dam/molex/molex-dot-com/products/automated/en-us/crimpspecificationpdf/501/501193/CS-501193-001-001.pdf)

| Place or buy | Qty for four ports | JLC / LCSC code |
| --- | --- | --- |
| JST B7B-XH-A(LF)(SN) vertical THT header | 4 on motherboard | `C144398` |
| JST XHP-7 and SXH-001T-P0.6 crimps | 4 housings and 28 contacts if the spare is installed | `C144406`, `C140573` |
| Molex 501189-2010 and 501193-7000 female gold crimps | 4 housings, at least 24 populated contacts for these signals | `C504999`, `C581416` |
| 0603 series links and test pads | 2 links and 2 signal test pads per port. Start with 0 Ω at TX/RX footprints for bench capture, then fit damping/protection based on measured edges and off-state tests. | `C21189` (0 Ω), `C23140` (33 Ω tuning candidate) |

Keep the independent `A50S_ESTOP_N` path and delayed motor-bus cut. Leave the former CAN transceiver and termination out of this capture draft. The active drive ICD still assigns commands to CAN, and ARC UART streaming/command firmware is unfinished. Prove all four links together with the motors switching, then verify fresh-command gating, link-loss braking, independent e-stop and re-arm before enabling drive. [Controller ICD](../../systems/icd/controller-telemetry.md) · [UART readiness](../../systems/reviews/a50s-uart-readiness.md)

## 10 — Steering UART and PWM

Draw a UART port for a steering motor controller and a separate PWM port for the existing RC servo. Fit or connect **one actuator path per vehicle** and select its protocol in firmware. Use the separately switched/fused steering branch from sheets 03–04 and preserve its delayed hardware cut. Place the 7-way steering UART port so the four drive looms cannot reach it, and label both ends of the steering adapter. The [accepted steering ADR](../../systems/adr/0019-steering-off-the-shelf-controller.md) selects moteus-c1 and supersedes the fifth-VESC design. The fifth A50S and RC servo are alternate capture options for review.

| Header | Contacts on the motherboard |
| --- | --- |
| `J_STEER_UART`, 7-way XH, mark **STEER** | 1 `STEER_UART_TX` from PE1 (142), 2 GND, 3 `STEER_UART_RX` to PE0 (141), 4 GND, 5 `STEER_A50S_ESTOP_N` optional from the reviewed hardware stop driver, 6 GND, 7 spare/NC |
| `J_STEER_SERVO`, 3-way XH, mark **SERVO** | 1 `SERVO_GND`, 2 `SERVO_VPLUS`, 3 `STEER_PWM`. Use a model-specific adapter from this contact order to the actual servo plug. |

- **Existing RC servo.** If the label confirms a standard three-wire PWM servo, route PB7 (137), TIM17_CH1N, as `STEER_PWM_MCU` through a 33 Ω footprint to `STEER_PWM`, with a 100 kΩ pulldown to `SERVO_GND` on the connector side. Feed `SERVO_VPLUS` and `SERVO_GND` from a **dedicated model-rated BEC** on the delayed switched steering branch. Bond the servo return to board ground at the power-return connection so PWM has a reference and stall current bypasses MCU ground traces. Size the BEC, header and return using specified or measured stall current. Verify the supply voltage and contact polarity before plugging it in, then bench-check the PWM threshold, pulse range, frame rate, loss-of-pulse behavior and power-off backfeed. If it needs a higher signal level, fit a buffer with a defined power-off state. The funding proposal records donor servo reuse. Its electrical ratings still require the model or unit label. [ARC funding proposal](https://docs.google.com/presentation/d/1dQNPlx5o0vL_Jx3N6AF2Sjgx_vF8eXRcj7DSrOtTeU4/edit#slide=id.p4)
- **Fifth A50S.** Use `J_STEER_UART` with the XH-to-Pico-Clasp mapping on sheet 09, adapting pin 5 to the reviewed steering stop input. Take switched motor power on XT30 and bare motor phases on MR30. This path needs a compatible bare BLDC motor, position feedback, VESC UART position-control integration, calibrated travel/torque limits and a tested command-loss strategy. An assembled three-wire servo takes PWM and BEC power on `J_STEER_SERVO`. [A50S connectors](https://teamtriforceuk.com/a50s-v2/) · [VESC command implementation](../../firmware/vesc/bldc/comm/commands.c)
- **moteus-c1.** Same motherboard UART header with a different adapter. Connect XH 1 → c1 AUX2 **6/D** (RX), c1 AUX2 **5/C** (TX) → XH 3, and AUX2 **7** → signal ground. Leave pin 5 of the XH and the AUX supply contacts unused. Use firmware **1.1.2 or newer** and the vendor UART default **921600 baud, 8N1**, then verify the configured loss-of-command response. [mjbots pinout](https://mjbots.github.io/moteus/reference/encoders/#c1n1x1-pins-aux2abs) · [UART guide](https://mjbots.github.io/moteus/integration/uart/)

| Place | Qty | JLC / LCSC code |
| --- | --- | --- |
| JST B7B-XH-A(LF)(SN), XHP-7 housing | 1 header and 1 housing for UART option | `C144398`, `C144406` |
| JST B3B-XH-A(LF)(SN), XHP-3 housing | 1 header and 1 housing for servo option | `C144394`, `C144402` |
| JST SXH-001T-P0.6 crimps | One per populated XH cable contact | `C140573` |
| 0 Ω UART link, 33 Ω PWM series resistor, 100 kΩ PWM bias | 2 UART links, 1 PWM series, 1 PWM bias | `C21189`, `C23140`, `C25803` |
| JST GHR-07V-S | 1 if using c1 AUX2 | `C398498` |
| BEC, output bulk, fuse, level buffer, servo-end mate | Conditional on the actual servo and stall current | TBD |

For either alternative, qualify real knuckle-angle feedback, loss-of-command behavior, centering or hold behavior, and the independent cut with the MCU reset. Drive stays inhibited until steering is healthy. Check that both front-knuckle sensors remain in the eight-angle allocation before relying on them for this check. [Steering health ADR](../../systems/adr/0038-steering-loss-stops-the-car.md)

## 11 — Angle and IMU ports

Use **JST XH 2.50 mm shrouded male headers on both boards**. Each sensor cable has two female XHP housings, female crimps, and straight-through numbered contacts. Check pin 1 from the mounting and mating views before building the first cable. Use the same order on the remote schematic and motherboard. Test pin-to-pin continuity before attaching power. [JST XH drawings](https://www.jst-mfg.com/product/pdf/eng/eXH.pdf)

```text
Motherboard male XH       female XHP ══ harness ══ female XHP       Sensor male XH
       pin 1                  1  ═════════════════════  1                pin 1
       pin 2                  2  ═════════════════════  2                pin 2
         …                    …                         …                   …
       pin n                  n  ═════════════════════  n                pin n
```

Bring `ANGLE_3V3` and `IMU_3V3` from the qualified 3.3 V sensor supply on sheet 01. If separate quiet LDOs are used, connect each sensor net to its assigned LDO output and check the aggregate startup and running current on that branch.

### Eight angle ports

Place `J_ANGLE_1` through `J_ANGLE_8`. The following is the proposed connector order on **both boards**.

| Contact | Motherboard and cable | AS5047P-ATSM on the remote board |
| --- | --- | --- |
| 1 | `ANGLE_3V3` | Pins 11 VDD and 12 VDD3V3 tied for 3.3 V mode, local 100 nF to GND |
| 2 | `GND` | Pin 13 GND and pin 5 TEST |
| 3 | `ANGLE_A_SCK` or `ANGLE_B_SCK` | Pin 2 CLK |
| 4 | `ANGLE_A_MOSI` or `ANGLE_B_MOSI` | Pin 4 MOSI |
| 5 | `ANGLE_A_MISO` or `ANGLE_B_MISO` | Pin 3 MISO |
| 6 | `ANGLE_1_CS_N` through `ANGLE_8_CS_N`, one per port | Pin 1 CSn, local 10 kΩ pull-up to `ANGLE_3V3` as a capture candidate |

- **ANGLE 1–4, bus A:** SCK PA5 (41), MOSI PB5 (135), MISO PA6 (42). CS in order PE9 (60), PE10 (63), PE11 (64), PE15 (68).
- **ANGLE 5–8, bus B:** SCK PD3 (117), MOSI PB15 (76), MISO PB14 (75). CS in order PD4 (118), PD7 (123), PD10 (79), PD11 (80).
- Start bring-up with SPI mode 1 at a low clock. Scope SCK and MISO at the longest run with the other three cables on that bus installed. Reserve branch series footprints, then select damping and clock from measured edges. Verify every inactive board releases MISO. Keep both front-knuckle locations identified for the steering cross-check. [AS5047P datasheet and 3.3 V circuit](https://www.infineon.com/assets/row/public/documents/30/49/infineon-as5047p-datasheet-en.pdf)

### Two IMU ports

Place `J_IMU_CG` and `J_IMU_FRONT`. Each eight-way XH carries `IMU_3V3`, `GND`, `SCK`, `MOSI`, `MISO`, `CS_N`, `DRDY`, and spare on **pins 1–8** in that order at both ends. Prefix pins 3–7 with `IMU_CG_` or `IMU_FRONT_`. Use the sensor's INT1 output for DRDY if firmware routes the data-ready event there. Set VDD and VDDIO and add local bypass per the sensor reference.

| Port prefix | SCK | MOSI | MISO | CS_N | DRDY |
| --- | --- | --- | --- | --- | --- |
| `IMU_CG_` | PC10 (111) | PC12 (113) | PC11 (112) | PG2 (87) | PC7 (97) |
| `IMU_FRONT_` | PE12 (65) | PE14 (67) | PE13 (66) | PG3 (88) | PC8 (98) |

| Place or buy | Qty | JLC / LCSC code |
| --- | --- | --- |
| B6B-XH-A(LF)(SN), angle male vertical THT header | 8 motherboard + 8 remote | `C144397` |
| S6B-XH-A-1(LF)(SN), alternate side-entry angle header | Per board where needed, substitutes for vertical | `C265069` |
| XHP-6 and SXH-001T-P0.6 female contacts | 16 housings and 96 contacts for eight angle cables | `C144405`, `C140573` |
| B8B-XH-A(LF)(SN), IMU male vertical THT header | 2 motherboard + 2 remote | `C157972` |
| XHP-8 and SXH-001T-P0.6 female contacts | 4 housings and 32 contacts if both IMU spares are wired | `C144407`, `C140573` |
| AS5047P-ATSM and ISM330DHCXTR | 8 angle and 2 IMU ICs, on remote boards | `C962063`, `C2655101` |
| CS pull-ups, 100 nF local bypass, damping footprints | Per remote board and signal branch as above | `C25804`, `C14663`, `C23140` (33 Ω tuning candidate) |
| TPS7A2033PDBVR quiet 3.3 V supplies, conditional | Up to 2. Feed from a rail with dropout headroom. Verify current budget and use TI's input/output bypass. Count once across sheets 01 and 11. | `C2862740` |

The JST crimps listed fit **22–28 AWG** within JST's insulation-diameter range. XH uses a friction lock. Add harness strain relief and verify retention on the car. The six-way angle pinout uses every contact, so record the exception to ARC's spare-wire convention. If a spare is required for production, change both ends together and choose a keying scheme that prevents a sensor cable from mating with a drive port. [ARC sensor ADR](../../systems/adr/0045-vehicle-state-sensor-boards.md) · [IMU datasheet](https://www.st.com/resource/en/datasheet/ism330dhcx.pdf) · [LDO circuit](https://www.ti.com/lit/ds/symlink/tps7a20.pdf#page=30)

## 12 — ToF ports

Place `J_TOF_1` through `J_TOF_4`. Use **6-position JST XH, 2.50 mm pitch** on both boards and straight-through cables. The draft assigns **1** to `TOF_n_VIN`, **2** to `GND`, **3** to `TOF_n_SDA`, and **4** to `TOF_n_SCL`. Confirm those four positions against the remote schematic before wiring the harness. The functions for **5 and 6** are TBD. Feed `TOF_1_VIN` through `TOF_4_VIN` from `TOF_VSUPPLY` through one removable supply link per port. Check both the board's power input and its I²C pull-up rail before fitting the links. Add level translation if the measured or published logic levels fail either endpoint's input thresholds. [JST XH drawings](https://www.jst-mfg.com/product/pdf/eng/eXH.pdf) · [VL53L4CD datasheet](https://www.st.com/resource/en/datasheet/vl53l4cd.pdf)

Use **30 × 25 mm** as the remote PCB placement estimate. The 6-position header body is **17.4 mm** wide. Check the outline against the mounting holes, cable clearance and downward optical opening.

| Port | MCU peripheral | Pin 3 SDA | Pin 4 SCL |
| --- | --- | --- | --- |
| `J_TOF_1` | I2C1 | `TOF_1_SDA` PB9 (140) | `TOF_1_SCL` PB8 (139) |
| `J_TOF_2` | I2C2 | `TOF_2_SDA` PB11 (70) | `TOF_2_SCL` PB10 (69) |
| `J_TOF_3` | I2C3 | `TOF_3_SDA` PC9 (99) | `TOF_3_SCL` PA8 (100) |
| `J_TOF_4` | I2C4 | `TOF_4_SDA` PF15 (55) | `TOF_4_SCL` PF14 (54) |

Use the pull-up pair already on **each remote board**. Leave any motherboard pull-up footprints DNP for this direct configuration. All four devices can stay at 7-bit address **0x29** because the bus pairs are separate. Use polled ranging with each remote board's local XSHUT startup wiring. Confirm rise time, stuck-bus recovery and behavior on reconnect at the actual cable length. If the pin map changes, the fallback is one `TCA9548A` channel per ToF with an upstream pull-up pair, address straps, reset and bypass. A single unmultiplexed bus would need a way to control each module's XSHUT separately. [ST multi-sensor guide](https://www.st.com/resource/en/user_manual/um2931-a-guide-to-using-the-vl53l4cd-timeofflight-high-accuracy-proximity-sensor-stmicroelectronics.pdf) · [TI mux datasheet](https://www.ti.com/lit/ds/symlink/tca9548a.pdf)

| Place or buy | Qty | JLC / LCSC code |
| --- | --- | --- |
| B6B-XH-A(LF)(SN) vertical THT header | 4 motherboard + 4 remote | `C144397` |
| XHP-6 and SXH-001T-P0.6 contacts | 8 housings and up to 48 crimps for four cables, depending on the final pinout | `C144405`, `C140573` |
| Removable supply link | 1 per port. Check current and fuse/load-switch sizing before population. | `C21189` for a 0 Ω 0603 candidate within its ratings |
| VL53L4CDV0DH/1, or actual populated remote model | 4 remote ICs | `C3178291` |
| TCA9548APWR | 1 only if using the mux fallback | `C130026` |

## 13 — Camera and Orin timing

| Part or group | Qty / lookup | What to place |
| --- | --- | --- |
| Timing and carrier connectors | Topology/MPNs TBD | Reserve four trigger outputs (stereo, quad, front event, downward), one trigger spare, PPS, and signal references. v1.0 camera triggers may route directly to the camera connectors. CSI data belongs on the carrier. |
| Signal conditioning | Buffer/level translator, bias, series damping, ESD and test access TBD | Confirm direction, input voltage, polarity, pulse width and power-off behavior for each receiver. Draw PPS and the proposed shutdown request / halt handshake as distinct nets. Verify what signal actually means the Orin is safe to power down. [Carrier timing and shutdown ICD](../../systems/icd/carrier-sync.md) |

## 14 — Lighting, status and controls

| Part or group | Qty / lookup | What to place |
| --- | --- | --- |
| Lighting outputs | **3** control functions, driver/load MPNs TBD | Cross-polarized ring with DC constant-current drive, grazing LEDs, and the reserved certified Class 1 VCSEL mode with a hardware current limit. Set safe startup states and choose the drivers against the actual load. [ARC lighting design](../../systems/adr/0014-ground-speed-event-camera.md) |
| Status indicators | Count, LED MPNs and resistor values TBD | Reserve GPIOs for visible e-stop, watchdog and power-fault indications. Add current-setting resistors or drivers and a connector if the indicators are panel mounted. |
| Hotspot button and indicator | Parts/routing TBD | Confirm carrier-owned GPIO routing before assigning connectors. The dev-kit baseline uses the Orin header for these signals. [Carrier GPIO interface](../../systems/icd/carrier-sync.md) |

## Capture and review

1. Import the [EasyEDA Pro working snapshot](easyeda/ARC-Motherboard.epro) to continue the draft, or import the [14-sheet A3 template](easyeda/ARC-Motherboard-Template.epro) to start fresh. In the working snapshot, sheets 01 and 02 together correspond to sheet 01 in this guide. Start with the overview, the power budget and the STM32/PHY symbols. Search by exact MPN or C-number and check package, pad count, pin 1 and exposed pads in Footprint Manager. Record the exact BOM entry when you close each TBD.
2. Check the candidate STM32 assignments in CubeMX or the firmware pinctrl. Resolve clock, DMA, timer and EXTI ownership. Capture the UART, Ethernet and sensor pages while power and safety parts are being sized.
3. Review the power stages together. Trace battery/wall isolation, each protected rail, the hardware brake and cutoff path, the regen path when the drive switch opens, and the halt-to-compute-cut path. Assign rated connectors and returns.
4. Reconcile the netlist with the block diagram and harness pinout. Check **four drive UARTs, the chosen steering UART or PWM path, one PHY, ten sensor CS, two DRDY, four separate ToF buses, and separate power-monitor I²C**. Run EasyEDA's schematic rule check and a trial PCB conversion. Review the current fab gate against the revised UART and sensor scope before release. [EasyEDA schematic checks](https://prodocs.easyeda.com/en/schematic/design-design-rule/) · [ARC fab gate](../../systems/reviews/fab-gate.md)

**Planning estimate for the 14 sheets:** **40–60 focused hours** for capture and footprint checks, plus **12–20 hours** for review and fixes. The rail and cutoff decisions, harness pinout, and I2C5 bring-up can extend the calendar. PCB routing, firmware implementation, harness testing and SIwave are separate.
