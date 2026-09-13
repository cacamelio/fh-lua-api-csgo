# Animation Hooks (`"pre_anim_update"`, `"post_anim_update"`, `"local_alpha"`)

These callbacks let you hook into the local player's animation system and transparency calculations.

---

## 1. `"pre_anim_update"`

Triggered immediately **before** the cheat updates the local player animation state (`LocalAnimations`).

### Signature
```lua
client.add_callback("pre_anim_update", function(local_player)
    -- local_player is a player_t object
end)
```

### Use Cases
- Inspecting or resetting custom animation layers prior to matrix calculation.
- Storing eye angles or previous velocity vectors.

---

## 2. `"post_anim_update"`

Triggered immediately **after** local animations have updated and bone matrices have been regenerated.

### Signature
```lua
client.add_callback("post_anim_update", function(local_player)
    -- local_player is a player_t object
end)
```

### Use Cases
- Reading updated animation parameters such as `animstate.duck_amount`, `animstate.move_yaw`, or `animstate.foot_yaw`.
- Custom third-person visual tweaks.

---

## 3. `"local_alpha"`

Triggered whenever the cheat renders the local player model (or viewmodel attachments) to determine transparency modulation (e.g. while scoped).

### Signature
```lua
client.add_callback("local_alpha", function(local_player, current_alpha)
    -- return new_alpha (number, 0.0 to 1.0)
end)
```

- **Arguments**:
  - `local_player`: [`player_t`](../types/entity-and-player.md)
  - `current_alpha`: Float value from $0.0$ (invisible) to $1.0$ (solid).
- **Return Value**:
  - Return a `number` to override the alpha modulation.
  - Return `nil` or omit return to keep the default alpha.

### Example: Smooth Scope Transparency
```lua
client.add_callback("local_alpha", function(local_player, alpha)
    -- If scoped, fade local player to 30% opacity
    if local_player:get_prop("m_bIsScoped") then
        return 0.3
    end
    return 1.0
end)
```
