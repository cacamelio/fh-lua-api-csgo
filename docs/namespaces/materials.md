# Namespace: `materials`

The `materials` namespace allows dynamically creating procedural Source engine materials at runtime, looking up existing materials, and executing custom chams draw calls.

---

## Methods

### `materials.create`
Creates a procedural Material with custom shader and KeyValues parameters.

```lua
materials.create(material_name: string, shader_name: string, key_values: table) -> material_t
```

#### Parameters
- `material_name`: Unique material identifier.
- `shader_name`: Source engine shader (e.g. `"VertexLitGeneric"`, `"UnlitGeneric"`, `"Modulate"`).
- `key_values`: Table of shader parameters without the leading `$` (the prefix is prepended automatically).

#### Example
```lua
local glow_material = materials.create("my_glow", "VertexLitGeneric", {
    ["basetexture"] = "vgui/white",
    ["wireframe"] = "0",
    ["additive"] = "1",
    ["ignorez"] = "1"
})
```

---

### `materials.find`
Finds an existing material by its texture/material name.

```lua
materials.find(material_name: string) -> material_t | nil
```

---

### `materials.draw_chams`
Draws the active model execution pass using the specified material override.

```lua
materials.draw_chams(material: material_t) -> void
```
- Typically called within the [`"draw_chams"`](../callbacks/chams.md) callback.
