# Namespace: `entity`

The `entity` namespace is the primary gateway to querying game entities, players, weapons, entity handles, and the CS:GO player resource.

---

## Methods

### `entity.get`
Retrieves an entity by its index or player User ID. Automatically resolves the entity into its specialized user type:
- If the entity is a player $\rightarrow$ returns [`player_t`](../types/entity-and-player.md).
- If the entity is a combat weapon $\rightarrow$ returns [`weapon_t`](../types/entity-and-player.md).
- Otherwise $\rightarrow$ returns [`entity_t`](../types/entity-and-player.md).
- Returns `nil` if the entity does not exist or is invalid.

```lua
entity.get(index: integer, is_userid?: boolean) -> player_t | weapon_t | entity_t | nil
```

#### Parameters
- `index`: Entity index ($1$ to $64$ for players, $>64$ for props/weapons/grenades) or User ID.
- `is_userid` *(optional)*: If `true`, `index` is treated as a server `userid` (as found in `player_hurt` or `player_death` events) and converted to an entity index via `EngineClient->GetPlayerForUserID`. Default is `false`.

#### Example
```lua
-- Get player from event userid
local victim = entity.get(event.userid, true)
if victim then
    print("Victim: " .. victim:get_name())
end
```

---

### `entity.get_local_player`
Returns the local player entity object.

```lua
entity.get_local_player() -> player_t | nil
```

#### Example
```lua
local me = entity.get_local_player()
if me and me:is_alive() then
    print("Local HP: " .. me:get_health())
end
```

---

### `entity.get_player_resource`
Returns the global `CCSPlayerResource` entity, allowing direct Netvar querying of team scores, ping, competitive ranks, and player levels.

```lua
entity.get_player_resource() -> entity_t | nil
```

#### Example
```lua
local pr = entity.get_player_resource()
if pr then
    local pings = pr.m_iPing -- Returns an ArrayWrapper
    print("My Ping: " .. pings[1])
end
```

---

### `entity.from_handle`
Retrieves an entity pointer by its 32-bit unsigned entity handle (`CBaseHandle`).

```lua
entity.from_handle(handle: integer) -> player_t | weapon_t | entity_t | nil
```

#### Example
```lua
local local_player = entity.get_local_player()
if local_player then
    local wep_handle = local_player.m_hActiveWeapon
    local weapon = entity.from_handle(wep_handle)
    if weapon then
        print("Active Weapon ID: " .. weapon:weapon_index())
    end
end
```
