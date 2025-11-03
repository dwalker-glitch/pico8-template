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

## 2. The fire_blaster() Function

This function is responsible for creating a new blaster object and adding it to the global blasters table when the fire button (PICO-8 button 5, typically 'X' or 'M') is pressed.

| Line | Code | Explanation | 
 | ----- | ----- | ----- | 
| `2` | `if btnp(5) then` | **Input Check:** `btnp(5)` ensures the code runs **only once** when button 5 is *first pressed* (preventing a machine gun effect unless intended). | 
| `3-7` | `add(blasters, { ... })` | **Blaster Creation:** Creates a new **blaster** table (object) and uses `add()` to insert it into the global `blasters` array. | 
| `4-5` | `x=player.x, y=player.y` | **Spawn Position:** The **blaster** spawns exactly where the player is. | 
| `6` | `speed = -BLASTER_SPEED` | **Velocity:** Gives the **blaster** a negative vertical speed (`-5` pixels per frame) to make it travel **up** the screen. |

---

## 3. Update and Draw Functions

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
function fire_blaster()
    if btnp(5) then -- Check for single press of X button
        add(blasters, {
            x=player.x+4, -- Spawn centered horizontally
            y=player.y,
            speed = -BLASTER_SPEED, -- Move up
        })
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
end

function draw_blasters()
    for b in all(blasters) do
        -- Draw the blaster (using sprite ID 1 as an example)
        spr(1, b.x, b.y)
    end
end
```
