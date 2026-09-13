# Enum Reference: `ClassOfEntity`

The `ClassOfEntity` enumeration categorizes models passed into the [`"draw_chams"`](../callbacks/chams.md) callback.

---

## Enumeration Table

| Value | Constant Name | Description | Base Model Present |
| :---: | :--- | :--- | :---: |
| `0` | `Enemy` | Enemy player models. | Yes |
| `1` | `Shot` | Ragebot on-shot ghost backtrack record. | No |
| `2` | `Backtrack` | Backtrack tick player trail records. | No |
| `3` | `Team` | Teammate player models. | Yes |
| `4` | `LocalPlayer` | Local third-person character model. | Yes |
| `5` | `ViewModel` | First-person viewmodel hands/arms. | Yes |
| `6` | `Weapon` | First-person and dropped weapon models. | Yes |
| `7` | `Attachment` | Sleeves, gloves, and wearable accessories. | Yes |
| `8` | `Fake` | Local player desync fake angle ghost model. | Yes |

---

## Example Usage

```lua
client.add_callback("draw_chams", function(ent_type)
    if ent_type == 4 then -- LocalPlayer
        -- Do not render local player model
        return 1 -- Block / cancel
    elseif ent_type == 5 then -- ViewModel
        -- Custom viewmodel styling...
    end
end)
```
