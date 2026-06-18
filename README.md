# corne-zmk

ZMK firmware for the **Corne** split keyboard (the "ZuulBoard" layout). This is the
parent of [`zmk-temper`](https://github.com/) — the temper config is a 5-column port
of this layout. Hardware here:

- **6 columns per side** — full outer column on both halves.
- **OLED display** enabled (`CONFIG_ZMK_DISPLAY=y`).
- **ZMK Studio** enabled on the left half via the `studio-rpc-usb-uart` snippet.

Built on `nice_nano_v2` with the stock `corne_left` / `corne_right` shields. Keyboard
name: `ZuulBoard`.

## Layers

| # | Name       | Activate                           |
|---|-----------|------------------------------------|
| 0 | Linux      | base                               |
| 1 | Arrows     | hold left thumb 2 (`SPACE`)        |
| 2 | Mouse & BT | hold left thumb 3 (`TAB`)          |
| 3 | Sound      | hold left thumb 1 (`ESC`)          |
| 4 | Numbers    | hold right thumb 1 (`BACKSPACE`)   |
| 5 | Symbols    | hold right thumb 2 (`RETURN`)      |
| 6 | F          | hold right thumb 3 (`DELETE`)      |
| 7 | Gaming     | tap `&to 7` on Mouse layer (row 0) |

### Linux (base)

```
╭─────┬─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────┬─────╮
│ CAPS│  Q  │  W  │  E  │  R  │  T  │   │  Y  │  U  │  I  │  O  │  P  │  ~  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ^  │  A* │  S* │  D* │  F* │  G  │   │  H  │  J* │  K* │  L* │  ;* │  '  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ∅  │  Z  │  X  │  C  │  V  │  B  │   │  N  │  M  │  ,  │  .  │  /  │  `  │
╰─────┴─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┴─────┴─────┴─────╯
                  │ ESC │ SPC │ TAB │   │ RET │ BSP │ DEL │
                  ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

`CAPS` = caps lock, `^` = `LS(6)`, `~` = `LS(`)`, `` ` `` = grave, `'` = SQT, `∅` = `&none`.

Hold-tap on home row (`rpi`, tap-preferred, 200ms term, 125ms prior-idle):

```
A = TAP A / HOLD LCTRL      J = TAP J / HOLD LSHIFT
S = TAP S / HOLD LALT       K = TAP K / HOLD LGUI
D = TAP D / HOLD LGUI       L = TAP L / HOLD LALT
F = TAP F / HOLD LSHIFT     ; = TAP ; / HOLD LCTRL
```

GUI (Cmd/Super) sits on **D** and **K**; Ctrl sits on **A** and **;**.

Thumb layer-taps (`&lt`):

```
ESC = TAP ESC / HOLD Sound      RET = TAP RET / HOLD Symbols
SPC = TAP SPC / HOLD Arrows     BSP = TAP BSP / HOLD Numbers
TAB = TAP TAB / HOLD Mouse&BT   DEL = TAP DEL / HOLD F
```

### Arrows

```
╭─────┬─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────┬─────╮
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │  ←  │  ↓  │  ↑  │  →  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │HOME │PGDN │PGUP │ END │  ▽  │  ▽  │
╰─────┴─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┴─────┴─────┴─────╯
                  │  ▽  │  ▽  │  ▽  │   │ RET │ BSP │ DEL │
                  ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

### Mouse & BT

```
╭─────┬─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────┬─────╮
│  ▽  │ULCK │  ▽  │  ▽  │  ▽  │GAME │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│BCLR │ BT0 │ BT1 │ BT2 │ BT3 │ BT4 │   │ ←   │  ↓  │  ↑  │  →  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
╰─────┴─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┴─────┴─────┴─────╯
                  │  ▽  │  ▽  │  ▽  │   │ MB2 │ MB1 │ MB3 │
                  ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

`GAME` = `&to 7`. `BCLR` = `&bt BT_CLR`. `BT0–BT4` = `&bt BT_SEL 0–4`. The right home
row is mouse movement (`&mmv`), the right thumbs are mouse buttons (`&mkp`). `ULCK` =
`&studio_unlock` — hold left `TAB` then press it to unlock ZMK Studio for live keymap
edits (left half only).

