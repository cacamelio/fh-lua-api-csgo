# User Types: `entity_t`, `player_t` & `weapon_t`

This section documents the entity class hierarchy in the cheat:
- `entity_t`: Base class for all game entities (`CBaseEntity`).
- `player_t`: Specialized player class inheriting from `entity_t` (`CBasePlayer`).
- `weapon_t`: Specialized combat weapon class inheriting from `entity_t` (`CBaseCombatWeapon`).

---

## 1. `entity_t`

### Methods
- `ent:ent_index() -> integer`: Returns the entity index.
- `ent:get_abs_origin() -> vector`: Returns the entity origin position in world space.
- `ent:get_abs_angles() -> qangle`: Returns the entity orientation angles.
- `ent:obb_mins() -> vector`: Collidable bounding box minimum vector.
- `ent:obb_maxs() -> vector`: Collidable bounding box maximum vector.
- `ent:collision_origin() -> vector`: Center coordinate of the entity's collision hull.
- `ent:is_player() -> boolean`: Returns `true` if this entity is a player.
- `ent:is_weapon() -> boolean`: Returns `true` if this entity is a weapon.
- `ent:set_model_index(index: integer) -> void`: Sets the visual model index.
- `ent:ptr() -> integer`: Returns the native C++ raw memory address of the entity.
- `ent:get_classid() -> integer`: Returns the ClientClass ID integer.
- `ent:get_classname() -> string`: Returns the network class name string (e.g. `"CCSPlayer"`, `"CWeaponAK47"`).

### Dynamic Netvar Access
You can read and write network variables directly using table indexing:
```lua
local team = ent.m_iTeamNum       -- Read Netvar
ent.m_flFlashDuration = 0.0      -- Write Netvar
```

---

## 2. `player_t` (Inherits from `entity_t`)

### Identity & Status
- `pl:get_name() -> string`: Returns the player's Steam persona name.
- `pl:get_player_info() -> player_info_t`: Returns the [`player_info_t`](#player_info_t) record.
- `pl:is_alive() -> boolean`: Returns whether the player is alive.
- `pl:is_enemy() -> boolean`: Returns whether the player is an enemy relative to local player.
- `pl:is_bot() -> boolean`: Returns whether the player is a server bot.
- `pl:is_dormant() -> boolean`: Returns whether the player is dormant (out of PVS).
- `pl:get_team() -> integer`: Returns team number (`2` for Terrorist, `3` for Counter-Terrorist).
- `pl:is_defusing() -> boolean`: Returns whether the player is actively defusing the bomb.

### Health, Armor & Flags
- `pl:get_health() -> integer`: Current health points ($0$ to $100+$).
- `pl:get_armor() -> integer`: Current armor value ($0$ to $100$).
- `pl:has_helmet() -> boolean`: Returns whether the player has a helmet.
- `pl:get_flags() -> integer`: Player bitflags (`FL_ONGROUND = 1`, `FL_DUCKING = 2`, etc.).
- `pl:get_velocity() -> vector`: Current 3D movement velocity vector.
- `pl:get_eye_angles() -> qangle`: Player eye / view angles.
- `pl:get_eye_position() -> vector`: Player eye origin in 3D space.

### Hitboxes & Bones
- `pl:get_hitbox_position(hitbox_id: integer) -> vector`: Returns the center world position of a hitbox ($0$ = Head, $1$ = Neck, $2$ = Pelvis, $3$ = Stomach, etc.).
- `pl:get_bone_position(bone_id: integer) -> vector`: Returns the world position of a bone matrix index ($0$ to $127$).
- `pl:get_bounding_box() -> vector, vector`: Returns the 2D screen-space bounding box: `mins, maxs`.

### Animations & Weapons
- `pl:get_active_weapon() -> weapon_t | nil`: Returns the weapon currently held in hands.
- `pl:get_animstate() -> animstate_t`: Returns the player's animation state.
- `pl:get_animlayers() -> array of animlayer_t`: Returns an array of the 13 active animation layers.
- `pl:get_simulation_time() -> table`: Returns `{ [1] = cur_simtime, [2] = old_simtime }`.
- `pl:get_dormant_last_update() -> number`: Last update time of dormant ESP tracker.
- `pl:get_esp_alpha() -> number`: ESP opacity float based on dormant fade ($0.0$ to $1.0$).
- `pl:set_icon(image: image_t) -> void`: Overrides the player's icon texture.

---

## 3. `weapon_t` (Inherits from `entity_t`)

### Methods
- `wep:weapon_index() -> integer`: CS:GO Item Definition Index (e.g. `7` for AK47, `9` for AWP).
- `wep:get_inaccuracy() -> number`: Weapon inaccuracy float.
- `wep:get_spread() -> number`: Weapon bullet spread cone.
- `wep:shooting_weapon() -> boolean`: Returns `true` if this weapon can fire bullets.
- `wep:can_shoot() -> boolean`: Returns `true` if weapon is ready to fire this tick.
- `wep:get_icon() -> image_t`: Returns weapon SVG icon texture.
- `wep:is_grenade() -> boolean`: Returns `true` if this item is a grenade.
- `wep:is_knife() -> boolean`: Returns `true` if this item is a knife.
- `wep:get_weapon_info() -> any`: Returns pointer to `CCSWeaponInfo` struct.

---

## `player_info_t`

The table returned by `pl:get_player_info()` contains:
- `info.name`: Player name string.
- `info.userid`: Server User ID integer.
- `info.steamID`: 32-bit account ID.
- `info.szSteamID`: Formatted string (e.g. `"STEAM_1:0:123456"`).
- `info.steamID64`: 64-bit Steam ID integer.
- `info.xuid_low`: Low 32 bits of XUID.
- `info.xuid_high`: High 32 bits of XUID.
- `info.bot`: `true` if fake player / bot.
