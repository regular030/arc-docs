# Total electrical cost — one car

- **Status:** Draft estimate; final BOMs, quotes and qualification pending
- **Date:** 2026-10-13
- **Currency:** CAD totals; CAD, USD and GBP supplier prices
- **Traces to:** SYS-01, SYS-03, SYS-05, SYS-06, SYS-07, SYS-08, SYS-09, SYS-10, SYS-13, SYS-14, SYS-19, SYS-23, SYS-24, SYS-29, SYS-30, SYS-32; ADR-0034, ADR-0042, ADR-0046, ADR-0047, ADR-0044, ADR-0034, ADR-0045, ADR-0053, ADR-0054

The priced electrical estimate is **CAD $11,502.52**, including estimated Ontario HST and a 30% allowance on the PCB package only. Round to **CAD $11,600.00 for the priced KEFC scope**. The independent full-stop brake E-34, Class 1 ground-lighting module and unresolved camera-interface work are additional costs, **TBD**; this is not a complete release quote.

One car uses one motherboard, one separate Jetson carrier, ten angle boards, two IMU boards and four corner-height ToF boards (ADR-0045). All sixteen sensor boards are assembled in house. The camera suite is two AR0234 stereo cameras, four side/rear OV9281 cameras and two B0223 OV9281 MIPI cameras, forward and downward (ADR-0053). Include one UST-10LX, one 500 GB NVMe, at most two 4S packs, four installed A50S controllers and one steering controller. **No motor-controller spares are included** (ADR-0054). **All purchases use new-unit pricing.** The 19 V wall supply is already owned.

## Cost summary

Base amounts exclude tax and contingency. The PCB base comes from unrounded inputs in [jlc-pcba-cost.csv](../budgets/jlc-pcba-cost.csv), rather than its already-buffered CAD $3,000 headline. Other categories have no contingency.

| Category | Base range, CAD | Working base, CAD | PCB contingency, CAD |
| --- | --- | --- | --- |
| Existing PCB package | $1,816.53–$2,689.35 | $2,252.94 | $675.88 |
| Compute | $1,974.02 | $1,974.02 | $0.00 |
| Perception | $2,664.12–$2,706.94 | $2,685.53 | $0.00 |
| Drive and steering | $1,572.60–$1,658.23 | $1,615.42 | $0.00 |
| Power, wiring and sensor accessories | $607.93–$943.35 | $775.64 | $0.00 |
| External hardware shipping | $114.17–$285.42 | $199.79 | $0.00 |
| **Total base** | **$8,749.38–$10,257.31** | **$9,503.34** | **$675.88** |

| Funding calculation | CAD |
| --- | --- |
| Base | $9,503.34 |
| 30% of PCB package | $675.88 |
| Subtotal before tax | $10,179.22 |
| Estimated 13% Ontario HST | $1,323.30 |
| **Priced estimate** | **$11,502.52** |
| **Rounded funding request** | **$11,600.00** |

