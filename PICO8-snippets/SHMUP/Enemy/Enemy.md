# Creating Enemies

Handling enemies requires three main systems: defining the enemy's properties, updating their position and status, and drawing them on the screen.

## 1. The Enemy Object

Enemies will be defined like the player - by a table with a series of variables. This allows us to control their position, speed, health, and any other qualities.

In order to create and control many enemies at once we need to create a container to put them in. Adding new enemies to the table will cause them to spawn, and deleting them will cause them to despawn.

```
-- GLobal table to store all active enemies
enemies = {}
```

In addition to the enemies table we need constants, these are variables that all enemies share.

```
-- Shared Constants
ENEMY_SPEED = 1.0
SPAWN_COOLDOWN = 30
spawn_timer = 0
```

| Field | Value | Explanation | 
| ----- | ----- | ----- | 
| ENEMY_SPEED | 1.0 | How fast all enemies move down the screen. | 
| SPAWN_COOLDOWN | 30 | The number of frames (e.g., 1 second) between attempts to spawn a new enemy. | 
| spawn_timer | 0 | A frame counter that tracks when the next spawn attempt can occur. |

In your `_init()` you need the following code to create the table and define the constants:

```
enemies = {}
ENEMY_SPEED = 1.0
SPAWN_COOLDOWN = 30
spawn_timer = 0

ENEMY_SPECS = {
    standard = { hp = 2, score = 100, speed_mult = 1.0, sprite = 9, move_func = move_standard },
    back_and_forth = { hp = 3, score = 200, speed_mult = 0.8, sprite = 10, move_func = nil }, 
    player_chaser = { hp = 1, score = 300, speed_mult = 1.2, sprite = 11, move_func = nil }, 
    circular_path = { hp = 4, score = 250, speed_mult = 0.5, sprite = 12, move_func = nil }, 
}
```

---

## 2. Spawning Logic

Spawning is what we call creating a new object (like an enemy or a power-up) and adding it to the game world.

This function creates a new enemy and adds it to the enemy table. It accepts a parameter we're calling `enemy_type`, that way we can spawn enemies that have different stats with one piece of code.

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
        move_func = spec.move_func or move_standard,
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
        spawn_enemy() -- Spawns a standard enemy by default
        spawn_timer = SPAWN_COOLDOWN
    end

    -- MOVEMENT & CLEANUP LOGIC
    for e in all(enemies) do
        -- 1. Advance the internal timer (used by advanced movement functions like zigzag)
        e.t += 0.05
        
        -- 2. Execute the enemy's assigned movement function
        -- This ensures that any new movement types are automatically handled here.
        if e.move_func then
            e.move_func(e)
        end
        
        -- 3. Cleanup: Remove if off-screen (y > 128)
        if e.y > 128 then
            del(enemies, e)
        end
    end
end
```

---

## 5. Simple Move

There are many ways enemies can move, but for now we are going to use a simple function to move them from the top of the screen to the bottom.

```
function move_standard(e)
    e.y += e.speed
end
```

---

## 6. draw_enemies():

This function simply draws the sprite for every active enemy object. It must be called in `_draw()`.

```
function draw_enemies()
    for e in all(enemies) do
        -- Draw the enemy using its defined sprite ID
        spr(e.sprite, e.x, e.y)
    end
end
```
