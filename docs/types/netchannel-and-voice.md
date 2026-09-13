# User Types: `net_channel_info_t` & Voice Data

This section documents `net_channel_info_t` (engine network statistics) and voice network data types.

---

## 1. `net_channel_info_t`

Obtained via `utils.get_net_channel()`.

### Methods
- `net:get_name() -> string`: Name of the net channel.
- `net:get_address() -> string`: IP address of the connected server.
- `net:get_time() -> number`: Current network time.
- `net:get_time_connected() -> number`: Total seconds connected to the server.
- `net:get_timeout_seconds() -> number`: Network connection timeout threshold.
- `net:get_time_since_last_received() -> number`: Seconds since last packet arrived.
- `net:get_latency(flow: integer) -> number`: Ping latency in seconds (`flow`: `0` for OUT, `1` for IN).
- `net:get_avg_latency(flow: integer) -> number`: Averaged ping latency.
- `net:get_loss(flow: integer) -> number`: Packet loss percentage ($0.0$ to $100.0$).
- `net:get_choke(flow: integer) -> number`: Choke percentage ($0.0$ to $100.0$).
- `net:get_packets(flow: integer) -> number`: Packets per second.
- `net:get_data(flow: integer) -> number`: Data throughput in bytes/second.
- `net:get_sequence(flow: integer) -> integer`: Current packet sequence number.
- `net:is_loopback() -> boolean`: True if hosting or connected to local listen server.
- `net:is_timing_out() -> boolean`: True if connection is timing out.
- `net:is_playback() -> boolean`: True if playing back a demo file.
- `net:is_valid_packet(flow: integer, seq: integer) -> boolean`: Checks packet validity.
- `net:get_packet_time(flow: integer, seq: integer) -> number`: Timestamp of packet.

#### Example
```lua
local net = utils.get_net_channel()
if net then
    local ping_ms = math.floor(net:get_latency(0) * 1000)
    local loss = math.floor(net:get_loss(1))
    print(string.format("Ping: %d ms | Loss: %d%%", ping_ms, loss))
end
```

---

## 2. `recv_voice_data_t`

Passed to the `"voice_message"` callback.

### Fields
- `data.client`: [`player_t`](entity-and-player.md) sender entity pointer.
- `data.audible_mask`: Audible client bitmask.
- `data.xuid`: 64-bit Steam ID of sender.
- `data.xuid_low` / `data.xuid_high`: Low/High 32-bit halves of XUID.
- `data.format`: Audio encoding format.
- `data.sequence_bytes`: Sequence length.
- `data.section_number`: Voice section index.
- `data.uncompressed_sample_offset`: Sample offset integer.
- `data:get_voice_data() -> string`: Returns the raw binary audio payload string.

---

## 3. `voice_data`

Constructible voice structure used with `utils.send_voice_message()`.

### Constructor
```lua
voice_data() -> voice_data
```

### Fields
- `v.xuid_low`: Low 32 bits of XUID.
- `v.xuid_high`: High 32 bits of XUID.
- `v.sequence_bytes`: Payload byte count.
- `v.section_number`: Section sequence index.
- `v.uncompressed_sample_offset`: Sample offset.
