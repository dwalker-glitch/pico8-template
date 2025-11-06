# Platformer Movement Basics: Velocity

Before worrying about walls or gravity, we must set up the player's core structure and introduce velocity (vx, vy), which is how we track speed and direction in a physics game.

## Required Player Setup (in _init)

For physics-based movement, the player object (o) needs to be initialized with horizontal (vx) and vertical (vy) velocity variables.

player = {
    x = 64,
    y = 64,
    vx = 0,
    vy = 0 -- Vertical Velocity (speed up/down)
}


Field

Value

Explanation

x

64 (example)

Initial X Position. The horizontal coordinate (in pixels).

y

64 (example)

Initial Y Position. The vertical coordinate (in pixels).

vx

0

Horizontal Velocity. Tracks the player's current speed and direction in the X-axis. Starts at 0.

vy

0

Vertical Velocity. Tracks the player's current speed and direction in the Y-axis. Starts at 0.

---

## The move(o) Function (The Engine)

The move(o) function will run every frame to calculate the new position. For now, we only need the basic structure and constants.

Note: At this stage, we have no input or collision, but the function holds the constants we need later.

```
function move(o)
    -- We save the old position in case we hit a wall later
    local lx=o.x
    local ly=o.y 
    
    -- PLATFORMER CONSTANTS (Tune these values to change the feel of the game)
    local gravity = 0.2     -- Rate of downward acceleration
    local terminal_vel = 3  -- Maximum speed when falling
    local accel = 0.5       -- Horizontal movement force (how fast you speed up)
    local friction = 0.85   -- Multiplier to slow down horizontal movement (0.85 = high friction)
    local max_h_vel = 3     -- Maximum horizontal movement speed

    -- In later steps, we will fill this function with gravity, input, and collision logic.
end
```
