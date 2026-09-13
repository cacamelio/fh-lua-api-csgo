# Namespace: `panorama`

The `panorama` namespace interfaces directly with Valve's Panorama UI engine, allowing direct execution of JavaScript code in the context of in-game HUD or main menu panels, as well as accessing UI panel hierarchies.

---

## Methods

### `panorama.load_string`
Executes JavaScript code inside a Panorama UI context.

```lua
panorama.load_string(js_code: string, context_panel?: string) -> boolean
```

#### Parameters
- `js_code`: JavaScript string to execute.
- `context_panel` *(optional)*: The target panel context name (defaults to `"CSGOHud"`). Can also be `"CSGOMainMenu"`.

#### Example
```lua
-- Play a sound through Panorama
panorama.load_string([[
    $.PlaySoundEvent("UIPanorama.inventory_item_note", 1.0);
]])

-- Inspect local player stats or display notification
panorama.load_string([[
    $.Msg("Hello from Panorama via Lua!");
]])
```

---

### `panorama.open`
Retrieves the [`ui_panel_t`](../types/panorama-panel.md) handle for a specified Panorama panel name (defaults to `"CSGOHud"`).

```lua
panorama.open(panel_name?: string) -> ui_panel_t | nil
```

#### Example
```lua
local hud_panel = panorama.open("CSGOHud")
if hud_panel then
    print("HUD panel ID: " .. hud_panel:get_id())
    print("Children count: " .. hud_panel:get_child_count())
end
```
