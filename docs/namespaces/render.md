# Namespace: `render`

The `render` namespace provides high-performance Direct3D rendering primitives, text drawing, font management, image loading, weapon SVG icons, and 3D geometric shapes.

---

## Screen & Camera

### `render.screen_size`
Returns the current screen resolution as a `vector` (where `x` = width, `y` = height).

```lua
render.screen_size() -> vector
```

---

### `render.camera_angles`
Gets or sets the player's view angles through the engine client.

```lua
render.camera_angles(new_angles?: qangle) -> qangle
```

---

### `render.camera_position`
Returns the current camera position in world space as a `vector`.

```lua
render.camera_position() -> vector
```

---

### `render.world_to_screen`
Projects a 3D world space coordinate into 2D screen pixels.

```lua
render.world_to_screen(world_pos: vector) -> vector
```
- Returns a `vector` where `x` and `y` are pixel coordinates. If offscreen, returns coordinates outside screen bounds or $(0, 0)$.

---

## Fonts & Text

### Built-in Font IDs
Instead of creating custom fonts, you can pass integer font IDs directly to `render.text` and `render.measure_text`:
- `1`: Standard `Verdana` (12px)
- `2`: `SmallFont` (9px pixel font)
- `3`: `VerdanaBold` (12px bold)
- `4`: `CalibriBold` (14px bold)

---

### `render.load_font`
Creates and registers a custom TrueType font handle.

```lua
render.load_font(family_name: string, size: integer, flags: string) -> font_handle
```

#### Flags
- `"b"`: Bold font weight (weight 600 instead of 400).
- `"a"`: Anti-aliased font rendering (`CLEARTYPE_NATURAL_QUALITY`).

#### Example
```lua
local my_font = render.load_font("Segoe UI", 16, "ba")
```

---

### `render.add_font`
Loads a custom TTF font from raw binary memory string (`AddFontMemResourceEx`).

```lua
render.add_font(mem_data: string) -> void
```

---

### `render.measure_text`
Measures the width and height of a text string when rendered with a given font.

```lua
render.measure_text(font: integer | font_handle, text: string) -> vector
```
- Returns a `vector` where `x` is width in pixels and `y` is height in pixels.

---

### `render.text`
Renders formatted text onto the screen.

```lua
render.text(font: integer | font_handle, pos: vector, color: color, flags: string, text: string) -> void
```

#### Text Flags
- `"c"`: Centered horizontally (`TEXT_CENTERED`).
- `"o"`: Outlined with black border (`TEXT_OUTLINED`).
- `"d"`: Rendered with dropshadow (`TEXT_DROPSHADOW`).
- `""`: Default left-aligned flat text.

#### Example
```lua
render.text(1, vector(100, 100, 0), color(255, 255, 255), "od", "Player Name")
```

---

## Primitives & Shapes

### Lines & Polygons
- `render.line(start: vector, end_pos: vector, col: color) -> void`
- `render.poly(col: color, ...vertices: vector) -> void`: Renders a filled polygon.
- `render.poly_line(col: color, ...vertices: vector) -> void`: Renders an outlined polygon wireframe.

---

### Rectangles
- `render.rect(start: vector, end_pos: vector, col: color, rounding?: integer) -> void`: Filled rectangle.
- `render.rect_outline(start: vector, end_pos: vector, col: color, rounding?: integer) -> void`: Outlined rectangle.
- `render.gradient(start: vector, end_pos: vector, top_left: color, top_right: color, bottom_left: color, bottom_right: color) -> void`: Four-corner gradient box.

---

### Circles & 3D Shapes
- `render.circle(center: vector, col: color, radius: number, start_deg?: number, pct?: number) -> void`: Filled 2D circle or pie slice.
- `render.circle_outline(center: vector, col: color, radius: number, start_deg?: number, pct?: number, thickness?: integer) -> void`: Outlined 2D circle or arc.
- `render.circle_gradient(center: vector, outer_color: color, inner_color: color, radius: number) -> void`: Radial gradient glow circle.
- `render.circle_3d(center: vector, col: color, radius: number) -> void`: Renders a circle on the ground in 3D world coordinates.
- `render.circle_3d_outline(center: vector, col: color, radius: number) -> void`: Renders a 3D circle outline in world space.

---

## Textures & Weapon Icons

### `render.get_weapon_icon`
Retrieves the preloaded DirectX SVG icon texture for a CS:GO weapon definition index.

```lua
render.get_weapon_icon(weapon_id: integer) -> image_t
```

---

### `render.load_image`
Decodes an image from a raw binary string buffer and returns a DirectX texture.

```lua
render.load_image(raw_data: string, size: vector) -> image_t
```

---

### `render.texture`
Draws an `image_t` texture at a specified screen position with optional color modulation.

```lua
render.texture(image: image_t, pos: vector, col?: color) -> void
```

---

## Clipping Rectangles

Use clip rectangles to mask rendering within a specific bounding area (e.g. for scrollable lists or smooth expanding panels):

- `render.push_clip_rect(start: vector, end_pos: vector) -> void`
- `render.pop_clip_rect() -> void`
