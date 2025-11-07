# Top-Down Player Setup

The player object needs to be defined within _init() and it should include basic position variables (x, y) and a fixed speed value. We also add optional variables for combat, animation, and game status.

```
player = {
    x = 64,
    y = 64,
    speed = 2.0, 
    max_hp = 100,
    hp = 100,
    invuln_timer = 0,
    sprite = 1,
    fire_cooldown = 0,
    points = 0
}
```

---

| Field | Value | Explanation | 
 | ----- | ----- | ----- | 
| `x` | `64` | **Initial X Position.** The horizontal coordinate (in pixels). | 
| `y` | `64` | **Initial Y Position.** The vertical coordinate (in pixels). | 
| `speed` | `2.0` | **Movement Speed.** The fixed number of pixels the player moves in any direction per frame. | 
| `max_hp` | `100` | **Maximum HP.** The total possible health of the player. | 
| `hp` | `100` | **Current HP.** The player's health. When this hits `0`, the player dies. | 
| `sprite` | `1` | **Sprite ID.** Used to track which sprite number should be drawn for the player's current state (e.g., walking, idle). | 
| `invuln_timer` | `0` | **Invulnerability Timer.** A frame counter. When greater than 0, the player cannot take damage. It is decremented every frame. | 
| `fire_cooldown` | `0` | **Fire Cooldown.** A frame counter. When greater than 0, the player cannot fire another shot. | 
| `points` | `0` | **Player Points.** The score or currency the player has collected. (Often used for score display or shop purchases). |
