# Advanced Enemy Mechanics
To create challenging encounters, we can assign special behaviors to enemies, including complex movement paths, retaliatory fire, and visual death effects. We will achieve this by adding a type field to the base enemy object.

## 1. Advanced Enemy Movement Patterns

To implement various movement patterns, we'll assign each enemy a type and use a switch (or if/elseif) statement inside the main update loop to change their velocity based on their behavior.

### A. The Enemy Object Update

We need to add a type field during spawn and give the enemy object an initial timer (t) for cycle-based movement (like circular paths or back-and-forth).

```
-- Modified spawn_enemy function to assign a type
function spawn_advanced_enemy(x_pos, enemy_type)
    add(enemies, {
        x = x_pos,
        y = -8,
        speed = 1.0,
        hp = 2,
        score_value = 200,
        type = enemy_type, -- 'back_and_forth', 'circle', or 'chaser'
        t = 0,             -- Timer for sine/cosine movement
        max_health = 2,
    })
end

-- Example call: spawn_advanced_enemy(rnd(120), "circle")
```

### B. Movement Implementations

We'll place this logic within the enemy loop in update_enemies():

```
function update_enemies()
    -- ... spawning logic here ...

    for e in all(enemies) do
        -- Increment the enemy's internal timer
        e.t += 0.05 

        if e.type == "back_and_forth" then
            -- Moves vertically (e.speed) but horizontally based on a sine wave (sin(t))
            e.y += e.speed
            e.x += cos(e.t * 3) * 0.5 -- Faster horizontal oscillation
        
        elseif e.type == "circular_path" then
            -- Uses cosine and sine functions to create orbit (needs an anchor point)
            local anchor_x = 64
            local orbit_radius = 20
            
            -- Move down slowly while orbiting
            e.y += 0.2
            e.x = anchor_x + cos(e.t) * orbit_radius
            
        elseif e.type == "player_chaser" then
            -- Simple homing: Move towards the player's current X position
            local player_x = player.x -- Assume 'player' is defined globally
            
            if player_x > e.x then
                e.x += 0.5 -- Move right
            elseif player_x < e.x then
                e.x -= 0.5 -- Move left
            end
            
            -- Always move down
            e.y += e.speed
        
        else 
            -- Default movement for simple enemies
            e.y += e.speed
        end

        -- ... collision and cleanup logic here ...
    end
end
```

## 2. Enemy Weapon Systems

For enemies to shoot, we need to implement a dedicated bullet system separate from the player's. This prevents friendly fire and simplifies collision checks.

### A. Enemy Bullet Setup

We need a new table in _init and constants for enemy bullet behavior.

```
-- In _init()
enemy_bullets = {}
ENEMY_BULLET_SPEED = 3.0
ENEMY_SHOOT_COOLDOWN = 60 -- Shoot every 60 frames (1 second)
```

### B. Enemy Shooting Function (shoot_player)

This function adds a new object to the enemy_bullets table.

```
function shoot_player(shooter)
    add(enemy_bullets, {
        x = shooter.x + 4, -- Center bullet on enemy sprite
        y = shooter.y + 8, -- Start bullet just below enemy
        speed = ENEMY_BULLET_SPEED,
    })
end
```

### C. Update and Draw Logic

We must call update_enemy_bullets() and draw_enemy_bullets() in the main loops.

```
function update_enemy_bullets()
    for b in all(enemy_bullets) do
        -- Enemy bullets move down, toward the player
        b.y += b.speed 

        -- Cleanup: Remove if off-screen
        if b.y > 128 then 
            del(enemy_bullets, b)
        end
        
        -- NOTE: Collision check with the player would happen here
    end
end

function draw_enemy_bullets()
    for b in all(enemy_bullets) do
        -- Draw the bullet (e.g., a simple color line or sprite 10)
        line(b.x, b.y, b.x, b.y + 2, 8) -- Draw a small red line
    end
end
```

### D. Integrating Enemy Fire Control

Finally, integrate the shooting logic into the update_enemies() function, using a shoot_timer on the enemy object.

```
function update_enemies()
    -- ... existing movement logic ...
    
    for e in all(enemies) do
        -- Movement logic here...
        
        -- Check if the enemy can shoot (optional: only certain types can fire)
        if e.type == "back_and_forth" then
            e.shoot_timer = (e.shoot_timer or 0) - 1
            
            if e.shoot_timer <= 0 and e.y > 0 then
                shoot_player(e)
                e.shoot_timer = ENEMY_SHOOT_COOLDOWN
            end
        end
        
        -- Cleanup logic here...
    end
end
```

## 3. Death and Explosions Animation

Instead of immediately deleting an enemy upon death, we'll replace it with a temporary explosion object that handles a short animation before being deleted.

### A. Explosion Setup and Spawning

Create a new global table and a function to generate the explosion effect.

```
-- In _init()
explosions = {}
EXPLOSION_DURATION = 15 -- Frames the explosion lasts

function spawn_explosion(x_pos, y_pos)
    add(explosions, {
        x = x_pos,
        y = y_pos,
        timer = EXPLOSION_DURATION,
        frame = 0, -- Used to cycle through animation sprites
    })
end
```

### B. Update and Draw Explosion Logic

The update function ticks the timer down, and the draw function uses the remaining timer/frame count to display the effect.

```
function update_explosions()
    for ex in all(explosions) do
        ex.timer -= 1
        ex.frame += 0.2 -- Increase frame slowly for animation effect
        
        if ex.timer <= 0 then
            del(explosions, ex)
        end
    end
end

function draw_explosions()
    for ex in all(explosions) do
        -- Use a series of sprites (e.g., 12, 13, 14) for the explosion
        local sprite_id = 12 + flr(ex.frame) % 3 
        
        -- Draw sprite, centered, larger scale (w=2, h=2)
        spr(sprite_id, ex.x, ex.y, 2, 2)
    end
end
```

### C. Integrating with Enemy Death

Finally, modify your enemy collision logic (where e.hp reaches zero) to call spawn_explosion before deleting the enemy.

```
-- Example: Logic run after a collision reduces enemy HP to 0
function handle_enemy_death(e)
    -- 1. Spawn the visual effect at the enemy's location
    spawn_explosion(e.x, e.y)
    
    -- 2. Grant the player score (assuming 'score' is global)
    score += e.score_value
    
    -- 3. Remove the enemy
    del(enemies, e)
end
```
