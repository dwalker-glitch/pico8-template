# PICO-8 Platformer Movement and Collision

This comprehensive document covers the foundational "Move and Check" collision method and introduces realistic platformer physics: gravity, momentum, and friction.

---

## 1. Required Player Setup (in _init)

For physics-based movement, the player object (o) needs to be initialized with horizontal (vx) and vertical (vy) velocity variables.

```
player = {
    x = 64,
    y = 64,
    vx = 0,
    vy = 0
}
```

| Field | Value | Explanation |
| :--- | :--- | :--- |
| `x` | `64` (example) | **Initial X Position.** The horizontal coordinate of the player (in pixels). |
| `y` | `64` (example) | **Initial Y Position.** The vertical coordinate of the player (in pixels). |
| `vx` | `0` | **Horizontal Velocity.** Tracks the player's current speed and direction in the X-axis (used for momentum and friction). Starts at `0`. |
| `vy` | `0` | **Vertical Velocity.** Tracks the player's current speed and direction in the Y-axis (used for gravity and jumping). Starts at `0`. |

---

## 2. The collide(o) Function

The collide() function determines if the player's 8×8 sprite is currently overlapping a solid map tile (a tile with Flag 0 set).


```
function collide(o)
    -- Calculate the four corners of the 8x8 sprite in tile coordinates
    local x1=o.x/8
    local y1=o.y/8
    local x2=(o.x+7)/8
    local y2=(o.y+7)/8
    
    -- Check Flag 0 (solid flag) for all four corners
    local a=fget(mget(x1,y1),0) -- Top-Left
    local b=fget(mget(x1,y2),0) -- Bottom-Left
    local c=fget(mget(x2,y1),0) -- Top-Right
    local d=fget(mget(x2,y2),0) -- Bottom-Right
    
    -- If ANY corner is touching a solid tile, return true
    if a or b or c or d then
        return true
    else
        return false
    end
end
```

---

### Four-Corner Check

Since collision is based on 8x8 tiles, you must check all four corners to ensure no part of the sprite is inside a wall. We use o.x+7 and o.y+7 to find the bottom-right edge of the 8x8 sprite.

---

## 3. Gravity Physics (Y-Axis)

Gravity is handled by continuously increasing the vertical velocity (o.vy) and checking for collision after the movement is applied.

| Variable/Concept | Code Role | Explanation | 
 | ----- | ----- | ----- | 
| **`o.vy`** | Vertical Velocity | Tracks current speed and direction (positive is down, negative is up). | 
| **`gravity`** | `o.vy = o.vy + gravity` | Continuously accelerates the player downward, simulating gravity. | 
| **`terminal_vel`** | Speed Clamp | Sets a maximum falling speed to prevent infinite acceleration. | 
| **Landing** | `if collide(o) then ... o.vy=0` | If the downward move causes a collision, `o.vy` is reset to `0`, causing the player to stop falling and "land" on the ground. |

---

## 4. Momentum & Friction Physics (X-Axis)

Horizontal movement is controlled by gradually accelerating the player and applying friction to slow them down when no buttons are pressed.

| Variable/Concept | Code Role | Explanation | 
 | ----- | ----- | ----- | 
| **`o.vx`** | Horizontal Velocity | Tracks the current left/right speed and direction. | 
| **Acceleration** | `o.vx += accel` | Player input adds a small force (`accel`) to the velocity, gradually speeding the player up. | 
| **Friction** | `o.vx *= friction` | Reduces the horizontal speed every frame, causing the player to slow down and glide to a stop when input is released (the *momentum*). | 
| **Wall Hit** | `if collide(o) then ... o.vx=0` | If the horizontal move causes a collision, `o.vx` is reset to `0`, causing the player to immediately stop and preventing them from getting stuck in the wall. |

---

## 5. move(o) Function

This function integrates all physics (gravity, momentum, friction) and input, performing the necessary separate collision checks for the Y-axis and X-axis.

```
function move(o)
    local lx=o.x -- Save the last safe X position
    local ly=o.y -- Save the last safe Y position
    
    -- PLATFORMER CONSTANTS (Tune these values to change the feel of the game)
    local gravity = 0.2     -- Rate of downward acceleration
    local terminal_vel = 3  -- Maximum speed when falling
    local accel = 0.5       -- Horizontal movement force
    local friction = 0.85   -- Multiplier to slow down horizontal movement (0.85 = high friction)
    local max_h_vel = 3     -- Maximum horizontal movement speed

    -- 1. APPLY GRAVITY & Y-VELOCITY
    o.vy = o.vy + gravity
    
    -- Clamp max falling speed
    if o.vy > terminal_vel then o.vy = terminal_vel end 
    
    -- Apply speculative move on Y-axis
    o.y = o.y + o.vy
    
    -- 2. COLLISION CHECK (Y AXIS)
    if collide(o) then
        o.y=ly    -- Collision! Move back to safe position
        o.vy=0    -- Stop vertical velocity (the player has landed or hit a ceiling)
    end
    
    -- 3. APPLY HORIZONTAL INPUT & FRICTION (MOMENTUM)
    if (btn(1)) o.vx += accel -- Right: increase velocity
    if (btn(0)) o.vx -= accel -- Left: decrease velocity
    
    o.vx *= friction -- Apply friction: slow down velocity every frame
    
    -- Clamp max speed using PICO-8's sgn() to maintain the correct direction
    if abs(o.vx) > max_h_vel then
        o.vx = max_h_vel * sgn(o.vx)
    end
    
    -- Apply speculative move on X-axis
    o.x = o.x + o.vx
    
    -- 4. COLLISION CHECK (X AXIS)
    if collide(o) then
        o.x=lx    -- Collision! Move back to safe position
        o.vx=0    -- Stop horizontal velocity (the player has hit a wall)
    end
end
```