### Sound

```
╭─────┬─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────┬─────╮
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │NXWP │VOL+ │NEXT │PVWP │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │CNXW │VOL- │PREV │CPVW │  ▽  │  ▽  │
╰─────┴─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┴─────┴─────┴─────╯
                  │  ▽  │  ▽  │  ▽  │   │STOP │PLAY │MUTE │
                  ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

`NXWP`/`PVWP` = next/prev workspace macros. `CNXW`/`CPVW` = move-window-to + switch
workspace macros (see [Macros](#macros)).

### Numbers

```
╭─────┬─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────┬─────╮
│  ▽  │  [  │  7  │  8  │  9  │  ]  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ▽  │  ;  │  4  │  5  │  6  │  =  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ▽  │  `  │  1  │  2  │  3  │  \  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
╰─────┴─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┴─────┴─────┴─────╯
                  │  .  │  0  │  -  │   │  ▽  │  ▽  │  ▽  │
                  ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

### Symbols

```
╭─────┬─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────┬─────╮
│  ▽  │  {  │  &  │  *  │  .  │  }  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ▽  │  :  │  $  │  %  │  ^  │  +  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ▽  │  ~  │  !  │  @  │  #  │  |  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
╰─────┴─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┴─────┴─────┴─────╯
                  │  (  │  )  │  _  │   │  ▽  │  ▽  │  ▽  │
                  ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

### F

```
╭─────┬─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────┬─────╮
│  ▽  │ F12 │ F7  │ F8  │ F9  │PSCR │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ▽  │ F11 │ F4  │ F5  │ F6  │  ▽  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ▽  │ F10 │ F1  │ F2  │ F3  │  ▽  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
╰─────┴─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┴─────┴─────┴─────╯
                  │ ESC │ SPC │ TAB │   │  ▽  │  ▽  │  ▽  │
                  ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

### Gaming

```
╭─────┬─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────┬─────╮
│ ALT │  Q  │  W  │  E  │  R  │  T  │   │  Y  │  U  │  I  │  O  │  P  │EXIT │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│SHFT │  A  │  S  │  D  │  F  │  G  │   │  H  │  J  │  K  │  L  │  ;  │  ∅  │
├─────┼─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┼─────┤
│CTRL │  Z  │  X  │  C  │  V  │  B  │   │  N  │  M  │  ,  │  .  │  /  │  ∅  │
╰─────┴─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┴─────┴─────┴─────╯
                  │ ESC │ SPC │ TAB │   │ RET │ BSP │ DEL │
                  ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

Plain keys, no home-row mods. The outer column carries `ALT`/`SHFT`/`CTRL` and an
`EXIT` key (`&to (-1)`, row 0 far right) to return to the previous layer.

## Macros

| Name         | Action                                        |
|--------------|-----------------------------------------------|
| `next_wp`    | Hold LGUI, tap LEFT — next workspace          |
| `prev_wp`    | Hold LGUI, tap RIGHT — prev workspace         |
| `ch_next_wp` | Hold LGUI+LSHIFT, tap LEFT — move ws + switch |
| `ch_prev_wp` | Hold LGUI+LSHIFT, tap RIGHT — move ws + switch |

## Custom behaviors

| Name  | Type      | Use                                                |
|-------|-----------|----------------------------------------------------|
| `rpi` | hold-tap  | Home-row mods; require-prior-idle 125ms            |
| `mpi` | hold-tap  | Mac-style hold-tap with `&trans` tap fallback      |

## Build & flash

CI builds both halves on push (see `.github/workflows/build.yml`):

1. Push to GitHub → Actions runs the matrix in `build.yaml`.
2. Download the `firmware.zip` artifact — contains
   `corne_left-nice_nano_v2-zmk.uf2` and `corne_right-nice_nano_v2-zmk.uf2`.
3. Double-tap reset on each half → mounts as a USB drive → drag the matching
   `.uf2` over.
4. Pair via Bluetooth: hold left `TAB` (Mouse & BT layer) + tap `&bt BT_SEL 0..4`.
