# Heltec_LoRa32 — KiCad 9 library

Symbols, footprints and 3D models for the Heltec WiFi LoRa 32 **V3 / V3.2**
and **V4**, for carrier boards the module plugs or solders into (OLED side
up).

| File | Contents |
|---|---|
| `Heltec_LoRa32.kicad_sym` | `Heltec_WiFi_LoRa32_V3`, `Heltec_WiFi_LoRa32_V4` |
| `Heltec_LoRa32.pretty/` | matching footprints (symbol Footprint fields are pre-filled) |
| `Heltec_LoRa32.3dshapes/` | Heltec's own STEP exports for each variant |
| `WiFi_LoRa_32_V4.2.0_datasheet.pdf` | source for pinout and dimensions |

## Install

1. Clone this repo. Keep `Heltec_LoRa32.pretty/` and `Heltec_LoRa32.3dshapes/`
   as siblings — the footprints reference their 3D model with a relative
   path (`../Heltec_LoRa32.3dshapes/<name>.step`), which KiCad resolves
   against the footprint library's own folder. No per-machine path variable
   needed; this is what makes it work unmodified for anyone who clones it.
2. KiCad → Preferences → Manage Symbol Libraries → add
   `Heltec_LoRa32.kicad_sym`, nickname **`Heltec_LoRa32`**.
3. Preferences → Manage Footprint Libraries → add `Heltec_LoRa32.pretty`,
   same nickname. The nickname must match or the symbols' Footprint fields
   won't resolve.

## Pad numbering

| Pads | Meaning |
|---|---|
| 1–18 | J2 pin 1–18 (GND, 5V, Ve, Ve, RX, TX, RST, GPIO0 …) |
| 19–36 | J3 pin 1–18 (GND, 3V3, 3V3, GPIO37 …) |
| 37–40 | V4 only: GPIO15, 16, 17 (OLED SDA), 18 (OLED SCL) at the antenna end |

Footprint origin is pad 1 (J2-1, USB end). J2 is the row along the bottom
in the top view, J3 the row along the top. Every pad has a GPIO label on
F.Fab.

## Geometry and confidence

- **Header pattern (both versions)**: 2×18 at 2.54 mm, rows 22.86 mm apart.
  Taken directly from Heltec's V4 dimension drawing. V4 is declared pin- and
  form-factor-compatible with V3.
- **V4 outline**: 51.69 × 25.4 mm, with a 2.16 mm chamfer at the USB end and
  a 3.81 mm × 45° antenna nose, per the drawing.
- **V4 pads 37–40**: the drawing gives no dimensions for these, so the
  positions were measured off the drawing to about ±0.1 mm (x = 45.72,
  y = centre ±3.30 / ±5.84). Check against your board before fabbing if you
  plan to use them.
- **V3 outline**: 50.2 × 25.5 mm, drawn with approximate corners and the
  same USB-end offset as V4. The V3 footprint includes a no-copper keepout
  under the antenna end for the onboard 2.4 GHz antenna. V4 has no keepout
  because both of its antennas are on IPEX connectors.
- Courtyard includes the 1.36 mm USB-C overhang. Leave extra room for the
  plug.

## 3D models

- **V4**: Heltec's own manufacturer STEP export (© Heltec Automation).
  Offset `(-2, 11.5, 5.5)` — confirmed correct in KiCad's 3D viewer.
- **V3**: a third-party simplified mechanical-integration model (not
  Heltec's own export). Offset `(20.5, 11.5, 2)` — confirmed correct in
  KiCad's 3D viewer.

## Things the symbol encodes

- Power pins: 5V and 3V3 are `power_in` (either can feed the board), and Ve
  is `power_out` (switched 3.3 V, enabled by pulling GPIO36/VEXT_CTRL low).
  When the module is powered only over USB, put a PWR_FLAG on any 3V3/5V
  net you draw from, or ERC will complain.
- V4 on-board uses, shown in the pin names:
  - GPIO2/7/46 drive the LoRa FEM (28 dBm PA). Don't repurpose them.
  - GPIO34 and GPIO38–42 go to the GNSS connector.
- GPIO1 reads VBAT when GPIO37/ADC_CTRL is high: VBAT = V × 490/100.
- GPIO0/3/45/46 are ESP32-S3 strapping pins.

## License

The symbol and footprint definitions (`.kicad_sym`, `.pretty/`) in this repo
are original work, released under the MIT License — see `LICENSE`.

The bundled datasheet PDF and STEP 3D models are © Heltec Automation,
included here for convenience under their terms for non-commercial personal
use (see the datasheet's copyright notice). They are not covered by the MIT
license above.
