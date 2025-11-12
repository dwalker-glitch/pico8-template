# Blaster Logic (Fire, Update, Draw)

To handle blasters in a space shooter, we need an array (a Lua table) to hold multiple blaster objects. Each blaster will have its own position and speed.

## 1. Required Setup (in _init)

We need two initializations: the main table to hold all the active blasters, and the constants for the blaster itself.

| Field | Value | Explanation |
| :--- | :--- | :--- |
| `blasters` | `{}` | **Blaster Collection.** This is a global table that will store every active **blaster** object. |
| `BLASTER_SPEED` | `5` (example) | **Constant Speed.** How many pixels per frame the **blaster** travels. |

```
-- Example Initialization (in your _init function)

-- Global table to store all active blasters
blasters = {}

-- Constant speed for all blasters
BLASTER_SPEED = 5
```

---

## 2. Cooldown Timer: Frame Counting

When the game runs, the _update() function is called every frame (60 times per second normally). We use this reliable, consistent interval to manage all in-game timers and cooldowns.

Frame Counting is the simple technique of continuously decrementing a counter (like player.fire_cooldown) inside _update() until it reaches 0. When the counter is at 0, the action (firing a shot) is allowed.

`update_player_timers(o)`

This new helper function must be called in _update() every frame to handle time progression for the player object (`o`).

| Line | Code | Explanation | 
| :--- | :--- | :--- | 
| `2` | `if btnp(5) and player.fire_cooldown == 0 then` | **Combined Check:** Only fire if the button is pressed *and* the timer is ready. | 
| `3-7` | `add(blasters, { ... })` | **Blaster Creation:** Creates and inserts the new projectile. | 
| `8` | `player.fire_cooldown = player.max_cooldown` | **Cooldown Reset:** Immediately sets the counter back to the maximum value, starting the delay. |

```
function update_player_timers(o)
    -- Decrement the fire cooldown every frame
    if o.fire_cooldown > 0 then
        o.fire_cooldown -= 1
    end
end
```

## 3. The fire_blaster() Function

This function is responsible for creating a new blaster object and adding it to the global blasters table when the fire button (PICO-8 button 5, typically 'X' or 'M') is pressed.

| Line | Code | Explanation | 
 | ----- | ----- | ----- | 
| `2` | `if btnp(5) and player.fire_cooldown == 0 then` | **Combined Check:** Only fire if the button is pressed *and* the timer is ready. | 
| `3` | `add(blasters, { ... })` | **Blaster Creation:** Starts a new object definition inside the `blasters` array. | 
| `4-5` | `x=player.x+4, y=player.y` | **Spawn Position:** The blaster spawns slightly offset (`+4` to center on an 8-pixel sprite) at the player's Y position. | 
| `6` | `speed = -BLASTER_SPEED` | **Velocity:** Gives the blaster negative vertical speed to move **up** the screen. | 
| `8` | `player.fire_cooldown = player.max_cooldown` | **Cooldown Reset:** Immediately sets the counter back to the maximum value, starting the delay. |

---

## 4. Collision Detection

This is the key to destroying enemies. We use a proximity check that tests if the bounding box of a blaster is overlapping the bounding box of an enemy. Since we'll assume both sprites are 8 pixels wide (+8 in the check), this logic is very reliable.

The function `check_blaster_hits()` should be called inside your main `_update()` loop after blasters and enemies have been moved.

| Line | Code | Explanation |
| :--- | :--- | :--- |
| `2` | `for b in all(blasters) do` | Loop through every active **blaster** (`b`). |
| `3` | `for e in all(enemies) do` | **Nested Loop:** Check this blaster against every active **enemy** (`e`). |
| `4` | `if b.x < e.x+8 and b.x+4 > e.x` | **AABB Check (X-axis):** Checks if the blaster's left edge is left of the enemy's right edge, AND the blaster's right edge is right of the enemy's left edge. (Using a small 4-pixel width for the blaster hitbox). |
| `5` | `and b.y < e.y+8 and b.y+8 > e.y then` | **AABB Check (Y-axis):** Checks if the blaster's top edge is above the enemy's bottom edge, AND the blaster's bottom edge is below the enemy's top edge. |
| `7` | `del(enemies, e)` | **Destruction:** The enemy is hit and immediately removed from the `enemies` table. |
| `8` | `del(blasters, b)` | The blaster has served its purpose and is removed from the `blasters` table. |
| `9` | `break` | **Optimization:** Stop checking this blaster against other enemies—it's already been destroyed! |

