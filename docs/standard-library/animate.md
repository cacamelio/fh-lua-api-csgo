# Standard Library: `animate`

```lua
local animate = require("animate")
local ease = require("ease")
```

The `animate` standard library provides a keyframe tweening engine built on top of high-resolution timing and easing functions.

> [!IMPORTANT]
> You must call `animate.update()` inside a recurring callback (e.g. `"render"`) to advance active animations!

---

## Functions

### `animate.new`
Creates a one-off animated transition between two numbers.

```lua
animate.new(from: number, to: number, duration: number, easing?: function, on_update?: function, on_complete?: function) -> integer
```

#### Parameters
- `from`: Initial numeric value.
- `to`: Final numeric value.
- `duration`: Time in seconds.
- `easing` *(optional)*: Easing curve from `require("ease")` (defaults to `ease.linear`).
- `on_update(val, t)` *(optional)*: Function called each frame with the interpolated value and progress $t \in [0, 1]$.
- `on_complete()` *(optional)*: Function called when animation reaches 100%.

#### Example
```lua
local menu_alpha = 0

local anim_id = animate.new(0, 255, 0.35, ease.out_cubic, function(val)
    menu_alpha = val
end, function()
    print("Fade-in animation finished!")
end)
```

---

### `animate.loop`
Creates an infinite ping-pong looping animation alternating between `from` and `to`.

```lua
animate.loop(from: number, to: number, duration: number, easing?: function, on_update?: function) -> integer
```

#### Example
```lua
local glow_radius = 5

animate.loop(5, 25, 1.2, ease.in_out_sine, function(val)
    glow_radius = val
end)
```

---

### Animation Control
- `animate.pause(id: integer) -> void`: Pauses an animation.
- `animate.resume(id: integer) -> void`: Resumes a paused animation.
- `animate.cancel(id: integer) -> void`: Cancels and deletes an animation.
- `animate.cancel_all() -> void`: Cancels all active animations.
- `animate.is_running(id: integer) -> boolean`: Checks if an animation is active.
- `animate.get_value(id: integer) -> number`: Retrieves the current value of an animation.
- `animate.count() -> integer`: Returns the number of currently active animations.
- `animate.update() -> void`: Ticks the animation pipeline (call every frame).
