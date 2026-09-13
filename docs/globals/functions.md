# Global Functions

The global environment provides built-in utilities for logging, engine chat output, safe function calls, time/tick conversion, and math helpers.

---

## `print`

Prints a log message to the internal developer console.

```lua
print(msg: any) -> void
```

### Parameters
- `msg`: The value or object to print. Converted to a string via `tostring(msg)`.

### Example
```lua
print("Initialized module successfully")
print(vector(10, 20, 30)) -- prints: vector(10, 20, 30)
```

---

## `error`

Outputs an error message to the developer console formatted in red with an `[Arctic]` tag.

```lua
error(msg: string) -> void
```

### Parameters
- `msg`: The error message string.

### Example
```lua
if not local_player then
    error("Local player is nil!")
end
```

---

## `print_raw`

Prints unformatted text to the console with custom color support.

```lua
print_raw(msg: string, color?: color) -> void
```

### Parameters
- `msg`: The message string to print.
- `color` *(optional)*: A `color` object. Defaults to white (`color(255, 255, 255)`).

### Example
```lua
print_raw("Highlighted warning: low fps\n", color(255, 200, 0))
```

---

## `chat_print`

Prints a message directly into the in-game CS:GO player chat HUD (client-side only).

```lua
chat_print(message: string) -> void
```

### Parameters
- `message`: Text message to display in the chat box.

### Example
```lua
chat_print(" [driphook] Config loaded successfully!")
```

---

## `safe_call`

Executes a function inside a protected wrapper. If an error occurs, it catches the error and prints the detailed traceback without interrupting script execution or crashing.

```lua
safe_call(func: function) -> void
```

### Parameters
- `func`: The Lua function to execute safely.

### Example
```lua
safe_call(function()
    local player = entity.get_local_player()
    local origin = player:get_abs_origin()
    print("Origin: " .. tostring(origin))
end)
```

---

## `to_ticks`

Converts a time in seconds (float) to tick count (integer) based on the server's tickrate.

```lua
to_ticks(time: number) -> integer
```

### Formula
$$\text{ticks} = \text{round}\left(\frac{\text{time}}{\text{globals.tickinterval}}\right)$$

### Example
```lua
local ticks = to_ticks(0.5) -- Returns 32 on a 64-tick server, 64 on 128-tick
```

---

## `to_time`

Converts tick count (integer) to time in seconds (float).

```lua
to_time(ticks: integer) -> number
```

### Formula
$$\text{time} = \text{ticks} \times \text{globals.tickinterval}$$

> [!NOTE]
> In certain builds, `to_time` may be aliased to tick conversion. You can always calculate precise time directly using:
> ```lua
> local seconds = ticks * globals.tickinterval
> ```

---

## `math.angle_diff`

Calculates the shortest angular difference between two angles in degrees, properly normalized to $[-180^\circ, 180^\circ]$.

```lua
math.angle_diff(destAngle: number, srcAngle: number) -> number
```

### Parameters
- `destAngle`: The target angle in degrees.
- `srcAngle`: The source angle in degrees.

### Returns
- `number`: The delta angle in range $[-180, 180]$.

### Example
```lua
local diff = math.angle_diff(10, 350)
print(diff) -- 20
```
