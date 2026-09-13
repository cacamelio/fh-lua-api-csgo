# Namespace: `plist`

The `plist` namespace interfaces with the cheat's internal Player List system. It allows inspecting player records and programmatically overriding ragebot targeting rules (whitelisting, priority, body aim, safepoint, and resolver foot yaw).

---

## Identifying Players

Functions in `plist` accept any of the following to identify a player:
1. A [`player_t`](../types/entity-and-player.md) object
2. An integer entity index ($1$ to $64$)
3. A string representing the player's 64-bit Steam ID (`"76561198..."`)

---

## Methods

### `plist.get_players`
Returns a snapshot array of all player entries in the player list.

```lua
plist.get_players(connected_only?: boolean) -> table
```
- `connected_only` *(optional, default `true`)*: Only returns players actively connected to the server.

#### Player List Entry Structure
```lua
{
    name = "PlayerName",
    index = 2,
    user_id = 4,
    team = 2,
    health = 100,
    alive = true,
    bot = false,
    enemy = true,
    connected = true,
    steam_id = "STEAM_1:0:123456",
    steam_id64 = "76561198000000000",

    -- Active overrides
    whitelist = false,
    priority = false,
    force_body = false,
    force_safepoint = false,
    force_foot_yaw = false,
    foot_yaw = 0.0
}
```

---

### `plist.get`
Retrieves a specific field value or override for a player.

```lua
plist.get(player: player_t | integer | string, field: string) -> any
```

#### Supported Fields
- Info: `"name"`, `"index"`, `"user_id"`, `"team"`, `"health"`, `"alive"`, `"bot"`, `"enemy"`, `"connected"`, `"steam_id"`, `"steam_id64"`
- Overrides: `"whitelist"`, `"priority"`, `"force_body"`, `"force_safepoint"`, `"force_foot_yaw"`, `"foot_yaw"`

---

### `plist.set`
Applies an override to a player in the player list.

```lua
plist.set(player: player_t | integer | string, field: string, value: any) -> boolean
```

#### Override Fields
- `"whitelist"` (`boolean`): Ignore this player in aimbot.
- `"priority"` (`boolean`): Prioritize targeting this player over others.
- `"force_body"` (`boolean`): Force body aim against this player.
- `"force_safepoint"` (`boolean`): Only shoot at safe points against this player.
- `"force_foot_yaw"` (`boolean`): Enable custom resolver foot yaw angle.
- `"foot_yaw"` (`number`): Custom foot yaw angle in degrees.

---

### `plist.reset` & `plist.reset_all`
- `plist.reset(player)`: Resets overrides for a specific player.
- `plist.reset_all()`: Resets overrides for all players in the list.

---

## Example: Auto-Prioritize High-Threat Target

```lua
client.add_callback("render", function()
    for _, entry in ipairs(plist.get_players()) do
        if entry.enemy and entry.alive then
            -- If enemy is low HP, force body aim to secure the kill
            if entry.health <= 30 then
                plist.set(entry.index, "force_body", true)
            end
        end
    end
end)
```
