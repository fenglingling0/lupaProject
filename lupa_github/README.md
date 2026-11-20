# lupa_plata

Handheld camera magnifier. ESP32-S3-WROOM-1-N16R8, OV5640 (DVP, 24-pin FPC), 4-layer, 28.2 x 67.1 mm, KiCad 9. Two-sided SMT assembly at JLCPCB.

## Architecture

- Power: USB-C -> BQ21061 (charger / power path) -> TPS63802 buck-boost -> 3V3. Hard on/off via SPDT on the TPS63802 EN. Camera rails: ME6211 (2V8), TLV75515 (1V5).
- Audio: PDM mic (MSM261) direct to GPIO, MAX98357A I2S amp. Storage: microSD over SPI. IMU: QMI8658B (INT on an RTC GPIO, wake-up).
- Battery sense: 100k/100k divider gated by TMUX1101 (IO18 enable, IO16 sense).
- Programming and logging over native USB (IO19/IO20); UART0 pins are used for I2S.
- Pin map: `pin_list.csv` / `pin_list_en.csv`.

## Stackup / layout

F.Cu / GND / 3V3 plane + slow signals / B.Cu. Vias 0.3/0.15 (JLCPCB 0.15 mm via option required), in-pad vias on the BQ21061 DSBGA (epoxy-filled, capped). Bottom side: low-profile parts only (passives up to 1206, SOT-23-5, SC70-5).

## Versions

| | v1_ploschadki_pod_payku | v2_razemy_jst |
|---|---|---|
| Battery / speaker / trigger | solder pads | JST PH 2-pin, vertical THT (B2B-PH-K-S) |
| Battery NTC | pads (J6) | removed, TS biased by R3 10k |
| Mic | MSM261D3526H1CPM (no JLCPCB stock) | MSM261DDB019 (C51928210) |
| DRC / ERC | clean | clean |

v2 is current. Differences beyond the table: D1/R24 and a few passives moved; CAM_VSYNC routed on the inner layer along the right edge.

## Status

Not built yet. DRC (with schematic parity) and ERC are clean; BOM has an LCSC number for every part.

Not verified on hardware:
- Start-up and charging with USB only (no battery).
- Placement rotation of polarized parts in the JLCPCB viewer (U1, U2, U3, X1, J2, J4, MK1).
- IO16 is ADC2: unusable while Wi-Fi is active.
- JST PH pin 1 = BAT+; battery pack polarity varies by vendor.
