# User Types: `material_t` & `image_t`

This section documents `material_t` (Source engine `IMaterial` wrapper) and `image_t` (`DXImage` DirectX texture wrapper).

---

## 1. `material_t`

Represents a Source engine material used in model and world rendering.

### Methods
- `mat:get_name() -> string`: Returns the internal material name.
- `mat:color_modulate(col?: color) -> color`: Gets or sets RGB color modulation.
- `mat:alpha_modulate(alpha?: number) -> number`: Gets or sets alpha transparency modulation ($0.0$ to $1.0$).
- `mat:var_flag(flag: integer, value?: boolean) -> boolean`: Gets or sets material var flags (e.g. `MATERIAL_VAR_WIREFRAME`, `MATERIAL_VAR_IGNOREZ`, `MATERIAL_VAR_NO_DRAW`).
- `mat:increment_ref_count() -> void`: Increments reference counter.
- `mat:decrement_ref_count() -> void`: Decrements reference counter.

#### Example
```lua
local mat = materials.find("models/player/custom_player/legacy/ctm_sas")
if mat then
    mat:color_modulate(color(255, 0, 100))
    mat:alpha_modulate(0.8)
end
```

---

## 2. `image_t`

Represents a Direct3D texture loaded into GPU memory.

### Properties
- `img.width`: Texture width in pixels.
- `img.height`: Texture height in pixels.

### Usage
Pass `image_t` objects to `render.texture(img, pos, [color])` or `player:set_icon(img)`.
