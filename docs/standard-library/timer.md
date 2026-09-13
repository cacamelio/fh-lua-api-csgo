# Standard Library: `timer`

```lua
local timer = require("timer")
```

The `timer` standard library provides scheduled delays, recurring interval timers, stopwatches, and framerate-independent exponential damping.

> [!IMPORTANT]
> You must call `timer.update()` inside a recurring callback (e.g. `"render"`) to advance active timers!

---

## Functions

### `timer.after`
Schedules a one-off function to execute after a specified delay in seconds.

```lua
timer.after(delay_seconds: number, callback: function) -> integer
```
- Returns an integer timer ID used to cancel the timer.

#### Example
```lua
timer.after(2.5, function()
    print("2.5 seconds have elapsed!")
end)
```

---

### `timer.every`
Schedules a recurring timer that fires repeatedly at a set interval.

```lua
timer.every(interval_seconds: number, callback: function, immediate?: boolean) -> integer
```
- If `callback()` explicitly returns `false`, the timer stops recurring and is removed.
- `immediate` *(optional, default `false`)*: If `true`, fires on the very first update call without waiting for the initial interval.

#### Example
```lua
local count = 0
timer.every(1.0, function()
    count = count + 1
    print("Tick " .. count)
    if count >= 5 then
        return false -- Stop after 5 ticks
    end
end)
```

---

### Cancellation & Status
- `timer.cancel(timer_id: integer) -> void`: Cancels an active timer by ID.
- `timer.cancel_all() -> void`: Cancels all active timers.
- `timer.count() -> integer`: Returns the number of currently pending timers.
- `timer.clock() -> number`: High-resolution clock timestamp (`os.clock()`).

---

### `timer.stopwatch`
Creates a stopwatch object for tracking elapsed durations with pause, resume, and reset methods.

```lua
local sw = timer.stopwatch()
sw:start()
-- ... later ...
print("Elapsed: " .. sw:elapsed() .. "s")
sw:stop()
sw:reset()
```

---

### `timer.damp`
Framerate-independent exponential damping helper (smooth lerp).

```lua
timer.damp(current: number, target: number, speed: number, dt: number) -> number
```

#### Example
```lua
local current_x = 0
client.add_callback("render", function()
    local target_x = ui.is_open() and 200 or 0
    -- Smoothly glide towards target_x regardless of FPS
    current_x = timer.damp(current_x, target_x, 10.0, globals.frametime)
    render.rect(vector(current_x, 100, 0), vector(current_x + 50, 150, 0), color(255, 0, 0))
end)
```
