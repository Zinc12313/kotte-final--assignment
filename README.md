# Bomberman World — Kotte Blast Arena

Student: Tafheem Ahmed Tajwar

## How to play

**Controls:** Move with `WASD` or the arrow keys. Place a bomb with `Space`. Restart the round with `R`. Quit with `Q` or `Escape`.

**Objective and rules:**
- Win by defeating all enemies before the 5:00 timer ends.
- Lose if you touch an enemy, stand in a blast, or the timer reaches 0:00.
- If death and defeating the last enemy happen in the same frame, **Lost** wins.
- You can place up to 3 bombs at once, one per tile. Bombs explode after 2 seconds and can set off other bombs.
- Blast range starts at 3. The pickup adds 1, with a maximum of 6.
- Purple chaser enemies follow you inside 250 px and stop beyond 350 px. Pink enemies wander.

**Configurations / demo setup:** Default. One fixed layout from a fixed seed (shown on screen). No debug keys.

## Features and validation

| Feature | Check, observed result, and expected behaviour |
| :--- | :--- |
| 1. Player damage and death | I touched an enemy in one round and stood in a blast in another, and also stood one tile from a blast. Both hits killed me once and the state became `Lost`. The near miss did nothing. The round stayed `Lost` until `R`. This matched what I expected. |
| 2. Round states and objective | I let the timer run out in one round and killed all enemies in another. The HUD showed `Lost` or `Won` with enemies left, and movement and bombs stopped. Death is checked before the win in `apply_structural_mutations`, so `Lost` wins. I did not reproduce a simultaneous death and win in play. |
| 3. Safe restart | I pressed `R` after a win, after a loss, and while bombs and blasts were active, several times in a row. The entity count went back to its starting value and no old bombs or blasts appeared. Range and bomb count reset. This matched what I expected. |
| 4. Solid bombs with owner escape | I placed a bomb, stepped off, and could not walk back on. After it exploded I could walk through. Pressing `Space` repeatedly on one tile made only one bomb. I could walk off my own bomb as expected. |
| 5. Blast-range pickup | I walked near the pickup, collected it, then stepped on the spot again. Range went from `3/6` to `4/6` once. A bomb placed before kept range 3, and one placed after used range 4. This matched what I expected. |
| 6. Multiple bombs per owner | I placed bombs on several tiles and tried a fourth, which was refused (`3/3`). After one exploded I placed another, and the count stayed correct when I repeated this. |
| 7. Chain reactions | I placed two bombs close together at different times. The first blast set off the second before its fuse ended, and the second did not explode again later. This matched what I expected. |
| 9. A second enemy behaviour | I went within 250 px of a chaser near crates, then moved past 350 px. It followed me without passing through crates or walls, then went back to wandering without flickering. This matched what I expected. |

**Known issues:**
- `EntityStore::clear()` resets generation numbers, so restart is safe only because `restart_round` clears every stored handle first.
- A crate's collision box sits a little above its tile, so in rare spots a bomb can be placed overlapping a crate.
- The maximum range of 6 cannot be reached with only one pickup.

## Sources and assistance

This project builds on the shared Kotte foundations.

- **AI assistance:**
  I used  Gemini to review my code against the assignment brief and my teacher's feedback, and to identify bugs (for example, enemies and the player could not overlap, so enemy contact could not kill). I reviewed and tested every change myself.