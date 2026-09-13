# User Type: `color`

The `color` class represents 32-bit RGBA color values used throughout the render pipeline and UI widgets.

---

## Constructors

```lua
color() -> color(255, 255, 255, 255)
color(r: integer) -> color(r, 255, 255, 255)
color(r: integer, g: integer) -> color(r, g, 255, 255)
color(r: integer, g: integer, b: integer) -> color(r, g, b, 255)
color(r: integer, g: integer, b: integer, a: integer) -> color(r, g, b, a)
```
- Channel values are integers from $0$ to $255$.

---

## Fields

- `col.r`: Red channel ($0$ to $255$).
- `col.g`: Green channel ($0$ to $255$).
- `col.b`: Blue channel ($0$ to $255$).
- `col.a`: Alpha channel ($0$ to $255$).

---

## Methods

### `col:alpha_modulate` & `col:alpha_modulatef`
- `col:alpha_modulate(alpha: integer) -> color`: Returns a new color with the specified integer alpha ($0$ to $255$).
- `col:alpha_modulatef(alpha: number) -> color`: Returns a new color with the alpha multiplied by a float factor ($0.0$ to $1.0$).

---

### `col:lerp`
Linearly interpolates between this color and another color.

```lua
col:lerp(other: color, weight: number) -> color
```

---

### `col:as_int32`
Returns the 32-bit packed RGBA integer.

```lua
col:as_int32() -> integer
```

---

### `col:as_fraction`
Returns the four RGBA components as normalized float fractions ($0.0$ to $1.0$).

```lua
col:as_fraction() -> number, number, number, number
```

---

### `col:clone`
Returns a duplicate copy of the color.

```lua
col:clone() -> color
```
