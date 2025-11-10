Space Shooter Game Plan and To-Do List

This is your master plan! It shows you how the game is built, what lists we use to keep track of everything, and the steps we still need to finish.

I. How the Game Works Every Second (The Game Loop)

Think of your game code like a movie director who follows three steps over and over again, many times every second:

_init() (The Setup Crew): This only runs once when the game starts. It gets the player ready, creates all the main lists, and sets the starting score to zero.

_update() (The Action Director): This is the core of the game. It handles all the action: moving the player, making enemies fly, shooting bullets, and, most importantly, checking for crashes (collisions).

_draw() (The Artist): This paints everything on the screen so you can see it. It draws the player, the enemies, the bullets, and your score.

Key Lists We Use (Data Collections)

We use these lists to store all the moving parts of the game while it’s running:

## Key Lists We Use (Data Collections)

| List Name | What it Holds | Reference Document |
| :--- | :--- | :--- |
| **`enemies`** | A list of all the bad guys currently flying on the screen. | `enemies.md`, `advanced_enemies.md` |
| **`player_bullets`** | A list of every shot fired by the player's ship. | Core Game Logic |
| **`enemy_bullets`** | A list of every shot fired by the enemies. | `advanced_enemies.md` (Sec 3) |
| **`explosions`** | A list of little visual puffs to show when something blows up. | `advanced_enemies.md` (Sec 4) |
| **`player`** | The most important item: your ship's location, health, and speed. | Core Game Logic |

---

II. The Game Development To-Do List

Use this checklist to track our progress. The right column tells you exactly which file has the instructions or notes for that part!

A. Game Setup (Getting Started)

## Game Setup and Player/Basic Enemy Checklist

| Status | Feature | Where to Find the Code |
| :---: | :--- | :--- |
| [ ] | **Main Lists Ready** (`enemies`, `player_bullets`, etc.) set up in the `_init()` function. | Core Game Logic |
| [ ] | **Director Functions Ready** (`_init()`, `_update()`, `_draw()`) are built. | Core Game Logic |
| [ ] | **Game Screens** (Title screen, Playing screen, Game Over screen) are set up. | Core Game Logic |

B. Player Ship System
| Status | Feature | Where to Find the Code |
| :---: | :--- | :--- |
| [ ] | **Player Ship** (`player`) created with starting health and speed. | Core Game Logic |
| [ ] | **Player Movement** (Moving the ship left and right with keyboard/touch). | Core Game Logic |
| [ ] | **Player Shooting** (Making the ship fire bullets after a short delay). | Core Game Logic |

C. Basic Bad Guys
| Status | Feature | Where to Find the Code |
| :---: | :--- | :--- |
| [ ] | **Enemy Stats** (Each enemy has a location, health, and score value). | `enemies.md` |
| [ ] | **Enemy Spawner** (Making new enemies appear randomly). | `enemies.md` |
| [ ] | **Simple Movement** (Bad guys just fly straight down the screen). | `enemies.md` |
| [ ] | **Clean Up** (Removing enemies that fly off the bottom of the screen). | `enemies.md` |

D. Advanced Bad Guys

E. Collision Detection
