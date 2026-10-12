# ICD-corner-connector

- **Revision:** H (2026-10-09). Restores operating A50S UART under ADR-0049; rev G drive-FD proposal withdrawn. Motor phases and Hall/built-in-temperature wiring still run from each corner to its controller on the deck; actual contacts and motor sensor mapping remain TBD. Earlier corner-board connector revisions remain superseded.
- **Status:** Draft. Needs Molex pin numbering against Triforce's pinout layout, and sign-off from both owners.
- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-12
- **Side A:** corner assembly: motor (E-51), gearbox (owner kunshpreet)
- **Side B:** chassis harness, the motor controllers on the deck (E-50), sync board, power board e-stop circuit (owner kunshpreet)
- **Traces to:** ADR-0042, SYS-05, SYS-13, SYS-19, SYS-25, ADR-0008, ADR-0009, ADR-0012, ADR-0034, ADR-0047, ADR-0049, ADR-0046, ADR-0052, RSK-11

The wheel, suspension and front knuckle angle boards and four corner-height ToF boards do not pass through the motor interface below; they wire directly to the motherboard's sync section (ADR-0008, ADR-0052). The [wiring schedule](../bom/motherboard-wiring.md) budgets four sensor runs / 26 conductors per front corner and three runs / 19 conductors per rear corner, including spares (TBC). All sixteen remote sensor boards, including both IMUs, are assembled in house.

## At the corner boundary

Two connections per corner, both crossing the suspension.

| Item | Value |
| --- | --- |
| Motor phases | 3 leads, motor to its controller on the deck. Connector at the corner boundary `TBD` (the A50S end is an MR30) |
| Phase current | Per corner at 1 g: `budgets/power-scenarios.md` (TBC); bursts set by the controller's current limit |
| Hall / built-in temperature cable | Buy a motor with a compatible built-in temperature sensor (ADR-0046). Hall A/B/C, Hall supply, GND and temperature use six conductors only if the verified sensor shares that return (TBC); otherwise the exact compatible cable/interface is TBD. Sensor type/curve and contacts must be documented before buying. No E-28 fallback. Routed away from phase leads |
| Length | Corner to deck, `TBD` from the CAD layout; as short as the layout allows |

## Motor controller connections (on the deck)

Each A50S sends full status on its own 2 Mbit/s 8N1 UART to the sync MCU. PB10 is USART3 TX and PB11 RX; connect TX to the receiver’s RX and RX to its TX, with reviewed reference returns. Confirm actual Pico-Clasp contacts before assembly. No local FD adapter or PA7 SPI breakout remains in the operating plan. See [controller telemetry](controller-telemetry.md).

The A50S V2.3c (ADR-0034) has an XT30 for power, an MR30 for the motor, a micro-USB for VESC Tool, and a 20-pin Molex Pico-Clasp for everything else. The harness side is the 501189-2010 receptacle crimp housing: 1.00 mm pitch, dual row, inner positive lock, −40 to +105 °C, with Molex 501193 gold crimp terminals (Molex's extraction tool for them is rated 28–32 AWG, so plan thin signal wire). Source: Triforce's product page and its pinout image (`Pinout_V2.3`, read 2026-10-07).

| Connection | Net | Electrical | Notes |
| --- | --- | --- | --- |
| XT30 | Motor bus | 4S: 12.0–16.8 V; transients up to the bus clamp threshold (TBD) | From the power board's switched motor bus (ADR-0034); cut by the e-stop delay. Triforce's supplied bulk capacitor goes on this cable as close to the board as possible |
| MR30 | Motor phases A, B, C | See above | |
| Micro-USB | VESC Tool | | Configuration and firmware upload |

**Signal connector, as laid out in Triforce's pinout image** (red-marked pins are 3.3 V max). Circuit numbers, provisional: Molex's drawing for 501189-2010 numbers the mating face with the latch on top as circuit 1 top-right, circuit 2 bottom-right, odd along the latch row and even along the other, counting leftwards. Assuming Triforce's image is that face, latch on top, position 10 is circuits 1 (top) and 2 (bottom) and position 1 is circuits 19 and 20. Verify on the first unit: the four corners are all GND, so check that 5 V sits at circuit 9 and 3.3 V at circuit 10 (if they're at 11 and 12, the image is mirrored):

| Position | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Circuits (provisional): top / bottom | 19 / 20 | 17 / 18 | 15 / 16 | 13 / 14 | 11 / 12 | 9 / 10 | 7 / 8 | 5 / 6 | 3 / 4 | 1 / 2 |
| Top row | GND | Hall 3 / CS | Hall 2 / MISO | Hall 1 / SCK | **Motor temp (3.3 V max)** | 5 V out | SWCLK | SWDIO | Servo / PPM | GND |
| Bottom row | GND | **SCK / ADC1 (3.3 V max)** | **MISO / ADC2 (3.3 V max)** | TX / SCL / MOSI | RX / SDA / NSS | 3.3 V out | CAN H | CAN L | Aux power in, 12–48 V | GND |

