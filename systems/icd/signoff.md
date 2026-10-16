# Interface ownership and review record

- **Status:** Draft. Ownership recorded; technical approval pending.
- **Updated:** 2026-10-09

kunshpreet identified themselves as owner for power, sync, carrier, firmware/Orin and mechanical/harness in this task. The same person therefore appears in both side roles where applicable. Ownership is recorded; **no technical sign-off is inferred from that assignment**. Review the specific revision / PR and record approval in each role. A safety change still needs a separate second reviewer under the repo rules.

This is the review package for [#102](https://github.com/Autonomous-Robotics-Carleton/arc-monorepo/issues/102). Existing outlines and interfaces still listed TBD in the [index](README.md) are not made complete by assigning an owner. Mechanical-outline ownership is recorded here; the outline files retain their existing TBD headers until their own review.

| Interface / source | Side A owner / role | Side B owner / role | Current scope / open release item | Side A approval | Side B approval |
| --- | --- | --- | --- | --- | --- |
| [corner-connector](corner-connector.md) | kunshpreet — corner/mechanical | kunshpreet — harness/power/sync | Rev H; UART contacts, ground and stop interfaces | Pending | Pending |
| [corner-mechanical](corner-mechanical.md) | kunshpreet — corner/mechanical | kunshpreet — chassis | Outline; CAD datums / fasteners / loads TBD | Pending | Pending |
| [can-command](can-command.dbc) | kunshpreet — sync firmware | kunshpreet — drive firmware | Schema draft; rates, firmware behavior and actual hardware qualification | Pending | Pending |
| [controller-telemetry](controller-telemetry.md) | kunshpreet — drive firmware | kunshpreet — sync firmware | Rev A active 2 Mbit/s UART; implementation, clock alignment and harness qualification pending | Pending | Pending |
| [power-rails](power-rails.md) | kunshpreet — power | kunshpreet — load-board integration | Rev A draft; ratings, contacts, stop / inrush / shutdown hardware | Pending | Pending |
| [power-sync-stack](power-sync-stack.md) | kunshpreet — power/monitors | kunshpreet — sync hardware/firmware | Rev B draft; internal motherboard signals, monitor transport / levels, clocks and GPIOs; ADR-0044 | Pending | Pending |
| [carrier-sync](carrier-sync.md) | kunshpreet — carrier/platform | kunshpreet — sync hardware/firmware | Rev B draft; B0223 frame triggers (ADR-0053), carrier/module map and handshake evidence | Pending | Pending |
| [sync-link](sync-link.md) / [schema](sync-link.xml) | kunshpreet — sync firmware | kunshpreet — Orin backend | Draft; message review, ports/addresses/rates and UART forwarding | Pending | Pending |
| ros2-msgs | kunshpreet — backend/Orin firmware | kunshpreet — ROS stack | ICD still TBD; source-message review needed | Pending | Pending |
| steering-bus | kunshpreet — sync firmware/hardware | kunshpreet — steering integration | ICD still TBD; steering FD rate / timeout protocol / pins | Pending | Pending |
| operator-link | kunshpreet — Orin/link integration | Operator-side owner TBD | ICD still TBD; operator-side owner not assigned by the role answer | Pending | Pending |
| battery-pack | kunshpreet — power/mechanical | Charging-station owner TBD | ICD still TBD; pack / connectors and station-side owner | Pending | Pending |
| [deck-grid](deck-grid.md) | kunshpreet — chassis/mechanical | kunshpreet — mount/harness integration | Outline; CAD grid / loads TBD | Pending | Pending |

## Review procedure

1. Link the exact PR / revision, schematic or contact drawing and applicable test evidence. Draft logical nets with physical contacts TBD cannot release the harness or PCB.
2. Review each side's ratings, voltage domains, directions, reference/return paths, safe startup / power-off behavior, timing, failure detection and recovery.
3. Record reviewer, date, accepted revision and remaining limitations in both side-role columns. Keep approvals **Pending** until explicitly given for the concrete revision.
4. Record separate safety reviewer / test approval for stop, watchdog, envelope or shutdown changes. That reviewer remains **TBD** for this package.

#102 cannot close yet: the three missing electrical documents now have drafts and the named roles have owners, but final values / existing outlines, operator and charging-station ownership, hardware evidence and approvals remain open.
