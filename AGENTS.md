# AGENTS.md — Corne ZMK config

ZMK firmware for the **Corne** split keyboard (nice_nano_v2), the "ZuulBoard" layout.
The temper config (`zmk-temper`) is a 5-column port of this repo. The only files an
agent normally needs to touch are:

| File | Purpose |
|------|---------|
| `config/corne.keymap` | All layers, macros, and behaviors |
| `config/corne.conf` | Kconfig flags (display, sleep, pointing, Studio) |
| `build.yaml` | CI build matrix (board/shield pairs) |
| `config/west.yml` | ZMK revision (`main`) and manifest |
| `README.md` | Layer diagrams — keep in sync with keymap changes |

## Hardware facts

- **6 columns per side** — full outer column on both halves.
- **42 keys total**: 3 rows × 12 cols + 6 thumb keys.
- **OLED present** (`CONFIG_ZMK_DISPLAY=y`).
- **ZMK Studio** enabled on the left half via the `studio-rpc-usb-uart` snippet.
- Keyboard name: `ZuulBoard`.
- Stock `corne_left` / `corne_right` shields from zmk (no external shield module).

## Keymap layout (index reference)

Positions are **0-indexed**, left-to-right, top-to-bottom:

```
Row 0:  cols 0–11   (top row)
Row 1:  cols 0–11   (home row)
Row 2:  cols 0–11   (bottom row)
Thumb:  6 keys — left thumb keys first (under cols 3–5), then right thumb (under cols 6–8)
```

Split visualisation (col 0 = far left, col 11 = far right):

```
╭col0─col1─col2─col3─col4─col5╮   ╭col6─col7─col8─col9─col10col11╮
│  row 0                       │   │                              │
│  row 1  (home)               │   │                              │
│  row 2                       │   │                              │
╰─────────┬────┬────┬────╮     │   │   ╭────┬────┬────┬───────────╯
          │thmb│thmb│thmb│     │   │   │thmb│thmb│thmb│
          ╰────┴────┴────╯     │   │   ╰────┴────┴────╯
```

Thumb layer-taps (base layer), in source order:

```
left thumb 0 = ESC  / hold → layer 3 (Sound)
left thumb 1 = SPC  / hold → layer 1 (Arrows)
left thumb 2 = TAB  / hold → layer 2 (Mouse & BT)
right thumb 0 = RET  / hold → layer 5 (Symbols)
right thumb 1 = BSPC / hold → layer 4 (Numbers)
right thumb 2 = DEL  / hold → layer 6 (F-keys)
```

## Home-row mods (Linux base, row 1)

`rpi` hold-tap (tap-preferred, 200ms term, 125ms prior-idle):

```
A = TAP A / HOLD LCTRL      J = TAP J / HOLD LSHIFT
S = TAP S / HOLD LALT       K = TAP K / HOLD LGUI
D = TAP D / HOLD LGUI       L = TAP L / HOLD LALT
F = TAP F / HOLD LSHIFT     ; = TAP ; / HOLD LCTRL
```

GUI (Cmd/Super) is on **D** and **K**; Ctrl is on **A** and **;**.

## Layer summary

| Layer | Name       | Activate              |
|-------|------------|-----------------------|
| 0     | Linux      | base                  |
| 1     | Arrows     | hold SPC              |
| 2     | Mouse & BT | hold TAB              |
| 3     | Sound      | hold ESC              |
| 4     | Numbers    | hold BSPC             |
| 5     | Symbols    | hold RET              |
| 6     | F-keys     | hold DEL              |
| 7     | Gaming     | `&to 7` on Mouse & BT |

The outer columns (col 0, col 11) carry: `CAPS`/`LS(6)`/`&none` on the left,
`LS(`)`/`'`/`` ` `` on the right (Linux layer). The Gaming layer keeps its outer
column too — `LALT`/`LSHIFT`/`LCTRL` on col 0 and a `&to (-1)` exit at row 0 col 11,
so it is **not** sticky (unlike the temper port, which dropped that column).

## Common ZMK keycode patterns used here

- `&kp KEY` — standard key press
- `&lt LAYER KEY` — layer-tap (hold = layer, tap = key)
- `&rpi MOD KEY` — hold-tap with require-prior-idle (home-row mods)
- `&bt BT_SEL N` — select Bluetooth profile N
- `&bt BT_CLR` — clear current Bluetooth bond (Mouse & BT layer, row 1 col 0)
- `&mmv MOVE_*` — mouse movement
- `&mkp MB*` — mouse button
- `&to N` — switch to layer N permanently
- `&studio_unlock` — unlock ZMK Studio editing

## Making keymap changes

1. Edit `config/corne.keymap`.
2. Match the ASCII diagram style in `README.md` — update the relevant layer
   diagram and any prose notes in the same commit.
3. Column indices start at 0 (top-left). Rows start at 0 (top). Each row has 12
   bindings. Thumb keys follow row 2 in source order (left-thumb keys first, then
   right-thumb keys).
4. Both halves share one keymap file; the corne shields handle split routing.

## Build & CI

Firmware is built by GitHub Actions on every push:

- Left half: `corne_left` + `studio-rpc-usb-uart` snippet (ZMK Studio).
- Right half: `corne_right`.

The workflow delegates entirely to
`zmkfirmware/zmk/.github/workflows/build-user-config.yml@main`. No local build
toolchain is needed — push and download the `firmware.zip` artifact.

## Flashing

1. Double-tap reset → half mounts as a USB drive.
2. Drag the matching `.uf2` from the `firmware.zip` artifact.
3. Pair: hold TAB (Mouse & BT layer) + tap `BT_SEL 0–4`.
4. Clear a bond: hold TAB + press bottom-left of the BT row (`BT_CLR`, row 1 col 0).
