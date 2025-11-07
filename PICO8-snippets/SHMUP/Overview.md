# SHMUP Overview

The SHMUP (Space Shooter/Scroller) genre focuses heavily on reaction time, projectile management, and clearing waves of enemies. Movement is typically non-stop, requiring the player to maintain direct, instant control of their craft.

The main goal is survival, maximizing score, and progressing through a constantly scrolling threat environment.

## 1. Player Setup

Fixed Speed: Unlike platformers, the player usually moves at a constant, fixed speed defined by a speed variable. The player does not accelerate or decelerate.

Fire Rate & Health: The player is defined by their rate of fire (controlled by a cooldown timer) and Health Points (HP), which act as a simple damage buffer against enemy attacks.

Minimal Stats: There are often no complex leveling systems; progression is usually handled by acquiring temporary power-ups that increase firepower or speed.

## 2. Level Design

Constant Movement: The level constantly scrolls (vertically or horizontally), creating the illusion that the player is moving through an infinite environment.

Boundary Management: The map is limited to the screen boundaries. The player cannot leave the screen, which simplifies movement and collision code.

Spawn Patterns: Levels are defined by pre-planned enemy wave patterns and timed events (like boss encounters or asteroid fields) that appear from the edges of the screen.

## 3. Movement

Direct Control: Movement is instant and simple. Pressing a direction button immediately moves the ship by the fixed speed value. When the button is released, the movement stops immediately.

No Physics: There is no gravity, friction, or momentum, making the controls feel highly responsive and arcade-like.

Boundary Check (Clamping): The core movement function must use a clamping technique to ensure the player's position is always locked between the minimum (0) and maximum (128 - sprite width) edges of the screen.

## 4. Combat

Collection Management: Combat involves managing two main collections (arrays) of objects: the player's outgoing blasters and the incoming enemies (or enemy projectiles).

Single-Press Fire: Shooting is typically handled with a btnp check for a single press, often paired with a cooldown timer to prevent machine-gun fire, balancing the game.

Hit Detection: The primary logic challenge is checking for collision between objects in one collection (e.g., blasters) against objects in another collection (enemies).

## 5. Timers & Spawning

Fire Cooldowns: A small frame counter (fire_cooldown) is essential for limiting the player's offensive power and forcing them to manage their shooting rate.

Enemy Spawners: Enemy waves are triggered by a game timer or score threshold, which dictates when new enemy objects are created and added to the enemies collection.

Blaster Cleanup: Blaster objects must be quickly removed from the blasters collection once they fly off-screen or hit an enemy, or the game will slow down due to too many active objects.

---

Key Mechanics (Code Systems Checklist)

| Mechanic | Code Focus |
| :--- | :--- |
| **Player Setup** | Defining initial variables (`x`, `y`, `speed`, `cooldown`, etc.). |
| **Movement** | Simple input handling combined with screen boundary **clamping**. |
| **Blasting** | Creating a new projectile object and adding it to an array (`blasters`). |
| **Timers & Cooldowns** | Frame counters for player fire rate and enemy spawning frequency. |
| **Combat** | Collision logic between the `blasters` array and the `enemies` array. |
| **Cleanup** | Removing blasters and enemies from their arrays when they go off-screen or die. |
| **Level Design** | A game-time counter to spawn enemy waves according to a defined pattern. |
