# Builds

The v1 car is built twice (ADR-0046). **v1.1 is the full design** in these files. **v1.0** is built first, on compute and a LiDAR on loan from the team's existing car, with fewer cameras. This page tracks what each build contains and where its order stands; the decision and its reasons are in ADR-0046.

## What differs

| Item | v1.0 | v1.1 |
| --- | --- | --- |
| E-01 compute | Orin Nano 8 GB developer kit module, on loan | Orin NX 16 GB |
| E-02 carrier | Developer kit carrier (P3768) | Antmicro fork |
| E-03 cooling | Developer kit cooler | NX cooler |
| E-04 SSD | 1 TB | 4 TB |
| E-10 LiDAR | Hokuyo UST-20LX, on loan | Hokuyo UST-10LX |
| E-12 side/rear cameras | Not fitted | Quad OV9281 kit |
| E-13 forward event camera | Not fitted | GenX320 |
| E-25 ground-speed camera | Arducam B0223 frame camera | GenX320 |
| E-29 ground lighting | LEDs | LEDs + Class 1 laser module |

Every other BOM row is in both builds. Rows that differ carry a "v1.0" note in [bom/electrical.md](bom/electrical.md), and [budgets/cost.csv](budgets/cost.csv) marks each line `both`, `v1.0` or `v1.1`.

## What v1.0 can't do

| Limit | Revisit |
| --- | --- |
| LIM-09: no learned policy alongside the platform stack (8 GB) | v1.1 |
| LIM-10: three cameras, no spare CSI port | v1.1 |
| LIM-11: frame-camera ground speed with LEDs, unproven above low speed | v1.1 |

## Built in v1.0 for v1.1

- The motherboard is the v1.1 board: four camera trigger groups plus a spare, every sensor connector and chip select, and a compute rail sized for the NX.
- The compute bay fits the dev kit and the Antmicro assembly (ADR-0043), with room for the NX cooler.
- Mounts for the quad and forward cameras are in the chassis from the start.

## Orders

| Build | Funding request | Ordered | Arrived |
| --- | --- | --- | --- |
| v1.0 | TBD | No | — |
| v1.1 | TBD | No | — |

When a build's design is frozen for ordering, tag it `car-v1.0` or `car-v1.1` (see [README](README.md), "Releases are tags").
