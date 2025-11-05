# function _init()

This function is your game's initialization phase. It runs ONLY ONCE before the main game loop begins. Everything you need to start the game—the rules, the players, and the status—must be created here.

## Purpose and Use

| Purpose | What it does | Analogy | 
 | ----- | ----- | ----- | 
| **Define Game State** | Creates variables to track game status, such as **score**, **lives**, **map level**, and **difficulty** settings. | Setting the stage for a play: defining the rules and writing the opening scene. | 
| **Setup Game Objects** | Defines the structure and starting positions for all the game elements, including the **player**, **enemies**, and **pickups**. | Gathering all the actors, assigning them their roles, and placing them on their starting marks. | 
| **Initialize Collections** | Creates tables (`{}`) that will be used to store all the information about objects that. | Making empty storage crates for "enemies to kill" and "blasters to fire" that will be filled during the game. | 
| **Define Constants** | Sets fixed values like `BLASTER_SPEED`, `GRAVITY`, and `JUMP_HEIGHT` that won't change during the game. | Setting the unbreakable physical laws of your game world. |

---

## Example Code

All variables and tables you need to use later in your game must be defined here first.

```
function _init()
    -- Global Variables (Game State Tracking)
    score = 0
    lives = 3
    game_level = 1
    
    -- Player Object Setup (The structure for the main character)
    player = { 
        x=64, 
        y=64, 
        vx=0, 
        vy=0,
        health=100
    }
    
    -- Empty Tables (Collections of dynamic objects)
    blasters = {}
    enemies = {}
    
    -- Constants (Fixed values for physics and balance)
    BLASTER_SPEED = 5
    GRAVITY = 0.2
end

```
