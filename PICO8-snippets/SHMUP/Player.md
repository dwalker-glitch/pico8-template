SHMUP Player Setup

The player object in a SHMUP needs a simple, core set of variables for position, speed, boundaries, and basic combat state. All of these variables are initialized once in the _init() function.

Required Player Setup (in _init)

```
player = {
    x = 60,
    y = 100,
    speed = 2.5,
    w = 8,
    h = 8,
    hp = 3,
    fire_cooldown = 0,
    invuln_timer = 0,
}
```
| Field | Value | Explanation | 
 | ----- | ----- | ----- | 
| `x` | `60` (example) | **Initial X Position.** Horizontal coordinate (in pixels). | 
| `y` | `100` (example) | **Initial Y Position.** Vertical coordinate (in pixels). | 
| `speed` | `2.5` | **Movement Speed.** How many pixels the player moves per frame. | 
| `w` | `8` | **Width.** Used for boundary checks (e.g., `128 - o.w` for the right edge). | 
| `h` | `8` | **Height.** Used for boundary checks. | 
| `hp` | `3` | **Health Points (Lives).** How many hits the player can take before dying. | 
| `fire_cooldown` | `0` | **Current Fire Cooldown.** A timer that must be reset to `0` before the player can shoot. | 
| `invuln_timer` | `0` | **Invulnerability Timer.** A frame counter set after taking damage to grant temporary invincibility. |
