# Chams Override Callback (`"draw_chams"`)

The `"draw_chams"` callback is called inside `CChams::DrawModelExecute` before drawing materials for a model. It lets you inspect the entity class type and selectively override or cancel model rendering.

---

## Signature

```lua
client.add_callback("draw_chams", function(entity_type)
    -- return code (0, 1, or nil)
end)
```

- **Arguments**:
  - `entity_type`: Integer representing the model's [`ClassOfEntity`](../constants-and-enums/entity-classes.md).
- **Return Value**:
  - `0`: Handled. Cheat will skip its standard internal layer rendering (useful if you drew custom materials via `materials.draw_chams`).
  - `1`: Blocked. Cancels all model rendering for this entity pass completely.
  - `nil`: Normal. Allows default cheat chams rendering to proceed.

---

## Entity Type IDs (`ClassOfEntity`)

| ID | Identifier | Description |
| :---: | :--- | :--- |
| `0` | `Enemy` | Enemy player models. |
| `1` | `Shot` | On-shot chams record models. |
| `2` | `Backtrack` | Backtrack history records. |
| `3` | `Team` | Teammate player models. |
| `4` | `LocalPlayer` | Local player third-person model. |
| `5` | `ViewModel` | First-person hands / arms model. |
| `6` | `Weapon` | First-person or dropped weapon model. |
| `7` | `Attachment` | Model attachments (sleeves, gloves, hats). |
| `8` | `Fake` | Desync fake angle ghost model. |

---

## Example: Custom Material Chams

```lua
-- Create a custom glow material
local my_mat = materials.create("my_glow_mat", "VertexLitGeneric", {
    ["$basetexture"] = "vgui/white",
    ["$wireframe"] = "0"
})

client.add_callback("draw_chams", function(entity_type)
    -- If rendering enemy models
    if entity_type == 0 then -- Enemy
        -- Modulate color to bright cyan
        my_mat:color_modulate(color(0, 220, 255))
        my_mat:alpha_modulate(0.85)

        -- Draw model with our custom material
        materials.draw_chams(my_mat)

        -- Return 0 to tell the cheat we handled the rendering
        return 0
    end
end)
```
