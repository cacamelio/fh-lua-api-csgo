# Namespace: `config`

The `config` namespace allows interacting with the cheat's configuration serialization engine to list, export, or import configuration profiles.

---

## Methods

### `config.list`
Returns an array of strings representing all saved cheat config names found on disk.

```lua
config.list() -> table
```

#### Example
```lua
local configs = config.list()
for i, name in ipairs(configs) do
    print(string.format("Config [%d]: %s", i, name))
end
```

---

### `config.dump`
Serializes the current state of all cheat settings into a JSON formatted string.

```lua
config.dump() -> string
```

---

### `config.restore`
Parses and applies a JSON configuration string to the cheat settings.

```lua
config.restore(json_string: string) -> boolean
```

#### Example
```lua
local success = config.restore(my_preset_json)
if success then
    print("Preset loaded successfully!")
end
```
