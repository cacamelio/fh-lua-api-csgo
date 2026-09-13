# User Types: `ui_element_t` & `groupbox_t`

This section documents `groupbox_t` (the widget container) and `ui_element_t` (the widget control handle).

---

## 1. `groupbox_t`

Returned by `ui.groupbox()` or `ui.find()`. Used to instantiate UI widgets inside a container box.

### Methods

#### `gb:checkbox`
Adds a boolean toggle checkbox.

```lua
gb:checkbox(name: string, default_value?: boolean) -> ui_element_t
```

#### `gb:slider_int`
Adds an integer slider.

```lua
gb:slider_int(name: string, min: integer, max: integer, default_value: integer, format?: string) -> ui_element_t
```
- `format` *(optional)*: Display formatting (default `"%d"`).

#### `gb:slider_float`
Adds a floating-point slider.

```lua
gb:slider_float(name: string, min: number, max: number, default_value: number, format?: string) -> ui_element_t
```
- `format` *(optional)*: Display formatting (default `"%.2f"`).

#### `gb:combo`
Adds a single-selection dropdown combo box.

```lua
gb:combo(name: string, ...items: string) -> ui_element_t
```

#### `gb:multicombo`
Adds a multi-selection dropdown box with bitflags.

```lua
gb:multicombo(name: string, ...items: string) -> ui_element_t
```

#### `gb:color_picker`
Adds an RGBA color picker popup attached to the previous widget or standalone.

```lua
gb:color_picker(name: string, default_color?: color) -> ui_element_t
```

#### `gb:keybind`
Adds a keybind button control.

```lua
gb:keybind(name: string) -> ui_element_t
```

#### `gb:input`
Adds a text input box (up to 64 characters).

```lua
gb:input(name: string) -> ui_element_t
```

#### `gb:button`
Adds a clickable button.

```lua
gb:button(name: string) -> ui_element_t
```

#### `gb:label`
Adds static text label.

```lua
gb:label(text: string) -> ui_element_t
```

---

## 2. `ui_element_t`

Handle returned when any widget is created or found via `ui.find()`.

### Value Access
- `elem:get([index]) -> any`: Returns the widget's current value:
  - Checkbox $\rightarrow$ `boolean`
  - SliderInt $\rightarrow$ `integer`
  - SliderFloat $\rightarrow$ `number`
  - ColorPicker $\rightarrow$ `color`
  - Combo $\rightarrow$ `integer` (0-indexed active slot)
  - MultiCombo $\rightarrow$ array of booleans, or a single boolean if `index` is passed
  - Input $\rightarrow$ `string`
- `elem:set(value, [index]) -> void`: Sets the widget value programmatically.

### Callbacks & Events
- `elem:set_callback(func: function) -> void`: Registers a Lua function called whenever the widget value is changed by the user.

### Visibility & Attributes
- `elem:visible(bool: boolean) -> void`: Hides or shows the widget.
- `elem:get_visible() -> boolean`: Returns whether the widget is visible.
- `elem:get_name() -> string`: Widget display name.
- `elem:type() -> integer`: Returns the [WidgetType](../constants-and-enums/widget-types.md) enum ID.
- `elem:get_min() -> number`: Minimum value (sliders).
- `elem:get_max() -> number`: Maximum value (sliders).
- `elem:get_items() -> table`: Array of string item names (combos).
- `elem:update_list(items: table) -> void`: Dynamically updates the items of a combo or multicombo!
- `elem:click() -> void`: Programmatically clicks a button widget, triggering its callbacks.

### Keybind Configuration
- `elem:get_key() -> integer`: Assigned virtual key code.
- `elem:set_key(key: integer, mode?: integer) -> void`: Sets keybind key and mode.
- `elem:get_mode() -> integer`: Keybind mode (`0` = Always on, `1` = Hold, `2` = Toggle, `3` = Force off).
- `elem:set_mode(mode: integer) -> void`: Sets mode.
- `elem:clear_key() -> void`: Unbinds key.
