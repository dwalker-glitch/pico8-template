# function _update() The Brain

This function is the engine of your game. It runs every single frame (30 or 60 times per second). This is where all the movement, input, and game logic lives.

IMPORTANT: Nothing you write here should be visible to the player. It only calculates the new state of the game.

## Purpose and Use

| **Purpose** | **What it does** | **Analogy** | 
| **Input** | Checks which buttons the player is pressing (e.g., jump, shoot). | Listening to the director's call for "Action!" or "Cut!" | 
| **Movement** | Moves the player, enemies, and blasters by changing their `x` and `y` values. | Moving the characters and objects on the stage behind the scenes. | 
| **Handle Collisions** | Determines if the player is touching an enemy, a pickup, or a wall. | Checking the script for dramatic events like a fight or finding a treasure. | 
| **Manage Status** | Lowers player health, updates the score, or checks if the game is over. | The director keeps track of time, health, and points on a clipboard. | 
| **Game Flow** | Spawns new enemies, moves to the next level, or runs a win/lose condition. | The script telling the actors and objects what happens next. |

---

## Example Code

Any function that changes the game state (like move(o) or update_blasters()) must be called here.
```
function _update()
    -- Handle Player Movement and Physics (Gravity, Jump, etc.)
    move(player)
    
    -- Update all active blasters (makes them move up and removes old ones)
    update_blasters()
    
    -- Check if any blasters have hit an enemy (collision logic)
    check_for_collisions()
    
    -- Update enemy positions or logic
    update_enemies()
end
```
