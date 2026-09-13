# Reference: UI Widget Types & Keybind Modes

This reference documents the internal widget types returned by `elem:type()` and the keybind modes used by `elem:get_mode()` and `ui.get_binds()`.

---

## Widget Type IDs (`WidgetType`)

| Type ID | String Name | Description | Value Type in `elem:get()` |
| :---: | :--- | :--- | :--- |
| `0` | `"checkbox"` | Boolean toggle switch. | `boolean` |
| `1` | `"slider_int"` | Integer slider. | `integer` |
| `2` | `"slider_float"` | Floating-point slider. | `number` |
| `3` | `"label"` | Static informational label. | `nil` |
| `4` | `"color_picker"` | RGBA color picker. | [`color`](../types/color.md) |
| `5` | `"combo"` | Single-select dropdown list. | `integer` (0-indexed selection) |
| `6` | `"multi_combo"` | Multi-select flag box. | `table` (array of booleans) |
| `7` | `"button"` | Clickable button trigger. | `nil` (use `elem:click()`) |
| `8` | `"input"` | Single-line text input field. | `string` |

---

## Keybind Modes

Returned by `elem:get_mode()` and used in `elem:set_mode(mode)`:

| Mode ID | Name | Behavior |
| :---: | :--- | :--- |
| `0` | **Always On** | Feature is permanently enabled regardless of key state. |
| `1` | **Hold** | Feature is active only while the assigned key is held down. |
| `2` | **Toggle** | Pressing the key toggles the feature state on and off. |
| `3` | **Force Off** | Feature is forced disabled regardless of other inputs. |
