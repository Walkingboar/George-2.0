# Retro PC Case + Macropad Project

Two projects in one folder: a **macropad PCB** (KiCad) and a **retro 3D-printed PC case** (Onshape / slicer).
Everything here was generated from text and **has not been tested** in KiCad, Onshape or a printer. Open, check, and fix before you order or print anything.

All sizes are in millimetres.

---

## 1. Macropad (RP2040 Pico)

A 4-key macropad with a rotary encoder, a 1.9" ST7789 screen and a WS2812 LED strip.

### Files
| File | What it is |
|---|---|
| `macropad.kicad_pro` / `.kicad_sch` / `.kicad_pcb` | KiCad project, schematic and board. Keep all three in one folder with the same name. |
| `build_macropad_pcb.py` | Script that builds the board from KiCad's own footprint libraries (alternative to the `.kicad_pcb`). |
| `code.py` | CircuitPython firmware that matches the pin map below. |

### Pin map (PCB and firmware must agree)
| Function | GPIO | Pico pin |
|---|---|---|
| Keys 1-4 | GP2, GP3, GP4, GP5 | 4, 5, 6, 7 |
| Encoder A / B / button | GP6 / GP7 / GP8 | 9, 10, 11 |
| LED data (via 330 ohm) | GP9 | 12 |
| Display CS / SCK / MOSI | GP17 / GP18 / GP19 | 22, 24, 25 |
| Display DC / RST / BL | GP20 / GP21 / GP22 | 26, 27, 29 |
| 3V3 / VBUS | | 36 / 40 |

### To finish the board
1. In the PCB editor run **Tools > Update Footprints from Library** and fix any "not found" footprints.
   - Pico: `RaspberryPi_Pico_Common_THT` (header-mounted).
   - Hotswap sockets: install the **Keyswitch Kicad Library** (or add its `.pretty` folder under Preferences > Manage Footprint Libraries), then pick the **Kailh MX 1.00u** hotswap footprint.
2. Add a GND zone, press **B** to fill, route traces with **X**, run **Inspect > Design Rules Checker**.
3. Check the display header pin order against your module before ordering.

### Firmware
Copy `code.py` to the Pico running CircuitPython 9.x. Libraries needed in `lib/`: `adafruit_st7789`, `adafruit_display_text`, `adafruit_hid`, `neopixel`.
If the screen image is shifted, change the `LCD_` settings at the top of the file.

---

## 2. Retro PC case

A 3D-printed ATX tower with a 360 mm AIO radiator at the top, a PSU deck, a back chamber for drives, and a front touch screen.

### Current design (v4)
- **Size:** 268 wide x 440 deep x 500 tall (+12 mm feet).
- **Motherboard:** ATX (305 x 244), also Micro-ATX and Mini-ITX once the hole pattern is added.
- **Cooling:** 2 x 120 mm front intake, 1 x 120 mm rear exhaust, 3 x 120 mm top (360 mm AIO).
- **Side panel:** printed plastic (no glass).
- **Screen:** 3.5" ILI9488 SPI touch display driven by an RP2040-Zero over USB-C, in its own small box that bolts to the front panel.
- **Walls 6 mm, tray 8 mm.**

### Files
| File | Use |
|---|---|
| `retro_case_v4.zip` | **The printable set.** `tiles/` = panels cut into pieces (max 198 x 153 mm), `parts/` = shelves, feet, screen box, and its own README. |
| `retro_case_v4_assembly_preview.stl` | Whole case assembled, for viewing only. Do not print. |
| `retro_case_v3_editable.step` | Editable solid-body copy of **v3** (older, smaller design) for Onshape. |

Older files (`retro_case_blockout.stl`, `retro_case_v2.stl`, `retro_case_v3*.stl`, `retro_case_front_panel*.stl`, `retro_case_glass_*.stl`, `side_panel_ascii.stl`, `screen_box_*.stl`) are earlier versions and can be ignored. The old screen box was for a 2.8" screen and no longer fits.

### How the tiles go together
Neighbouring tiles overlap 10 mm with a half-thickness step (half-lap). Glue the overlap (PETG: epoxy or super glue). Each tile is exported flat with its outside face on the bed, so no supports are needed. Names are `<panel>_<column>-<row>.stl`.

### Do NOT print yet
- **`tray_*`** - no motherboard screw posts. The exact ATX / Micro-ATX hole positions are not confirmed.
- **`back_*`** - the I/O opening and expansion-slot opening are placeholders.

### Material
Use PETG or ASA. PLA softens near 55-60 C and can sag.

### Hardware
- 8 x M3 heat-set inserts (4 in the screen box posts, 4 in the front panel, pressed in from the back).
- 8 x M3 x 8 mm screws (4 for the screen box lid, 4 for the ears).
- Radiator and fan screws come with the AIO.
- Zip ties for the loops in the tray and deck.

### Parts to buy
- **Screen:** 3.5" ILI9488 SPI touch module (about 98 x 56 mm board, assumed).
- **Microcontroller:** RP2040-Zero (USB-C).
- **Power button:** momentary push button with 2-pin lead (16, 19 or 22 mm).
- **Front USB:** USB 3.0 header cable and/or USB-C Type-E cable (only if the motherboard has a Type-E header).

---

## Open items

1. **Motherboard screw hole positions** for the tray (ATX + Micro-ATX).
2. **I/O and expansion-slot cutouts** on the back wall.
3. **Power button and front USB** hole sizes and positions on the front panel.
4. **Right wall** needs fasteners so the drive chamber can be opened.
5. **Screen board size** - measure the real board and adjust the box.
6. **Printer bed size** - tiles assume 220 x 220 mm.
7. **Confirm** motherboard model, PSU type (ATX or SFX) and fan size (120 or 140 mm).
8. **Screen firmware and PC program** for the temperature dashboard.

## Assumptions made
- Fans are 120 mm. The GPU is a PowerColor RX 9060 XT Reaper (about 220 mm long, 2 slots).
- The AIO radiator is 360 mm and mounts at the top.
- Heat-set inserts are M3 with about a 4.1 mm hole.