How the car uses it:

| Net | Use |
| --- | --- |
| CAN H / CAN L | Command bus (ADR-0009), twisted pair |
| TX / RX | Operating 2 Mbit/s UART telemetry to E-30; 3.3 V logic. A50S PB10 TX → H723 RX, PB11 RX ← H723 TX; references/ground offsets and connector contacts require review (ADR-0049) |
| Servo / PPM | **ESTOP** input. Not marked 3.3 V max, so 5 V logic is acceptable per Triforce's marking (TBC with them); the controller MCU's pin (PB6 on the A50S, not the sync MCU's) is 5 V tolerant |
| Hall 1–3, Motor temp, 5 V, GND | From the motor's sensor cable. The 5 V output powers the motor's Hall sensors only; the controllers' 5 V outputs are never tied together (Triforce: "only use 1 BEC") |
| SWCLK / SWDIO | **SWD on the connector:** a bad firmware build is recovered with a probe through the harness, no soldering (RSK-20) |
| Aux power in | Optional logic supply. Feeding it from an always-on rail would keep the controllers' MCUs and telemetry alive after the e-stop cuts the motor bus: the EE's call |
| ADC1, ADC2, 3.3 V | No drive-FD allocation. Preserve UART, Hall and ESTOP; do not parallel controller supply outputs |

### ESTOP behaviour

Physical chassis standstill even with one or all A50S firmwares halted (SYS-05, ADR-0047). Normal firmware braking and the motor-bus cut remain, but E-34 independent braking and its interface / sequence are TBD. Torque cut or coasting alone is not a pass.

- **Fail-safe:** our firmware enables the MCU's internal pull-down on the ESTOP input (TBC that the board adds no pull-up), so an unplugged connector, a broken wire or an unpowered e-stop circuit reads as e-stop.
- **Stage 1, braking (firmware):** when ESTOP is low, the controller ignores commands on the command bus and ramps brake current to the target deceleration until the wheel stops.
- **Stage 2, torque cut (hardware):** the power board removes motor-bus power after T, whatever the controller firmware is doing. **T = 2.4 s (TBC; set 2026-10-08):** the stop from 9 m/s at ~5 m/s² takes ~1.9 s with its ~150 ms ramp, and T adds ~25% margin, as ADR-0012 intended. Requires confirmation on rig R2; no measured result is recorded. T must come after the stop: cutting the bus while the controllers are still braking ends the braking.
- **The motor-bus switch opens when unpowered,** so losing the e-stop circuit's power also gives the hardware cut.
- **A broken ESTOP wire to one controller** brakes that corner, which reports it in its telemetry (`ESTOP`, ADR-0035), and the sync MCU stops the car through SYS-25. It doesn't trigger the hardware cut: only the button does. Accepted as a trade-off (2026-10-07).
- **Release:** doesn't restart motion. The controllers wait for fresh commands once the motor bus is back.

## Termination

- **Command bus:** 120 Ω at the sync board and at the far end of the trunk (ADR-0009). Nothing populated on the controllers.
- **Drive UART:** point-to-point links; no CAN termination resistors on TX/RX. Signal/reference/protection design remains TBD under ADR-0042.

## Open issues

1. Confirm the provisional circuit numbers above on the first unit (5 V at circuit 9, 3.3 V at circuit 10).
2. ESTOP logic level (3.3 or 5 V; the PPM pin takes either) and driver: the power board sources it; the current per controller.
3. Whether the Aux power input is fed from an always-on rail (telemetry and logging survive the motor-bus cut).
4. Compliant built-in-temperature motor choice, sensor type/curve, Hall/temp pinout and the corner connectors; lengths from the CAD layout (ADR-0046).
5. Grounding review under proposed ADR-0042: rated return branches / collection point, no accidental parallel motor return through communication wiring, UART reference currents/logic margins and measured CAN common-mode margins. See [grounding proposal](../adr/0042-grounding-and-isolation.md); acceptance remains pending.
6. Brake ramp, target deceleration, delay T and independent-brake / drive-inhibit sequence under ADR-0047.
7. Four operating UARTs: actual PB10/PB11 contacts, H723 pin/DMA assignment, source-clock alignment, 3.3 V ground/reference margins and power-off behavior remain unqualified. See [UART readiness](../reviews/a50s-uart-readiness.md). No local SPI/FD adaptation is required under ADR-0049.
