# Enum Reference: `ERoundEndReason`

The `globals.last_round_end_reason` property returns an integer ID corresponding to Valve's internal `ERoundEndReason` enumeration. This enum is broadcasted in demo records and the `round_end` game event.

---

## Enumeration Table

| ID | Enum Constant Name | Description |
| :---: | :--- | :--- |
| `-1` | `Invalid_Round_End_Reason` | Invalid or uninitialized round end reason. |
| `0` | `RoundEndReason_StillInProgress` | Round is still actively ongoing. |
| `1` | `Target_Bombed` | Target was successfully bombed (Bomb exploded). |
| `2` | `VIP_Escaped` | The VIP successfully escaped. |
| `3` | `VIP_Assassinated` | The VIP was assassinated. |
| `4` | `Terrorists_Escaped` | Terrorists successfully escaped the map zone. |
| `5` | `CTs_PreventEscape` | Counter-Terrorists prevented Terrorists from escaping. |
| `6` | `Escaping_Terrorists_Neutralized`| Escaping Terrorists were eliminated. |
| `7` | `Bomb_Defused` | The C4 bomb was successfully defused by CTs. |
| `8` | `CTs_Win` | Counter-Terrorists win (Terrorists eliminated or target defended). |
| `9` | `Terrorists_Win` | Terrorists win (Counter-Terrorists eliminated). |
| `10` | `Round_Draw` | Round ended in a stalemate / draw. |
| `11` | `All_Hostages_Rescued` | Counter-Terrorists rescued all hostages. |
| `12` | `Target_Saved` | Bomb target saved (Time expired before bomb could be planted). |
| `13` | `Hostages_Not_Rescued` | Time expired without hostages being rescued. |
| `14` | `Terrorists_Not_Escaped` | Time expired without Terrorists escaping. |
| `15` | `VIP_Not_Escaped` | Time expired without VIP escaping. |
| `16` | `Game_Commencing` | Game commencing / warm-up completed. |
| `17` | `Terrorists_Surrender` | Terrorist team voted to surrender. |
| `18` | `CTs_Surrender` | Counter-Terrorist team voted to surrender. |
| `19` | `Terrorists_Planted` | Terrorists planted the bomb. |
| `20` | `CTs_ReachedHostage` | Counter-Terrorists reached hostage rescue zone. |
| `21` | `RoundEndReason_Count` | Total count of enum values. |

---

## Example Lua Lookup Table

```lua
local RoundEndReasons = {
    [-1] = "Invalid",
    [0]  = "Still in Progress",
    [1]  = "Target Bombed",
    [2]  = "VIP Escaped",
    [3]  = "VIP Assassinated",
    [4]  = "Terrorists Escaped",
    [5]  = "CTs Prevented Escape",
    [6]  = "Escaping Terrorists Neutralized",
    [7]  = "Bomb Defused",
    [8]  = "Counter-Terrorists Win",
    [9]  = "Terrorists Win",
    [10] = "Round Draw",
    [11] = "All Hostages Rescued",
    [12] = "Target Saved",
    [13] = "Hostages Not Rescued",
    [14] = "Terrorists Not Escaped",
    [15] = "VIP Not Escaped",
    [16] = "Game Commencing",
    [17] = "Terrorists Surrender",
    [18] = "CTs Surrender",
    [19] = "Terrorists Planted",
    [20] = "CTs Reached Hostage"
}

client.add_callback("game_events", function(event)
    if event:get_name() == "round_end" then
        local reason = globals.last_round_end_reason
        local friendly_name = RoundEndReasons[reason] or ("Unknown (" .. reason .. ")")
        print("Round Result: " .. friendly_name)
    end
end)
```
