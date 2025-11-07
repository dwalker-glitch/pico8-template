# Enemy Management

Handling enemies requires three main systems: defining the enemy's properties, a mechanism for creating them on the screen (spawning), and a function to handle their movement and removal (cleanup).

## 1. Required Setup (in _init)

In addition to the enemies table (which is already set up), we need constants to define the enemy's default behavior and control the spawn rate.

| Field | Value | Explanation | 
| ----- | ----- | ----- | 
| `ENEMY_SPEED` | `1.0` (example) | **Constant Speed.** How fast all enemies move down the screen. | 
| `SPAWN_COOLDOWN` | `30` (example) | **Spawn Rate.** The number of frames (e.g., 1 second) between attempts to spawn a new enemy. | 
| `spawn_timer` | `0` | **Active Timer.** A frame counter that tracks when the next spawn attempt can occur. |

```
-- Example Initialization (in your _init function)

-- Global table to store all active enemies
enemies = {}

-- Constants for enemy behavior
ENEMY_SPEED = 1.0
SPAWN_COOLDOWN = 30
spawn_timer = 0
```

---

## 2. The Enemy Object Structure

When we create a new enemy, we add it to the enemies table using a structure that contains its position, movement, and combat state.

```
-- Example: A single enemy object added to the 'enemies' table
{
    x = 60,
    y = -8,
    hp = 1,
    speed = ENEMY_SPEED,
    score_value = 100,
}
```

| Field | Purpose | Example Value | 
| ----- | ----- | ----- | 
| `x`, `y` | **Position** | `x=rnd(120), y=-8` | 
| `hp` | **Health Points** | `1` | 
| `speed` | **Vertical Movement** | `ENEMY_SPEED` | 
| `score_value` | **Reward** | `100` |

---

## 3. Spawning Logic

Spawning is the process of programmatically creating a new object (like an enemy or a power-up) and adding it to the game world's collection (the enemies table) with its initial position and properties.

This function creates a new enemy instance. We use rnd(128 - 8) to place the enemy randomly across the top of the screen (0 to 120, assuming an 8-pixel width) and start them just above the visible boundary (y = -8).

```
function spawn_enemy()
    add(enemies, {
        -- Start X randomly across the screen width
        x = rnd(120), 
        -- Start Y just above the top edge
        y = -8, 
        -- Set movement and health
        speed = ENEMY_SPEED,
        hp = 1,
        score_value = 100,
    })
end
```

---

## 4. update_enemies():

This function handles two jobs: moving all existing enemies down the screen, and checking if it's time to spawn a new enemy. This function must be called in _update().

Timer and Cleanup Logic

| Line | Code | Explanation | 
| ----- | ----- | ----- | 
| `2-4` | `spawn_timer ... spawn_enemy()` | **Spawning:** If the timer is ready (`<= 0`), call the spawn function and reset the timer. | 
| `6` | `for e in all(enemies) do` | **Iteration:** Loop through every active enemy object. | 
| `7` | `e.y += e.speed` | **Movement:** Moves the enemy down the screen. | 
| `8-10` | `if e.y > 128 then ...` | **Cleanup:** Removes the enemy if it moves off the bottom of the screen to save memory. |

```
function update_enemies()
    -- SPAWNING LOGIC (Called first)
    spawn_timer -= 1
    if spawn_timer <= 0 then
        spawn_enemy()
        spawn_timer = SPAWN_COOLDOWN
    end

    -- MOVEMENT & CLEANUP LOGIC (Called second)
    for e in all(enemies) do
        -- 1. Move the enemy down
        e.y += e.speed
        
        -- 2. Cleanup: Remove if off-screen (y > 128)
        if e.y > 128 then
            del(enemies, e)
        end
        
        -- (Collision check and damage logic will go here later)
    end
end
```

---

## 5. draw_enemies(): Rendering

This function simply draws the sprite for every active enemy object. It must be called in _draw().

```
function draw_enemies()
    for e in all(enemies) do
        -- Draw the enemy (using sprite ID 9 as an example)
        spr(9, e.x, e.y)
    end
end
```
