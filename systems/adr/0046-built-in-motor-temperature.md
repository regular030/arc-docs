# ADR-0046: Buy drive motors with built-in temperature sensors

- **Status:** Accepted (owner procurement requirement; exact motor remains TBD)
- **Date:** 2026-10-09
- **Deciders:** Kunshpreet Baweja, through the instruction to remove the separate motor-temperature part and buy motors with built-in sensing
- **Traces to:** SYS-01, SYS-13, SYS-19, E-28, E-50, E-51
- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-12
- **Supersedes:** ADR-0026's exact motor-part selection only. Its one-motor-per-wheel and transverse, inboard, staggered layout remain accepted.

## Context

The previous BOM bought separate can-mounted thermistors as a fallback for motors without documented temperature sensing. The owner now requires the temperature sensor to be supplied inside each drive motor. The Castle 1010 sensor cable's temperature function and curve have not been established, so the previous motor choice cannot be treated as compliant without evidence.

## Decision

Buy drive motors with **built-in temperature sensors compatible with the A50S motor-temperature input**. Remove the external motor-can thermistor purchase and attachment work; retain E-28 as a Deleted BOM row. Motor temperature remains required telemetry under SYS-19.

The exact compliant E-51 motor, supplier price, sensor type/curve and cable pinout are **TBD**. Castle 1010 remains a historical candidate pending verification, rather than an approved purchase. Retain the existing speed, supply, mechanical fit, Hall commutation and serviceability constraints; a replacement that changes those needs owner/mechanical review.

Carry forward ADR-0026's accepted arrangement: one motor per wheel, transverse, inboard and staggered fore/aft, with a two-stage gearbox and CVD to each wheel. Existing Castle-based gearing, envelope, mass and power figures remain inherited planning assumptions until the compliant motor is verified; they are not qualified replacement-motor specifications.

## Consequences and release checks

- The built-in sensor connects to that motor's A50S, alongside its Hall signals. Temperature returns through controller telemetry; it consumes no separate motherboard ADC channel or remote temperature PCB.
- Obtain the manufacturer's temperature-sensor type, electrical connection and conversion curve before buying. Confirm compatibility with the reviewed A50S input circuit and firmware configuration, not simply the presence of a sixth Hall-cable contact.
- Confirm the actual input voltage limits, cable mapping and isolation from motor phases; calibrate against a reference temperature and test configured temperature limiting and telemetry on the arrival rig.
- If a candidate has no suitable built-in sensor, choose another motor through review. Do not quietly reinstate E-28 or assume that a Hall-sensored motor includes temperature sensing.
- Motor price and any revised shaft, can, gearing or mounting implications remain open until a compliant motor is selected. The archived Castle price is not a quote for the new selection.

The [corner interface](../icd/corner-connector.md), [arrival test](../tests/a50s-arrival-and-fd.md) and [wiring plan](../bom/motherboard-wiring.md) carry this procurement constraint into the harness and acceptance checks.