```
function check_blaster_hits()
    -- Loop through all active blasters
    for b in all(blasters) do
        -- Check this blaster against all enemies
        for e in all(enemies) do
            -- AABB Collision Check (Assuming 8x8 enemy sprite and an 8-pixel tall blaster)
            if b.x < e.x+8 and b.x+4 > e.x 
            and b.y < e.y+8 and b.y+8 > e.y then
                
                -- Hit! Destroy both the enemy and the blaster
                del(enemies, e)
                del(blasters, b)
                
                -- Stop checking this blaster against other enemies
                break 
            end
        end
    end
end
```

---

## 5. Update and Draw Functions

These functions are called inside your main _update() and _draw() loops every frame.

### update_blasters(): Moving and Cleanup

This function moves every active blaster and removes those that have gone off-screen (cleanup).

| Line | Code | Explanation | 
 | ----- | ----- | ----- | 
| `2` | `for b in all(blasters) do` | **Iteration:** Loops through every **blaster** object (`b`) currently in the `blasters` table. | 
| `3` | `b.y += b.speed` | **Movement:** Moves the **blaster** vertically by its `speed` (which is a negative number, so it moves up). | 
| `4-6` | `if b.y < -8 then ...` | **Cleanup:** If the **blaster's** Y position is off the top of the screen (less than 0, with a small buffer of -8), it is removed from the `blasters` table using `del()`. |

### draw_blasters(): Rendering

This function simply draws the sprite for every active blaster.
| Line | Code | Explanation | 
 | ----- | ----- | ----- | 
| `2` | `for b in all(blasters) do` | **Iteration:** Loops through every **blaster** object (`b`). | 
| `3` | `spr(1, b.x, b.y)` | **Drawing:** Draws **Sprite 1** (a small square is a common default) at the **blaster's** current position. |

---

## Full Blaster Code

```
function update_player_timers(o)
    -- Cooldown Timer: Decrement until 0
    if o.fire_cooldown > 0 then
        o.fire_cooldown -= 1
    end
    
    -- (Invulnerability timer logic would go here later)
end

function fire_blaster()
    -- Only fire if the button is pressed AND the cooldown is ready (== 0)
    if btnp(5) and player.fire_cooldown == 0 then 
        add(blasters, {
            x=player.x+4, -- Spawn centered horizontally on player sprite
            y=player.y,
            speed = -BLASTER_SPEED, -- Move up
        })
        
        -- Reset the cooldown timer immediately
        player.fire_cooldown = player.max_cooldown
    end
end

function check_blaster_hits()
    -- Loop through all active blasters
    for b in all(blasters) do
        -- Check this blaster against all enemies
        for e in all(enemies) do
            -- AABB Collision Check (Blaster vs. Enemy)
            if b.x < e.x+8 and b.x+4 > e.x 
            and b.y < e.y+8 and b.y+8 > e.y then
                
                -- Hit! Destroy both the enemy and the blaster
                del(enemies, e)
                del(blasters, b)
                
                -- Stop checking this blaster against other enemies
                break 
            end
        end
    end
end

function update_blasters()
    for b in all(blasters) do
        b.y += b.speed
        
        -- Cleanup: Remove the blaster if it flies off the top of the screen
        if b.y < -8 then
            del(blasters, b)
        end
    end
    
    -- Run collision check after movement is complete
    check_blaster_hits()
end

function draw_blasters()
    for b in all(blasters) do
        -- Draw the blaster (using sprite ID 1 as an example)
        spr(1, b.x, b.y)
    end
end
```
