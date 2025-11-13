# Creating Enemies

Handling enemies requires three main systems: defining the enemy's properties, updating their position and status, and drawing them on the screen.

## 1. The Enemy Object

Enemies will be defined like the player - by a table with a series of variables. This allows us to control their position, speed, health, and any other qualities.

```
-- Example: A single enemy object added to the 'enemies' table
{
    x = 60,
    y = -8,
    hp = 2, 
    speed = ENEMY_SPEED,
    score_value = 100,
    type = "standard",
    t = 0
}
```

In order to create and control many enemies at once we need to create a container to put them in. Adding new enemies to the table will cause them to spawn, and deleting them will cause them to despawn.

In addition to the enemies table we need constants, these are variables that all enemies share.

| Field | Value | Explanation | 
| ----- | ----- | ----- | 
| ENEMY_SPEED | 1.0 | How fast all enemies move down the screen. | 
| SPAWN_COOLDOWN | 30 | The number of frames (e.g., 1 second) between attempts to spawn a new enemy. | 
| spawn_timer | 0 | A frame counter that tracks when the next spawn attempt can occur. |

In your `_init()` you need the following code to create the table and define the constants:

```
-- Global table to store all active enemies
enemies = {}

-- Shared Constants
ENEMY_SPEED = 1.0
SPAWN_COOLDOWN = 30
spawn_timer = 0

-- Enemy Configuration: Defines HP, score, and speed adjustments by type
ENEMY_SPECS = {
    standard = { hp = 2, score = 100, speed_mult = 1.0 },
    back_and_forth = { hp = 3, score = 200, speed_mult = 0.8 }, -- Tougher, slower zigzag
    player_chaser = { hp = 1, score = 300, speed_mult = 1.2 },  -- Fragile, high-value, fast chaser
    circular_path = { hp = 4, score = 250, speed_mult = 0.5 },  -- Very slow, tanky orbital path
}
```

---

## 2. Spawning Logic

Spawning is what we call creating a new object (like an enemy or a power-up) and adding it to the game world.

This function creates a new enemy and adds it to the enemy table. The `rnd()` function generates a random number within a range, we use `rnd(128)` to place the enemy randomly across the top of the screen (0 to 120, assuming an 8-pixel width) and start them just above the visible boundary (y = -8).

```
function spawn_enemy(enemy_type)
    -- If no type is provided (e.g., spawn_enemy()), default to "standard"
    local type_name = enemy_type or "standard"
    -- Look up the stats in the specifications table
    local spec = ENEMY_SPECS[type_name] or ENEMY_SPECS.standard

    add(enemies, {
        x = rnd(120), 
        y = -8, 
        
        -- Apply the type-specific speed multiplier and stats
        speed = ENEMY_SPEED * spec.speed_mult,
        hp = spec.hp,
        max_health = spec.hp,
        score_value = spec.score,
        sprite = spec.sprite,
        
        -- Set the type and initial timers
        type = type_name, 
        t = 0,
        shoot_timer = 0,
    })
end
```

---

## 4. update_enemies():

This function handles two jobs: moving all existing enemies down the screen, and checking if it's time to spawn a new enemy. This function must be called in `_update()`

Timer and Cleanup Logic

| Line | Code | Explanation | 
| ----- | ----- | ----- | 
| `2-4` | `spawn_timer ... spawn_enemy()` | If the timer is ready (`<= 0`), call the spawn function and reset the timer. | 
| `6` | `for e in all(enemies) do` | Update each active enemy object. | 
| `7` | `e.y += e.speed` | Moves the enemy. | 
| `8-10` | `if e.y > 128 then ...` | Removes the enemy if it moves off the bottom of the screen. |

```
function update_enemies()
    -- SPAWNING LOGIC
    spawn_timer -= 1
    if spawn_timer <= 0 then
        spawn_enemy()
        spawn_timer = SPAWN_COOLDOWN
    end

    -- MOVEMENT & CLEANUP LOGIC
    for e in all(enemies) do
        -- 1. Move the enemy down
        e.y += e.speed
        
        -- 2. Cleanup: Remove if off-screen (y > 128)
        if e.y > 128 then
            del(enemies, e)
        end
    end
end
```

---

## 5. draw_enemies():

This function simply draws the sprite for every active enemy object. It must be called in `_draw()`.

```
function draw_enemies()
    for e in all(enemies) do
        -- Draw the enemy (using sprite ID 9 as an example)
        spr(9, e.x, e.y)
    end
end
```
