# File Structure & Modules

The cheat manages its Lua filesystem relative to the CS:GO process execution directory. All directories are created automatically upon cheat initialization if they do not already exist.

---

## Directory Layout

```
<Game Directory>/
└── driphook/
    └── scripts/
        ├── cfg/
        │   ├── my_script.cfg        # Auto-generated JSON config for my_script.lua
        │   └── visuals.cfg          # Auto-generated JSON config for visuals.lua
        ├── lib/
        │   ├── my_library.lua       # Custom library accessible via require("my_library")
        │   └── math_utils/
        │       └── init.lua         # Accessible via require("math_utils")
        ├── my_script.lua            # User script
        └── visuals.lua              # User script
```

---

## The `package.path` Configuration

The cheat dynamically configures Lua's `package.path` during setup. The search paths are initialized in the following priority order:

1. System Lua default search paths
2. `driphook/scripts/?.lua`
3. `driphook/scripts/?/init.lua`
4. `driphook/scripts/lib/?.lua`
5. `driphook/scripts/lib/?/init.lua`

This means you can import helper files using standard Lua `require`:

```lua
-- If you have driphook/scripts/lib/helpers.lua
local helpers = require("helpers")

-- If you have driphook/scripts/lib/anti_aim/init.lua
local aa_toolkit = require("anti_aim")
```

---

## Preloaded Standard Libraries

Before checking `package.path`, LuaJIT checks `package.preload`. The cheat comes with 6 compiled-in standard libraries ready to use without needing physical files on disk:

```lua
local vector  = require("vector")   -- Math & vector utilities
local color   = require("color")    -- Color spaces, HSV, hex, interpolation
local timer   = require("timer")    -- High-precision timers, stopwatch, damp
local ease    = require("ease")     -- In/Out easing curves
local animate = require("animate")  -- Keyframe animations
local class   = require("class")    -- OOP class definition & inheritance
```

---

## Automated Configuration (`cfg/`)

When you create UI widgets using `ui.groupbox` methods (such as `checkbox`, `slider_int`, `color_picker`, etc.), the cheat automatically binds their state to your script's identity:

- **Saving**: Whenever a script unloads or the cheat triggers `ScriptSaveButton()`, the values of all widgets belonging to that script are serialized to JSON in `driphook/scripts/cfg/<script_name>.cfg`.
- **Loading**: When you load a script, the cheat looks for `driphook/scripts/cfg/<script_name>.cfg`. If present, all widget values are restored and their respective Lua change callbacks are fired automatically.
- **Format**: The configuration is pure JSON and can be inspected or edited manually with any text editor.
