# Standard Library: `color`

```lua
local color_lib = require("color")
```

The `color` standard library provides utilities for color space transformations, HSV manipulation, hex string parsing, darkening, lightening, and animated rainbow generation.

---

## Construction & Conversion

### `color_lib.new`
Creates an RGBA table `{ r, g, b, a }`.

```lua
color_lib.new(r?: integer, g?: integer, b?: integer, a?: integer) -> table
```

---

### `color_lib.from_float` & `color_lib.to_float`
- `color_lib.from_float(r, g, b, a) -> table`: Creates color from normalized floats ($0.0$ to $1.0$).
- `color_lib.to_float(c) -> r, g, b, a`: Converts integer color to four normalized floats ($0.0$ to $1.0$).

---

### `color_lib.from_hsv` & `color_lib.to_hsv`
- `color_lib.from_hsv(h: number, s: number, v: number, a?: integer) -> table`: Creates RGBA color from Hue ($0$ to $360$), Saturation ($0.0$ to $1.0$), Value/Brightness ($0.0$ to $1.0$), and optional Alpha ($0$ to $255$).
- `color_lib.to_hsv(c) -> h, s, v`: Converts RGBA color table to `h, s, v`.

---

### `color_lib.from_hex` & `color_lib.to_hex`
- `color_lib.from_hex(hex_string: string) -> table`: Parses hex strings like `"#FF5500"`, `"FF5500"`, or `"FF5500FF"`.
- `color_lib.to_hex(c, include_alpha?: boolean) -> string`: Formats color as a hex string (e.g. `"#FF5500"` or `"#FF5500FF"`).

---

## Manipulation & Effects

- `color_lib.lerp(a, b, t: number) -> table`: Linearly interpolates between colors `a` and `b` by fraction `t` ($0.0$ to $1.0$).
- `color_lib.with_alpha(c, a: integer) -> table`: Returns a copy of color `c` with new alpha `a`.
- `color_lib.lighten(c, amount: number) -> table`: Lightens color towards white by fraction `amount` ($0.0$ to $1.0$).
- `color_lib.darken(c, amount: number) -> table`: Darkens color towards black by fraction `amount` ($0.0$ to $1.0$).
- `color_lib.rainbow(speed?: number, offset?: number) -> table`: Generates a real-time cycling rainbow color based on `os.clock()`.

---

## Preset Colors
- `color_lib.white`, `color_lib.black`, `color_lib.red`, `color_lib.green`, `color_lib.blue`, `color_lib.yellow`, `color_lib.cyan`, `color_lib.magenta`, `color_lib.orange`, `color_lib.purple`.
