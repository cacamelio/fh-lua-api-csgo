# Namespace: `ui`

The `ui` namespace provides complete control over creating custom menu tabs, groupboxes, and widgets that seamlessly integrate into the cheat's GUI. It also provides tools to inspect and bind to existing built-in cheat menu settings.

---

## Creation Methods

### `ui.tab`
Creates a brand new top-level tab in the cheat menu.

```lua
ui.tab(name: string, icon?: image_t) -> CMenuTab*
```

#### Parameters
- `name`: Display name of the tab.
- `icon` *(optional)*: Direct3D texture icon. If omitted, uses the default scripts icon.

---

### `ui.groupbox`
Creates a new groupbox container inside an existing tab (such as `"Scripts"`, `"Rage"`, `"Visuals"`, or your own custom tab).

```lua
ui.groupbox(tab_name: string, groupbox_name: string) -> groupbox_t
```

#### Example
```lua
-- Add a groupbox to the built-in "Scripts" tab
local my_box = ui.groupbox("Scripts", "My Utility")

-- Or create a new tab and put a groupbox inside
ui.tab("Lua Hub")
local hub_box = ui.groupbox("Lua Hub", "Features")
```

---

## Querying and Inspection Methods

### `ui.find`
Finds an existing menu groupbox or widget by tab, groupbox name, and optional widget name and type.

```lua
ui.find(tab_name: string, groupbox_name: string, widget_name?: string, widget_type?: integer) -> groupbox_t | ui_element_t | nil
```

#### Example
```lua
-- Find a groupbox
local aimbot_box = ui.find("Rage", "Aimbot")

-- Find a specific widget (e.g. Master Switch)
local rage_enabled = ui.find("Rage", "Aimbot", "Enabled")
if rage_enabled then
    print("Ragebot is currently: " .. tostring(rage_enabled:get()))
end
```

---

### `ui.is_open`
Returns whether the cheat menu GUI is currently open on screen.

```lua
ui.is_open() -> boolean
```

---

### `ui.get_binds`
Returns a table containing all active keybinds that are set to show in the cheat's keybind list.

```lua
ui.get_binds() -> table
```

#### Return Table Structure
Each entry in the returned array contains:
- `id`: Internal unique keybind string ID.
- `name`: Display name of the keybind.
- `key`: Virtual key code (e.g. `0x01` for mouse1, `0x10` for shift).
- `mode`: Keybind mode integer (`0` = Always on, `1` = Hold, `2` = Toggle, `3` = Force off).

#### Example
```lua
client.add_callback("render", function()
    local binds = ui.get_binds()
    local y = 200
    for _, b in ipairs(binds) do
        render.text(1, vector(20, y, 0), color(255, 255, 255), "", b.name)
        y = y + 16
    end
end)
```

---

### Menu Tree Exploration

- `ui.tabs() -> table`: Returns an array of strings representing all registered tab names.
- `ui.groups(tab_name: string) -> table`: Returns an array of groupbox names inside a given tab.
- `ui.elements(tab_name: string, group_name: string) -> table`: Returns an array of widget descriptions inside a groupbox.
- `ui.all() -> table`: Returns an array of description tables for every widget across the entire menu!

```lua
-- Dump entire menu structure to console
for _, w in ipairs(ui.all()) do
    print(string.format("[%s -> %s] %s (%s)", w.tab or "?", w.group or "?", w.name, w.type))
end
```
