# User Type: `ui_panel_t`

The `ui_panel_t` class wraps Valve's Panorama `IUIPanel` interface, allowing inspection and manipulation of HTML/XML-like UI panels in CS:GO.

---

## Methods

### Identification & Hierarchy
- `panel:get_id() -> string`: Returns the panel element ID (e.g. `"HudHealth"`, `"CSGOHud"`).
- `panel:get_parent() -> ui_panel_t | nil`: Returns parent panel.
- `panel:get_child_count() -> integer`: Number of direct children.
- `panel:get_child(index: integer) -> ui_panel_t | nil`: Child by 0-based index.
- `panel:get_first_child() -> ui_panel_t | nil`: First child element.
- `panel:get_last_child() -> ui_panel_t | nil`: Last child element.
- `panel:find_child(name: string) -> ui_panel_t | nil`: Recursively searches children for panel ID.
- `panel:find_child_in_layout_file(name: string) -> ui_panel_t | nil`: Searches XML layout.

---

### Visibility & Classes
- `panel:is_visible() -> boolean`: True if panel is rendered.
- `panel:set_visible(visible: boolean) -> void`: Toggles panel visibility.
- `panel:has_class(class_name: string) -> boolean`: Checks CSS class presence.
- `panel:add_class(class_name: string) -> void`: Adds CSS class.
- `panel:remove_class(class_name: string) -> void`: Removes CSS class.
- `panel:toggle_class(class_name: string) -> void`: Toggles CSS class.
- `panel:set_has_class(class_name: string, enable: boolean) -> void`: Explicitly sets class state.

---

### State & Input
- `panel:set_enabled(enable: boolean) -> void`: Enables/disables panel.
- `panel:is_enabled() -> boolean`: Returns enabled state.
- `panel:set_accepts_input(accept: boolean) -> void`: Toggles input capture.
- `panel:accepts_input() -> boolean`: Returns whether panel captures mouse/keyboard input.

---

### Layout & Dynamic Children
- `panel:load_layout_from_string(xml_str: string) -> boolean`: Loads XML layout into panel.
- `panel:create_children(xml_str: string) -> void`: Dynamically instantiates child elements.
- `panel:remove_and_delete_children() -> void`: Deletes all children.
