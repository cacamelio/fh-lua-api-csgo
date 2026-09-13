# Namespace: `json`

The `json` namespace provides high-performance JSON encoding and decoding between Lua tables and JSON strings (powered by `nlohmann::json`).

---

## Methods

### `json.stringify`
Serializes a Lua table or array into a formatted JSON string.

```lua
json.stringify(table_data: table) -> string
```

#### Example
```lua
local data = {
    name = "Default Preset",
    values = { 10, 20, 30 },
    enabled = true
}

local json_text = json.stringify(data)
print(json_text)
-- Output: {"enabled":true,"name":"Default Preset","values":[10,20,30]}
```

---

### `json.parse`
Parses a JSON string into a native Lua table.

```lua
json.parse(json_string: string) -> table
```

#### Example
```lua
local raw = '{"pitch": 89.0, "yaw": 180.0, "active": true}'
local parsed = json.parse(raw)

print("Pitch: " .. parsed.pitch)
print("Yaw: " .. parsed.yaw)
print("Active: " .. tostring(parsed.active))
```
