# User Type: `vector`

The `vector` class represents a 3D coordinate or direction vector ($x, y, z$). It features overloaded arithmetic operators and geometric utility methods.

---

## Constructors

```lua
vector() -> vector(0, 0, 0)
vector(x: number, y: number) -> vector(x, y, 0)
vector(x: number, y: number, z: number) -> vector(x, y, z)
```

---

## Fields

- `vec.x`: Floating-point X component.
- `vec.y`: Floating-point Y component.
- `vec.z`: Floating-point Z component.

---

## Operators & Metamethods

- **Addition**: `v1 + v2`
- **Subtraction**: `v1 - v2`
- **Multiplication**: `v1 * v2` or `v1 * number`
- **Division**: `v1 / v2` or `v1 / number`
- **Equality**: `v1 == v2`
- **Length Operator**: `#v1` returns the Euclidean length of the vector.
- **String Conversion**: `tostring(v)` returns `"vector(x, y, z)"`.

---

## Methods

### `vec:length` & `vec:length_sqr`
- `vec:length() -> number`: Returns Euclidean length $\sqrt{x^2 + y^2 + z^2}$.
- `vec:length_sqr() -> number`: Returns squared length $x^2 + y^2 + z^2$.
- `vec:length2d_sqr() -> number`: Returns squared 2D horizontal length $x^2 + y^2$.

---

### `vec:dist`
Calculates the 3D distance between this vector and another.

```lua
vec:dist(other: vector) -> number
```

---

### `vec:dot` & `vec:cross`
- `vec:dot(other: vector) -> number`: Computes vector dot product.
- `vec:cross(other: vector) -> vector`: Computes 3D cross product vector.

---

### `vec:lerp`
Linearly interpolates between this vector and another.

```lua
vec:lerp(target: vector, weight: number) -> vector
```

---

### `vec:angles`
Converts between direction vectors and Euler angles:
- `vec:angles() -> qangle`: Converts this direction vector into a `qangle` (`pitch`, `yaw`, `roll`).
- `vec:angles(angle: qangle) -> vector`: Computes the forward direction vector from an angle.

---

### `vec:to_screen`
Projects this 3D world coordinate to 2D screen pixels.

```lua
vec:to_screen() -> vector
```
- Returns a `vector` where `x` and `y` are pixel coordinates on your monitor.

---

### `vec:closest_ray_point` & `vec:dist_to_ray`
- `vec:closest_ray_point(start_pos: vector, end_pos: vector) -> vector`: Returns the point on a ray segment closest to this vector.
- `vec:dist_to_ray(start_pos: vector, end_pos: vector) -> number`: Returns the perpendicular distance from this point to a ray.
