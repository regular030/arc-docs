# ADR-0047: E-stop must reach a full stop with VESC firmware halted

- **Status:** Accepted (user clarification of the requirement; braking implementation remains open)
- **Date:** 2026-10-08
- **Deciders:** Kunshpreet Baweja, through the explicit instruction that the car has to go to a full stop
- **Traces to:** SYS-04, SYS-05, SYS-25, E-32, E-34, E-41, E-50, RSK-11
- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-12
- **Supersedes:** ADR-0012
- **Amends:** ADR-0034 (e-stop guarantee; bought A50S, deck placement and speed cap retained)
- **Related:** ADR-0019 (steering behavior remains; its cut timing must be coordinated with the revised stop)

## Context

ADR-0012 permits a corner to coast when its VESC firmware hangs. ADR-0034 moved the hardware torque cut to the power board for bought A50S controllers. Neither a gate-drive cut nor motor-bus power removal supplies an independent braking path or proves that the car has stopped. The user explicitly rejected that fallback: the car has to reach a full stop.

## Decision

1. A physical e-stop must bring the **vehicle to physical standstill**, including when Orin, sync-MCU or one or all VESC firmwares are halted. Coasting until friction happens to stop the car is not an accepted braking path.
2. Preserve the controlled brake ramp in normal operation. Provide an **independent braking path** that can achieve standstill when VESC firmware cannot perform that ramp. A firmware watchdog, CAN brake command or delayed motor-bus or gate-drive cut alone does not meet this requirement.
3. Drive permission must be removed in hardware without depending on the failed firmware. The independent brake must remain effective through that transition. A halted controller may retain active PWM, so the design must address unwanted drive torque as well as the absence of braking.
4. E-stop release must not restart motion. Re-arm behavior, standstill detection and any brake-release sequence must be specified before layout freeze; normal commands cannot bypass the stop.
5. The inherited TBC 2.4 s motor-bus cut at 9 m/s (ADR-0034 / ICD-corner-connector) and TBC 5 m/s² deceleration are normal-operation design targets, not proof of a full stop. Specify the maximum stopping time / distance, acceptable motion-measurement resolution and standstill observation interval, then verify them on the actual floor, at the measured mass and speed.

**Implementation is not selected by this ADR.** The electrical and mechanical owners must select and size E-34. Candidates include a spring-applied friction brake rated for dynamic emergency stopping, or an independent motor-braking override with suitable power, current limiting and near-zero-speed behavior. A bare phase short or a small holding-only brake must not be presented as a validated solution. No particular actuator, relay, MCU or brake-driver IC is approved here.

## Consequences and verification

- E-stop-related PCB and harness design is incomplete until E-34, its interfaces and the stop sequence are defined. A timer IC remains useful for torque inhibition, but is not the stopping mechanism.
- Verify on the loaded motor bench before track testing. Inject halted firmware, retained PWM, loss of commands, broken ESTOP and loss of relevant control power. Then test progressively on the vehicle with one and all VESC firmwares halted; measure **chassis motion**, not only wheel rotation or EN_GATE.
- Record braking onset, standstill time / distance, current, thermal / bus-voltage stress and re-arm behavior. Wheel lock while the car slides is not a pass. Torque cut alone is not a pass.
- RSK-11 stays open and blocks release of the e-stop design until these checks pass. Existing regen-clamp sizing remains required for the normal electrical-braking path.

## Engineering evidence

The [TI DRV8301 datasheet (historical custom-controller reference)](https://www.ti.com/lit/ds/symlink/drv8301.pdf), section 7.4.1, distinguishes gate-driver disable from motor braking. [Kendrion's spring-applied brake documentation](https://www.kendrion.com/en/products-services/industrial-brakes/spring-applied-brakes/servo-line) illustrates power-off engagement, but each candidate still needs its own dynamic-stop energy, speed, thermal and mounting qualification. These sources support the design review; they do not establish compliance of this car.