The tax-inclusive planning range is **CAD $10,502.60–$12,502.44**. HST uses the [CRA's Ontario rate](https://www.canada.ca/en/revenue-agency/services/tax/businesses/topics/gst-hst-businesses/charge-collect-which-rate/calculator.html). Treat source amounts as before tax, with no rebate assumed; include tax on the PCB reserve if spent. Actual invoice/import tax, duty and brokerage are TBC. Replace this estimate with checkout totals without taxing a tax-inclusive invoice twice.

## PCB package

The [PCB estimate](jlc-pcba-cost-estimate.md) is unchanged. Its working base is **CAD $2,252.94**, or **$2,928.82 with 30%**, before tax. It includes bare boards, core PCBA, loose sensor parts, shared in-house consumables and PCB/loose-component shipping.

| Design | Installed | Fabricated | JLC-populated | Populated in house |
| --- | --- | --- | --- | --- |
| Jetson carrier — E-02 | 1 | 5 | 2 | 0 |
| Power/sync motherboard — E-30 / E-41 / E-46 | 1 | 5 | 2 | 0 |
| AS5047P angle | 10 | 10 | 0 | 10 |
| ISM330DHCXTR IMU | 2 | 5 | 0 | 2 |
| Corner-height ToF | 4 | 5 | 0 | 4 |
| Optional separate clamp — E-43 | 1 if separate | 5 | 2 | 0 |

Factory minimums leave surplus core boards; they do not fund a second car. The optional clamp is retained in this costing scenario. Reprice if it is integrated into the motherboard. Known parts and missing schematic quantities are in [pcb-component-breakdown.md](pcb-component-breakdown.md); the complete imported carrier reference is in [jetson-carrier-reference-parts.md](jetson-carrier-reference-parts.md).

## Bought hardware

Supplier prices below are **line totals for the stated quantity**. Fixed prices are used directly; TBC ranges use their midpoints. Supplier checks were made on 2026-10-12/13. Named price references do not select an MPN where the functional BOM still says TBD. Compute totals **CAD $1,974.02 before tax / $2,230.64 with estimated HST**; its carrier is already in the PCB package.

| Item | Quantity | Supplier line price / allowance | Working CAD | Source |
| --- | --- | --- | --- | --- |
| Jetson Orin NX 16GB module — E-01 | 1 | USD $1,199.99 | $1,712.51 | [Waveshare SKU 24056](https://www.waveshare.com/jetson-orin-nx.htm?sku=24056) |
| 500 GB swappable NVMe SSD — E-04 | 1 | CAD $159.99 | $159.99 | [Kingston NV3 SNV3S/500G at Canada Computers](https://www.canadacomputers.com/en/desktop-laptop-internal-ssds/261113/kingston-nv3-500g-nvme-pcie-4x4-ssd-snv3s-500g.html) |
| MT7922 / RZ616 Wi-Fi card — E-05 | 1 | CAD $24.00 | $24.00 | [Framework Canada new RZ616 card](https://frame.work/ca/en/products/amd-rz616-wi-fi-6e?v=FRANRFFX01) |
| Wi-Fi antenna assembly / leads — E-05 | 1 dual-antenna set | CAD $19.00 | $19.00 | [Framework Canada antenna module](https://frame.work/ca/en/products/antenna-module) |
| Compute heatsink + fan — E-03 | 1 set | USD $24.90 | $35.53 | [Seeed Super-mode cooler SKU 100021109](https://www.seeedstudio.com/Aluminum-Heatsink-with-Fan-for-Jetson-Orintm-NX-Orintm-Nano-Module-in-MAXN-SUPER-p-6636.html) |
| Unmanaged ≥5-port GbE switch — E-06 | 1 | CAD $22.99 | $22.99 | [TP-Link TL-SG105 at Canada Computers](https://www.canadacomputers.com/en/unmanaged-switches/58859/tp-link-tl-sg105-soho-5-port-10-100-1000mbps-desktop-switch-tl-sg105.html) |
| Hokuyo UST-10LX LiDAR — E-10 | 1 new | USD $1,200.00 | $1,712.52 | [Acroname new UST-10LX](https://acroname.com/store/scanning-laser-rangefinder-ethernet-r359-ust-10lx) |
| Arducam AR0234 stereo kit — E-11 | 1 kit / 2 cameras | USD $259.99 | $371.03 | [B0492N listed price](https://www.arducam.com/blog/product/arducam-2-3mp2-ar0234-color-global-shutter-synchronized-stereo-camera-bundle-kit-for-nvidia-jetson-agx-orin-orin-nano-orin-nx/) |
| Forward Arducam B0223 OV9281 MIPI — E-13 | 1 | USD $51.99 | $74.19 | [Arducam B0223](https://www.arducam.com/blog/product/arducam-ov9281-mipi-1mp-monochrome-global-shutter-ir-camera-module-for-jetson-board/) |
| Downward Arducam B0223 OV9281 MIPI — E-25 | 1 | USD $51.99 | $74.19 | [Arducam B0223](https://www.arducam.com/blog/product/arducam-ov9281-mipi-1mp-monochrome-global-shutter-ir-camera-module-for-jetson-board/) |
| Arducam quad OV9281 kit — E-12 | 1 kit / 4 cameras | USD $244.99 | $349.63 | [B0267 listed price](https://www.arducam.com/blog/product/arducam-1mp4-quadrascopic-camera-bundle-kit-for-raspberry-pi-nvidia-jetson-nano-xavier-nx-four-ov9281-global-shutter-monochrome-camera-modules-and-camarray-camera-hat/) |
| Ground LED lighting candidates — E-29 | 1 pack / 25 LEDs; fitted count TBC | USD $6.95 | $9.92 | [Adafruit 754 white LEDs](https://www.adafruit.com/product/754) |
| Visible-light polarizer film — E-29 | 2 sheets | USD $35.90 | $51.23 | [Edmund Optics 86-178](https://www.edmundoptics.com/p/50-x-50mm-018mm-thickness-polarizing-laminated-film/28636/) |
| Ground optics / exposure-driver accessories — E-29 | 1 set, TBC | USD $15.00–$45.00 (TBC allowance) | $42.81 | [Arducam M12 lens guidance](https://docs.arducam.com/Optics-and-Lenses/Types-of-Lenses/M12-Lens/); [Adafruit 6354 LED current-driver candidate](https://www.adafruit.com/product/6354) |
| Drive controller: A50S V2.3c 12S + fitted heatsink — E-50 | 4 installed, one per wheel | GBP £400 | $755.16 | [Triforce](https://teamtriforceuk.com/a50s-v2/) |
| Castle 1010-4400 kV drive-motor pricing candidate — E-51 | 4 | USD $399.80 | $570.55 | [GalliMods Castle 060-0098-00](https://www.gallimods.com/shop/c/p/114-1010-4400KV-Sensored-Brushless-Motor-18-Shaft-x84282061.htm) |
| Steering electronics: moteus-c1 controller PCB — E-31 | 1, part of one steering actuator | USD $69.00 | $98.47 | [mjbots USD $69 listing](https://mjbots.com/products/moteus-c1) |
| Steering motor: mjbots mj5208 pricing candidate — E-31 | 1, paired with the controller above | USD $74.00 | $105.61 | [mjbots mj5208](https://mjbots.com/products/mj5208) |
| Steering belt / two pulleys / actuator mount — E-31 | 1 set, TBC | USD $30.00–$90.00 (TBC allowance) | $85.63 | [SDP/SI timing belts and pulleys](https://www.sdp-si.com/products/Timing-Belt-Pulleys/index.php) |
| Gens ace 5300 mAh 4S hardcase pack candidate — E-40 | 2 packs maximum; 1 used per run | USD $191.98 | $273.97 | [Gens ace GEA534S60E5GT](https://genstattu.com/gens-ace-5300mah-4s-60c-14-8v-hardcase-g-tech-lipo-battery14-with-ec5-plug/) |
| 19 V wall supply — E-44 | 1 already owned; 0 to buy | Already owned | $0.00 | Already owned |
| Panel service Ethernet cable — E-09 | 1 | USD $4.95 | $7.06 | [Adafruit 909](https://www.adafruit.com/product/909) |
| Remaining harness / motor distribution — E-45 | 1 car set, TBC | USD $145.05–$345.05 (TBC allowance) | $349.71 | [Molex Pico-Clasp 501189 housings](https://www.molex.com/en-us/products/series-chart/501189); [Adafruit 30 AWG signal wire](https://www.adafruit.com/product/2051) |
| Standard e-stop button — E-32 | 1 | CAD $8.43 | $8.43 | [RobotShop DFRobot FIT0156](https://ca.robotshop.com/products/emergency-stop-switch) |
| Hotspot / illuminated momentary button — E-08 | 1 | USD $1.95 | $2.78 | [Adafruit 1479](https://www.adafruit.com/product/1479) |
| Loop-key anti-spark connector candidate — E-42 | 1 male/female pair | CAD $9.99 | $9.99 | [Adrenaline Hobby ADR1038 XT90-S pair](https://www.adrenalinehobby.com/products/connectors-xt90s-male-female-anti-spark) |
| External status LED stock — E-33 | 2 packs / 25 red + 25 green; fitted count TBD | USD $12.00 | $17.13 | [Adafruit 297 red LEDs](https://www.adafruit.com/product/297); [Adafruit 298 green LEDs](https://www.adafruit.com/product/298) |
| K&J D42DIA diametric magnet candidate — E-22 / E-23 / E-24 | 10 | USD $4.60 | $6.56 | [K&J Magnetics D42DIA](https://www.kjmagnetics.com/d42dia-neodymium-diametric-disc-magnet) |
| External regen dump resistors + heatsink — E-43 | 1 load assembly; resistor count TBD | CAD $75.00–$125.00 (TBC allowance) | $100.00 | [Ohmite HS100 3R F at DigiKey](https://www.digikey.ca/en/products/detail/ohmite/HS100-3R-F/5307114); [Wakefield thermal extrusion catalogue](https://wakefieldthermal.com/literature/) |
| Bought-hardware shipping beyond PCB/loose-sensor orders | One allowance across suppliers, TBC | USD $80.00–$200.00 (TBC allowance) | $199.79 | [Triforce shipping policy](https://teamtriforceuk.com/shipping-returns/); [Adafruit shipping policy](https://www.adafruit.com/shipping); [DFRobot shipping policy](https://www.dfrobot.com/shipping) |

**One steering actuator, two components:** the moteus-c1 is the controller PCB (USD $69), and the mj5208 is the separate motor candidate (USD $74). Together they cost **USD $143 / CAD $204.18 before shipping/tax**, with the belt/pulleys/mount priced separately. Both component rows belong to E-31; they do not represent two steering actuators.

**A50S price basis, checked 2026-10-13:** [Triforce](https://teamtriforceuk.com/a50s-v2/) lists **£70 + £30 fitted heatsink = £100 each**, so **four fitted controllers are £400 / CAD $755.16 before shipping/tax**. ADR-0054 removes controller spares from this purchase scope. The optional fully populated signal connector costs another £5 per controller; it is not selected in these prices. Confirm the supplied connector kit before ordering the harness. [Triforce shipping](https://teamtriforceuk.com/shipping-returns/) depends on order weight/service; its Canadian amount is TBC and is covered by the combined shipping allowance above. Estimated HST is added once in the funding calculation, rather than included in the £400 line.

The camera-kit prices include their HATs, stock lenses and supplied cables. The B0223 price includes its supplied module accessories. Board connectors are in the PCB package; external mating connectors are in the harness allowance. The panel Ethernet cable, loop-key pair, ten encoder magnets and external dump load are separate purchases. Integrated motor, controller and motherboard sensing has no extra sensor charge. LED packs leave stock for repair; the number fitted is TBD.

The [new UST-10LX](https://acroname.com/store/scanning-laser-rangefinder-ethernet-r359-ust-10lx) is **USD $1,200 / CAD $1,712.52**, before tax and Canadian shipping, checked 2026-10-13. Necessary wiring/cabling is listed as included. External-hardware shipping is budgeted once above; Canadian delivery/import charges are TBC.

### Sourcing and release checks

These are purchase references, not approval of an unresolved part or circuit. Fixed prices are new-unit listings; allowance rows are planning amounts, not vendor quotes. Check stock and Canadian delivery before placing orders.

| Item | Source / remaining check |
| --- | --- |
| Drive motor — E-51 | Castle 1010-4400 kV, **060-0098-00**, at [GalliMods](https://www.gallimods.com/shop/c/p/114-1010-4400KV-Sensored-Brushless-Motor-18-Shaft-x84282061.htm): USD $99.95 each, **only one shown in stock**. Four-unit availability/price TBC. [Castle's motor-care guidance](https://castlecreations.com/blogs/castle-news/expert-tips-caring-for-your-motor) describes internal temperature reporting on its sensored motors; it does not supply this motor's sensor curve/pin map. Confirm those against the A50S and qualify speed, current, thermal and mechanical fit under ADR-0046 before buying |
| Steering — E-31 | [mj5208](https://mjbots.com/products/mj5208): USD $74 motor candidate. Source the belt/pulleys through [SDP/SI](https://www.sdp-si.com/products/Timing-Belt-Pulleys/index.php); exact ratio, pitch, bores, belt length/width and actuator mount require mechanical review. Controller, motor and mechanism are three separate cost lines |
| Packs — E-40 | [Gens ace GEA534S60E5GT](https://genstattu.com/gens-ace-5300mah-4s-60c-14-8v-hardcase-g-tech-lipo-battery14-with-ec5-plug/): USD $95.99 each, two total. 4S non-HV, 5.3 Ah hardcase, EC5. Verify bay fit, reviewed connector adaptation, Canadian delivery and ≥10 min hard-driving runtime; nominal capacity does not prove SYS-03 |
| E-stop — E-32 | Use a **standard off-the-shelf latching button**, with [DFRobot FIT0156](https://ca.robotshop.com/products/emergency-stop-switch) as the CAD $8.43 pricing candidate. Retailer specifies twist reset; maker wording is inconsistent. Confirm delivered latch and NC contacts against the [Y090 datasheet](https://cdn.robotshop.com/media/d/dfr/rb-dfr-527/pdf/onpow-datasheet.pdf). Wire the signal contact into the motherboard e-stop circuit; it does not carry pack current or replace E-34 |
| Hotspot / status — E-08 / E-33 | [Adafruit 1479](https://www.adafruit.com/product/1479) momentary button, [297 red](https://www.adafruit.com/product/297) and [298 green](https://www.adafruit.com/product/298) LED packs. Final indication mapping, GPIO/bias circuits, resistor values and fitted count TBD |
| Loop key — E-42 | [XT90-S male/female pair, ADR1038](https://www.adrenalinehobby.com/products/connectors-xt90s-male-female-anti-spark): CAD $9.99, listed available. Loop wire/strain relief is in the harness allowance. Verify contact polarity, peak-current and anti-spark/precharge behavior; this is not a released disconnect design |
| Harness / distribution — E-09 / E-45 | [Adafruit 909](https://www.adafruit.com/product/909) panel CAT5e cable; [501189-2010 / 501193 terminals](https://www.molex.com/en-us/products/series-chart/501189) for the A50S signal interface in ICD-corner-connector. [30 AWG flexible wire](https://www.adafruit.com/product/2051) is a signal-wire candidate, not motor/battery wire. Final power-wire gauge, remote connector MPNs, lengths, contact counts, shielding and adapter/fuse schedule TBD. Use supplied kit connectors/cables before buying duplicates |
| Angle magnets — E-22 / E-23 / E-24 | [D42DIA](https://www.kjmagnetics.com/d42dia-neodymium-diametric-disc-magnet): USD $0.46 each, ten total. Diametric, Ø6.35 × 3.175 mm pricing candidate. Confirm installed AS5047P field/gap, shaft fit, runout and retention; ordinary axial disc magnets are unsuitable |
| Dump load — E-43 | [Ohmite HS100 3R F](https://www.digikey.ca/en/products/detail/ohmite/HS100-3R-F/5307114): CAD $22.98 each. [Wakefield extrusions](https://wakefieldthermal.com/literature/) provide a heatsink quotation route. Final resistance/count, pulse ratings, airflow/thermal recovery and assembly price TBD. The historical two-resistor example remains insufficient for the upper regen scenario (ADR-0044) |
| Ground optics — E-29 | [Adafruit 754](https://www.adafruit.com/product/754) LED pack and two [Edmund 86-178](https://www.edmundoptics.com/p/50-x-50mm-018mm-thickness-polarizing-laminated-film/28636/) visible-light sheets (USD $17.95 each; new clearance). Driver candidate [Adafruit 6354](https://www.adafruit.com/product/6354) is USD $4.50 within the accessory allowance; topology/current, strobe timing and irradiance remain TBC. Start with the supplied B0223 lens; any replacement [M12 optics](https://docs.arducam.com/Optics-and-Lenses/Types-of-Lenses/M12-Lens/) need image/working-distance review. The 400–700 nm films do not qualify 850 nm speckle optics |
| Class 1 speckle option — E-29 | [Brightlaser VM-0850F-025M-HL-0A0](https://www.brightlaser.com.hk/download/en/202201/VM-0850F-025M-HL-0A0.pdf) is a manufacturer Class 1 line-module quotation candidate, not a verified floor-illumination solution. Public price not found: **additional quote TBD**. Obtain certification/test evidence and review footprint/speckle, package/diffuser, hardware current limit, driver and installed classification before purchase |
| Independent full-stop brake — E-34 | [Kendrion Servo Line](https://www.kendrion.com/en/products-services/industrial-brakes/spring-applied-brakes/servo-line) is a supplier route for dynamic-stop review/RFQ. Actuator/design/count and **additional price TBD** under ADR-0047. Do not substitute a holding-only part or declare the button/torque cut a complete braking path |

Custom-PCB support-part values and quantities still require schematics. The linked [component breakdown](pcb-component-breakdown.md) covers every known MPN and the [carrier reference](jetson-carrier-reference-parts.md) covers all imported designators; sourcing a candidate does not resolve a missing circuit, pinout or qualification result.

## Sensor inventory

This table allocates costs already included above; it is not an additional subtotal.

| Measurement | Fitted quantity | Implementation | Cost allocation |
| --- | --- | --- | --- |
| Wheel angle — E-22 | 4 | AS5047P ARC boards | Ten-board angle batch |
| Suspension angle — E-23 | 4 | AS5047P ARC boards | Same angle batch |
| Front-knuckle angle — E-24 | 2 | AS5047P ARC boards | Same angle batch |
| Body/CG IMU — E-20 | 1 | ISM330DHCXTR ARC board | Two-board IMU batch |
| Front-axle IMU — E-21 | 1 | Same ISM330DHCXTR board | Same IMU batch |
| Corner height — E-26 | 4 | ARC ToF boards; VL53L4CD preferred/TBC | Four-board ToF batch |
| Forward stereo — E-11 | 2 | AR0234 kit | Bought kit |
| Side/rear views — E-12 | 4 | OV9281 quad kit | Bought kit |
| Forward mono — E-13 | 1 | B0223 OV9281 MIPI | Bought module |
| Ground velocity — E-25 | 1 | Downward B0223 OV9281 MIPI | Bought module |
| Localization — E-10 | 1 | UST-10LX | Bought LiDAR |
| Ground illumination — E-29 | 1 set | LED modes / certified Class 1 speckle option | LED/film/accessory lines; Class 1 module quote additional TBD |
| Motor commutation/temperature — E-51 | 4 sets | Built into compliant bought motors; compatibility TBC | Castle pricing candidate |
| Drive current/voltage/FET temperature — E-50 | 4 fitted controllers | A50S sensing/telemetry | Controller purchases |
| Steering angle/current — E-31 | 1 controller | moteus-c1; E-24 cross-check | Steering purchase |
| Branch/battery V/I/faults — E-41 | Channels TBC | Motherboard monitors/shunts | Motherboard |
| Per-cell voltage — E-46 | 4 cell inputs | Motherboard cell monitor | Motherboard |

The sixteen remote sensing ICs are already included in the PCB package at **CAD $95.02**. The fifth camera-scale ToF is removed under ADR-0045; scale derivation from the four corner readings is TBD. Deleted E-07, E-27 and E-28 have no purchase cost. No dead-wheel fallback hardware is ordered.

## Camera links and logging

ADR-0053 selects the B0223 pair. SYS-07 requires ≤10 µs alignment: pair frames with sync-MCU hardware-trigger timestamps and sequence numbers. SYS-08 requires perception ≥30 Hz and camera-to-output ≤50 ms; SYS-29 requires frame arrival within one frame +5 ms p99. Ground velocity still requires SYS-10 qualification on the actual floor. There is no event-camera output.

The [Orin NX datasheet](https://connecttech.com/ftp/pdf/jetson_orin_nx_datasheet.pdf) gives eight CSI lanes at up to 2.5 Gbit/s/lane, 20 Gbit/s aggregate. The [carrier plan](../../hardware/orin-carrier/README.md#four-port-csi-feasibility) uses four physical 2-lane ports; the imported CAD still needs that adaptation. The [B0223 specification](https://www.arducam.com/blog/product/arducam-ov9281-mipi-1mp-monochrome-global-shutter-ir-camera-module-for-jetson-board/) lists two lanes but also contains one-lane wording. Delivered lane mode/rate, trigger access and custom-carrier/JetPack drivers are TBC. The [stereo kit](https://www.arducam.com/blog/product/arducam-2-3mp2-ar0234-color-global-shutter-synchronized-stereo-camera-bundle-kit-for-nvidia-jetson-agx-orin-orin-nano-orin-nx/) output rate is TBD; the [quad kit](https://www.arducam.com/blog/product/arducam-1mp4-quadrascopic-camera-bundle-kit-for-raspberry-pi-nvidia-jetson-nano-xavier-nx-four-ov9281-global-shutter-monochrome-camera-modules-and-camarray-camera-hat/) specifies two 1.0-Gbit/s lanes.

`Payload = aggregate width × height × wire bits/pixel × fps`. Kit widths already include all images. The 10% protocol/burst allowance below is **TBC**, to be replaced by measurements. Modes are screening inputs, not qualified operating points. SYS-24 limits each useful-rate path to 50% of capacity.

| Mode | Payload, Gbit/s | With 10%, Gbit/s | Required host capacity / result |
| --- | --- | --- | --- |
| Stereo: 3840 × 1200 RAW10, 38 fps | 1.751040 | 1.926144 | ≥3.852288 Gbit/s total; ≥1.926144 per lane if 2 lanes. Kit mode/rate TBC |
| Quad: 5120 × 800 RAW8, 45 fps | 1.474560 | 1.622016 | 81.10% screened of 2.0 Gbit/s: fails 50% |
| Quad: 5120 × 800 RAW10, 35 fps | 1.433600 | 1.576960 | 78.85% screened: fails 50% |
| Quad: 5120 × 720 RAW10, 40 fps | 1.474560 | 1.622016 | 81.10% screened: fails 50% |
| Forward B0223: 1280 × 800 RAW10, 80 fps | 0.819200 | 0.901120 | ≥1.802240 Gbit/s total; 45.06% if two verified 1.0-Gbit/s lanes |
| Downward B0223: 640 × 400 RAW10, 240 fps | 0.614400 | 0.675840 | ≥1.351680 Gbit/s total; 33.79% if two verified 1.0-Gbit/s lanes |

The screened stereo + quad + B0223 loads total **5.125120 Gbit/s**, below 50% of the NX's theoretical aggregate capacity. This does not resolve the quad's individual link failure or establish simultaneous capture. Its highest listed load would need a supported 2-lane output at **≥1.622016 Gbit/s/lane**; a faster receiver alone cannot increase the stock transmitter rate. Interface solution and cost are TBD.

### Storage

At the stereo 38-fps, quad RAW10 40-fps and B0223 80/240-fps screening modes:

| Logging format, TBC | Camera write, MB/s | Ten minutes, GB | Minimum sustained storage for ≤50%, MB/s | 500 GB result |
| --- | --- | --- | --- | --- |
| RAW10 stored in 16-bit samples | 931.840 | 559.104 | 1,863.680 | Does not fit before OS/other logs |
| RAW10 packed losslessly to 10 bits | 582.400 | 349.440 | 1,164.800 | 150.560 GB nominal headroom before OS/other logs |

Packing preserves all pixel bits and reduces file storage, not CSI load. Implementation and CPU/copy cost are TBC. The write limits are camera-only; add twice the other log rate. The [500 GB Kingston NV3 specification](https://www.kingston.com/en/ssd/nv3-nvme-pcie-ssd) gives up to 3,000 MB/s write, which does not establish sustained speed after cache exhaustion. Test the exact drive at installed temperature and representative fill, with the learned policy, localization, perception and MPC running. Reserve actual OS/software space, metadata/stride and other logs, then prove ≥10 min capture without drops or corruption (SYS-09 / SYS-24). Lossy video or reduced useful rates are not assumed in this estimate.

## Open items

| Item | Required work / cost |
| --- | --- |
| E-34 independent full-stop brake | Supplier route above; select and qualify hardware under ADR-0047; mandatory additional price TBD |
| Camera interfaces — RSK-01 | Resolve quad bandwidth, stereo/B0223 lane modes, triggers, rails, I²C/reset/clocks and required JetPack drivers; additional interface cost TBD |
| Ground velocity — RSK-07 | Qualify 0–12 m/s optical review envelope, lens/aperture, exposure/strobe, illumination and four-ToF scale mapping; final optical cart and additional Class 1 module quote TBD |
| 500 GB storage | Qualify packed logging, available space and sustained writes against SYS-09 / SYS-24 |
| Motors / steering / dump load | Sources above; motor temperature curve/pinout, four-unit stock, actuator mechanics and regen load ratings/quotes TBC under ADR-0046 / ADR-0044 |
| Existing 19 V supply — E-44 | Check rating, polarity, connector and mains lead against low-voltage peak load; no new supply purchase |
| Imports | Actual shipping, tax, duty and brokerage TBC |

Ground equipment is separate: LiPo charger/safe charging setup (availability TBD), G-01 Flint 3/router cables, G-04 development boards, G-05 bench transceivers and missing laptop adapters. G-02 is deferred. This electrical estimate excludes general chassis/drivetrain hardware, tyres, sensor mounts/windows/shrouds, machining/printing, general fasteners, paid labour and new workshop equipment. Required steering-actuator accessories and encoder magnets are included above.

## Calculation basis

Use the [Bank of Canada 2026-10-09 planning rates](https://www.bankofcanada.ca/rates/exchange/daily-exchange-rates/): **USD 1 = CAD 1.4271; GBP 1 = CAD 1.8879**. Canadian prices stay in CAD. Add unrounded line totals, use range midpoints, add 30% of the existing PCB base only, then estimate 13% HST. Round the final amount to cents and the funding request up to CAD $100. Rounded rows can differ by a few cents from the source total. Refresh actual carts/FX before funding submission.

The existing [PCB cost document](jlc-pcba-cost-estimate.md), PCB CSV and [parts selection](pcb-parts-selection.md) are retained. The older [cost.csv](../budgets/cost.csv) contains historical whole-car allowances; this file carries the current B0223, 500 GB, new-unit market-price and owned-supply cost basis.
