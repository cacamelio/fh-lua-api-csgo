# User Type: `qangle`

The `qangle` class represents 3D Euler angles used by the Source engine for player view angles and entity orientations.

---

## Constructors

```lua
qangle() -> qangle(0, 0, 0)
qangle(pitch: number, yaw: number) -> qangle(pitch, yaw, 0)
qangle(pitch: number, yaw: number, roll: number) -> qangle(pitch, yaw, roll)
```

---

## Fields

- `ang.pitch`: Pitch angle (looking up/down). Range $[-89^\circ, 89^\circ]$.
- `ang.yaw`: Yaw angle (rotation around horizontal axis). Range $[-180^\circ, 180^\circ]$.
- `ang.roll`: Roll angle (bank / tilt). Usually $0^\circ$ in CS:GO.

---

## Example

```lua
local my_angles = qangle(89.0, 180.0, 0.0)

client.add_callback("createmove", function(cmd)
    -- Aim straight down and backwards
    cmd.viewangles = my_angles
end)
```
