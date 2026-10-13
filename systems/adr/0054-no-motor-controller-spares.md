# ADR-0054: Exclude motor-controller spares from one-car procurement

- **Status:** Accepted (owner procurement scope)
- **Date:** 2026-10-13
- **Deciders:** Kunshpreet Baweja
- **Traces to:** E-31, E-50, SYS-13, ADR-0034, ADR-0052
- **Amends:** ADR-0034 (controller purchase quantity only)
- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-13

## Context

The one-car funding estimate included four installed A50S controllers and two purchased spares. The owner instructed that motor-controller spares be excluded.

## Decision

Fund and buy **four A50S V2.3c controllers with fitted heatsinks**, one per drive motor, and **one moteus-c1 steering controller**. Include no purchased motor-controller spares. Retain the selected controllers, drive layout, UART telemetry and braking requirements.

This replaces ADR-0034's spare-controller purchase scope. Its accepted decision body remains the historical record. Core-PCB factory minimums under ADR-0052 remain separate manufacturing charges; they are not purchased motor-controller spares.

## Consequences

Remove the spare-controller cost line from the [total electrical estimate](../bom/total-electrical-cost.md), and align E-50 and the electrical handoff with four purchased drive controllers. Replacement controllers require a later purchase if one fails; no replacement stock is funded by this estimate.
