# Namespace: `cvar`

The `cvar` namespace allows querying and manipulating Source engine console variables (`ConVar`).

---

## Methods

### `cvar.find` or `cvar[name]`
Looks up an engine console variable by name. Returns a `convar` usertype, or `nil` if not found.

```lua
-- Direct method
local cv = cvar.find("sv_cheats")

-- Or table indexing syntax
local cv = cvar["sv_cheats"]
local fov = cvar.viewmodel_fov
```

---

## The `convar` Object

| Method / Property | Description |
| :--- | :--- |
| `cv:get_name() -> string` | Retrieves the name of the ConVar. |
| `cv:int([new_value]) -> integer` | Gets or sets integer value. |
| `cv:float([new_value]) -> number` | Gets or sets float value. |
| `cv:string([new_value]) -> string` | Gets or sets string value. |
| `cv:add_flag(flag: integer)` | Adds an FCVAR flag bit. |
| `cv:remove_flag(flag: integer)` | Removes an FCVAR flag bit (e.g. remove `FCVAR_CHEAT`). |
| `cv.description` | Help string description. |
| `cv.default` | Default value string. |
| `cv.flags` | Bitfield flags integer. |
| `cv.has_min` / `cv.has_max` | Booleans indicating clamp bounds. |
| `cv.min` / `cv.max` | Minimum and maximum allowed float values. |

---

## Example: Changing Viewmodel FOV

```lua
local fov_cvar = cvar.viewmodel_fov
if fov_cvar then
    print("Default viewmodel FOV: " .. fov_cvar:float())
    fov_cvar:float(75.0) -- Set to 75
end
```
