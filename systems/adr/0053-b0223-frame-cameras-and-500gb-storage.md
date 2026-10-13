# ADR-0053: B0223 frame cameras replace GenX320; logs use a 500 GB NVMe

- **Status:** Accepted (parts selection; integration and performance qualification pending)
- **Date:** 2026-10-13
- **Deciders:** Kunshpreet Baweja
- **Traces to:** SYS-03, SYS-07, SYS-08, SYS-09, SYS-10, SYS-24, SYS-29, RSK-01, RSK-07, E-04, E-11, E-12, E-13, E-25, E-29
- **Supersedes:** ADR-0014
- **Amends:** ADR-0021 (camera timing), ADR-0025 (camera allocation)
- **Related:** ADR-0015, ADR-0016, ADR-0032, ADR-0052
- **Last editor:** Kunshpreet Baweja
- **Last edited:** 2026-10-13

## Context

The two GenX320 modules were the forward event camera and the downward ground-speed camera. Their provisional cost was USD $800–1,200 for the pair. The owner selected two Arducam B0223 OV9281 MIPI cameras instead and specified a cheap 500 GB NVMe. The four side/rear OV9281 cameras stay.

SYS-10 still requires direct forward and sideways ground velocity. SYS-07 timing, SYS-08 perception, SYS-09 full-run logging and SYS-24 bandwidth limits remain in force. Changing the parts does not establish that those requirements are met.

## Options

| Option | Pros | Cons |
| --- | --- | --- |
| Retain GenX320 | Event output; retains the previous ground-speed approach | Higher provisional cost; buyable Jetson module and driver still unqualified |
| Two B0223 OV9281 MIPI modules | USD $51.99 each; global shutter; vendor lists external triggering and a 240 fps cropped Orin NX mode | No event output; ground optics, driver, timing and sustained logging need qualification |

## Criteria

Reduce purchase cost while retaining both camera locations and direct ground-velocity measurement. Preserve the existing timing, perception and logging requirements; record the loss of event-camera capability as a v1 limit.

## Decision

Use **two Arducam B0223 OV9281 MIPI cameras**, one forward under E-13 and one downward under E-25, with **one 500 GB swappable NVMe** under E-04. The exact SSD model is TBD.

| Port / item | Camera | Acquisition |
| --- | --- | --- |
| CSI 1 — E-11 | AR0234 stereo kit, two cameras | Sync-MCU frame trigger |
| CSI 2 — E-12 | OV9281 quad kit, four cameras | Sync-MCU frame trigger |
| CSI 3 — E-13 | B0223, forward | Sync-MCU frame trigger; 1280 × 800 at 80 fps is a TBC screening mode |
| CSI 4 — E-25 | B0223, downward | Sync-MCU frame trigger; 640 × 400 at 240 fps is a TBC screening mode |

All eight cameras produce frames. Pair each frame's sequence with its sync-MCU trigger timestamp under ADR-0021. Replace the two event-clock sync outputs with frame-trigger outputs; retain four trigger groups plus one spare. Trigger levels, pulse timing, connector contacts and the released MCU/carrier pin map remain TBD.

Retain the downward mounting space, floor window/shroud, three illumination options and dead-wheel fallback from ADR-0014. The illumination options are cross-polarized LEDs, grazing LEDs and an off-the-shelf certified IEC 60825-1 Class 1 speckle module with a hardware current limit. Frame exposure/strobe control, lens/aperture and mounting geometry are TBC; no pulse settings or optical performance are approved here. Derive ground motion from frame images; estimator implementation is TBD. Height/scale mapping uses the four corner ToFs under ADR-0052 and remains unqualified.

## Consequences

- Event-camera output is no longer available in v1 (LIM-09). Event-vision experiments require a future hardware change. Simulator backends remain governed by ADR-0032; the installed sensor inventory is eight frame cameras.
- The carrier still needs four physical CSI ports. The [B0223 vendor specification](https://www.arducam.com/blog/product/arducam-ov9281-mipi-1mp-monochrome-global-shutter-ir-camera-module-for-jetson-board/) lists two lanes but also contains one-lane wording. Confirm the delivered lane mode, driver and trigger breakout on our carrier and required JetPack before layout freeze.
- The stock quad kit still exceeds SYS-24's 50% link-use ceiling at its advertised full-resolution rates. Camera selection does not waive that limit or qualify a faster aggregator.
- At the screening modes above, storing RAW10 in 16-bit samples would use **559.104 GB per ten minutes**, before other logs: it cannot fit on a 500 GB drive. Lossless packed RAW10 would use **349.440 GB** and require **≥1,164.800 MB/s sustained camera-only storage capacity** for SYS-24, plus twice the other log rate. Packing, free space, sustained writes and concurrent compute performance are TBC, not implemented or verified.
- Qualify direct velocity/slip on the actual floor under SYS-10, ≤10 µs trigger alignment under SYS-07, and full-run capture/logging under SYS-09 / SYS-24. Reopen this choice if the selected hardware cannot meet those requirements.

The [electrical estimate](../bom/total-electrical-cost.md) prices the selected pair, current compute and one-car hardware. The sixteen in-house sensor boards and core-PCB assembly scope remain as recorded in ADR-0052.
