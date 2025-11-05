# function _init()

This function is your game's initialization phase. It runs ONLY ONCE before the main game loop begins.

## Purpose and Use

| Purpose | What it does | Analogy | 
 | ----- | ----- | ----- | 
| **Start Values** | Sets initial scores, player health, and position variables. | Gathering all the actors, props, and costumes before the camera rolls. | 
| **Setup Tables** | Creates empty tables (arrays) for things like blasters, enemies, or power-ups. | Making an empty list for "enemies to kill" and "blasters to fire." |

---

## Example Code

All variables and tables you need to use later in your game must be defined here first.

```
function _init()
    -- Global Variables
    score = 0
    lives = 3
    
    -- Player Object Setup
    player = { x=64, y=64, vx=0, vy=0 }
    
    -- Empty Tables (Collections)
    blasters = {}
    enemies = {}
    
    -- Constants
    BLASTER_SPEED = 5
end
```
