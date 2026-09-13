# Standard Library: `vector`

```lua
local vec = require("vector")
```

The `vector` standard library provides geometric, trigonometric, and utility functions extending the built-in `vector` usertype.

---

## Functions

### Creation & Unit Vectors
- `vec.new(x?, y?, z?) -> vector`: Returns a new `vector(x or 0, y or 0, z or 0)`.
- `vec.zero() -> vector`: Returns `vector(0, 0, 0)`.
- `vec.forward() -> vector`: Returns unit forward vector `vector(1, 0, 0)`.
- `vec.right() -> vector`: Returns unit right vector `vector(0, 1, 0)`.
- `vec.up() -> vector`: Returns unit up vector `vector(0, 0, 1)`.

---

### Normalization & Clamping
- `vec.normalize(v: vector) -> vector`: Returns a unit vector with length 1 pointing in the same direction (or zero if length is 0).
- `vec.clamp_length(v: vector, max_length: number) -> vector`: Clamps the vector's magnitude to `max_length`.

---

### Length & Distances
- `vec.length2d(v: vector) -> number`: Computes horizontal 2D length $\sqrt{x^2 + y^2}$.
- `vec.distance(a: vector, b: vector) -> number`: 3D Euclidean distance.
- `vec.distance2d(a: vector, b: vector) -> number`: 2D horizontal distance ignoring Z.

---

### Geometric Operations
- `vec.angle_between(a: vector, b: vector) -> number`: Computes angle between two direction vectors in degrees ($0^\circ$ to $180^\circ$).
- `vec.reflect(v: vector, normal: vector) -> vector`: Calculates reflection of vector `v` bouncing off a surface with given unit `normal`.
- `vec.project(a: vector, b: vector) -> vector`: Projects vector `a` onto vector `b`.
- `vec.rotate2d(v: vector, degrees: number) -> vector`: Rotates a 2D vector around the Z axis by specified degrees.
- `vec.lerp(a: vector, b: vector, t: number) -> vector`: Linear interpolation between `a` and `b` by fraction `t` ($0.0$ to $1.0$).
- `vec.approx_equal(a: vector, b: vector, epsilon?: number) -> boolean`: Checks if two vectors are approximately identical within tolerance.
- `vec.unpack(v: vector) -> number, number, number`: Unpacks into `v.x, v.y, v.z`.
