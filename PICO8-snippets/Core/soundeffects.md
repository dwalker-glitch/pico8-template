# PICO-8 Sound Effects (SFX)

Adding sound effects is key to making your game feel responsive and fun. PICO-8 makes this very simple using the built-in sfx() function.

## The sfx() Function

The sfx() function is what you use to tell PICO-8 to play a specific sound that you have designed in the sound editor.

| **Function** | **Code** | **Purpose** | 
| **`sfx(n)`** | `sfx(1)` | Plays sound effect number **`n`** once. | 
| **`sfx(n, channel)`** | `sfx(1, 0)` | Plays sound effect number **`n`** on a specific channel (`0` to `3`). |

### Key Concepts

Sound ID (n): This is the number (0 to 63) corresponding to the sound effect you created in the PICO-8 sound editor.

Channels (0-3): PICO-8 has only 4 channels, meaning only 4 sounds can play at the exact same moment. If you don't specify a channel, PICO-8 finds the first available one.

---

## Where to Call sfx()

You should call the sfx() function at the exact moment the action happens in your code. This means calling it from inside the _update() function or another function called by _update().

Example: Jump Sound

To play a sound when the player jumps, you place the sfx() call right after the condition that checks for a jump:
```
-- In your move(o) function or similar
if (btnp(4)) then
    if is_on_ground(o) then
        o.vy = jump_height
        sfx(2) -- Play Sound Effect ID 2 (e.g., a 'boing' sound)
    end
end
```

Example: Blaster Sound

To play a sound when the player fires, you place the sfx() call inside the fire_blaster() function:

```
function fire_blaster()
    if btnp(5) then
        add(blasters, {
            -- new blaster object properties
        })
        sfx(1) -- Play Sound Effect ID 1 (e.g., a 'pew' sound)
    end
end
```
