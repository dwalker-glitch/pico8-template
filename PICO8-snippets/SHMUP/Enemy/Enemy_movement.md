Enemy Movement Patterns

This section implements various complex movement patterns by utilizing the enemy's type field and its internal timer (t) within the main update_enemies() loop.

1. Advanced Enemy Spawner

To use these advanced behaviors, you must use a dedicated function that includes the type and t properties during initialization.

A. The spawn_advanced_enemy Function

```
-- Function to spawn an enemy with advanced properties
function spawn_advanced_enemy(x_pos, enemy_type)
    add(enemies, {
        x = x_pos,
        y = -8,
        speed = 1.0,
        hp = 2,              -- Health Points (HP)
        score_value = 200,
        type = enemy_type,   -- 'back_and_forth', 'circular_path', or 'player_chaser'
        t = 0,               -- Timer used for pattern movement
        max_health = 2,
        shoot_timer = 0,     -- Timer for enemy firing
    })
end
```

-- Example call: spawn_advanced_enemy(64, "player_chaser")


2. Movement Implementations

This logic must be placed inside the update_enemies() function. For each enemy, we check its type and apply the corresponding movement calculation before handling cleanup.

```
function update_enemies()
    -- ... spawning logic here ...

    for e in all(enemies) do
        -- 1. Increment the enemy's internal timer (used for smooth patterns)
        e.t += 0.05 

        -- 2. Check the enemy's behavior type
        if e.type == "back_and_forth" then
            -- Moves vertically (e.speed) but horizontally based on a cosine wave.
            -- This creates a smooth left-right zigzag pattern.
            e.y += e.speed
            -- cos(e.t * 3) makes the horizontal oscillation faster
            e.x += cos(e.t * 3) * 0.5 
        
        elseif e.type == "player_chaser" then
            -- Homing: The enemy attempts to match the player's X position.
            local player_x = player.x -- Assumes 'player' object is defined globally
            local chase_speed = 0.5
            
            -- Compare the enemy's X position to the player's X position
            if player_x > e.x then
                e.x += chase_speed -- Move right
            elseif player_x < e.x then
                e.x -= chase_speed -- Move left
            end
            
            -- Always move down
            e.y += e.speed
            
        elseif e.type == "circular_path" then
            -- Uses sine/cosine for an orbiting motion (as defined previously)
            local anchor_x = 64
            local orbit_radius = 20
            e.y += 0.2
            e.x = anchor_x + cos(e.t) * orbit_radius
        
        else 
            -- Default vertical movement for simple enemies
            e.y += e.speed
        end

        -- ... collision and cleanup logic follows ...
    end
end
```
