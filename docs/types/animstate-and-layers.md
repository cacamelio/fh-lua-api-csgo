# User Types: `animstate_t` & `animlayer_t`

This section documents `animstate_t` (`CCSGOPlayerAnimationState`) and `animlayer_t` (`AnimationLayer`), which provide full transparency into CS:GO's procedural animation system.

---

## 1. `animstate_t`

Obtained via `player:get_animstate()`.

### Angles & Feet
- `state.eye_yaw`: Pitch view angle currently used by animstate.
- `state.eye_pitch`: Yaw view angle.
- `state.foot_yaw`: Current rendered feet direction.
- `state.last_foot_yaw`: Previous foot yaw.
- `state.move_yaw`: Current direction of player movement relative to facing angle.
- `state.move_yaw_ideal`: Ideal movement yaw calculated by velocity.
- `state.move_yaw_current_to_ideal`: Delta between current and ideal move yaw.
- `state.min_aim_yaw` / `state.max_aim_yaw`: Aim yaw clamping boundaries.
- `state.min_pitch` / `state.max_pitch`: Aim pitch clamping boundaries.

### Movement & Ducking
- `state.duck_amount`: Current crouch fraction ($0.0$ standing, $1.0$ fully ducked).
- `state.duck_additional`: Additional crouch interpolation factor.
- `state.recrouch_weight`: Weight for rapid crouching transitions.
- `state.speed_normalized`: Speed normalized between $0.0$ and $1.0$.
- `state.running_speed`: Running fraction.
- `state.ducking_speed`: Ducking movement speed fraction.
- `state.move_weight`: Weight applied to movement blending.
- `state.move_weight_smoothed`: Smoothed movement weight.
- `state.duration_moving`: Continuous seconds spent moving.
- `state.duration_still`: Continuous seconds spent completely motionless.

### Air & Ground Tracking
- `state.on_ground`: Boolean indicating if the player is touching ground.
- `state.landing`: Boolean indicating if the player is undergoing landing animation.
- `state.duration_in_air`: Time in seconds spent airborne.
- `state.left_ground_height`: Height coordinate where the player left the ground.
- `state.hit_ground_weight`: Impact compression factor when landing.
- `state.walk_to_run_transition`: Transition blend ratio between walking and running.
- `state.in_air_smooth_value`: Air smoothing accumulator.

### Positions & Velocities
- `state.origin`: World position vector.
- `state.last_origin`: Previous tick position vector.
- `state.velocity`: 3D velocity vector.
- `state.velocity_normalized`: Unit direction vector of velocity.
- `state.velocity_length_2d`: Horizontal speed in units/sec.
- `state.velocity_z`: Vertical velocity.

---

## 2. `animlayer_t`

Obtained via `player:get_animlayers()`, which returns an array of 13 animation layers.

### Fields
- `layer.client_blend`: Boolean indicating if client blending is enabled.
- `layer.layer_fade_out`: Fade-out blend factor.
- `layer.order`: Layer composite priority order.
- `layer.sequence`: Activity sequence index.
- `layer.prev_cycle`: Animation cycle progress on the previous frame ($0.0$ to $1.0$).
- `layer.cycle`: Current animation cycle progress ($0.0$ to $1.0$).
- `layer.weight`: Current blend weight ($0.0$ to $1.0$).
- `layer.weight_delta_rate`: Rate of weight transition.
- `layer.playback_rate`: Speed multiplier of the animation playback.
- `layer.owner`: Entity pointer owning this animation layer.
