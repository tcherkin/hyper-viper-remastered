# Hyper Viper Remastered

**Bite from the tail. Split the body. Survive the heads.**

Hyper Viper Remastered is a fast maze arcade game in which every collision has a direction, every creature has a temperament, and every safe-looking route can become a trap.

[Play the current browser version](https://tcherkin.github.io/hyper-viper-remastered/)

## From MSX to Palm OS to the browser

The project traces its roots to the classic **Hyper Viper for MSX**. Years later, Georgii Cherkin created his own Palm OS interpretation in NS Basic, with original graphics, sounds, bitmap fonts, levels and additional mechanics.

This browser edition restores that Palm-era visual and audio identity while expanding the game with multiplayer and AI modes, procedurally generated mazes, directional combat, coordinated creature intelligence, touch pathfinding, Power Mode, evolving difficulty and modern performance optimizations.

The game is still in active development. This README documents **v0.99.49.6 - Dead Code Cleanup**, the current optimized development build.

## The objective

Clear each level by eliminating every snake and every fragment created from it.

That sounds simple, but a snake is not removed merely because you touch its body:

- approach the tail and eat it segment by segment;
- cross the middle and the snake splits into two independent creatures;
- catch a head from behind and you can finish it;
- collide with a head from the front and the snake eats you;
- reduce a snake to a solitary head and it becomes smaller, angrier and far more dangerous.

When no vipers remain, every active player receives a **500-point level bonus**, and the next procedurally generated maze begins.

## Game modes

The opening menu supports five ways to play:

| Key | Mode | Players |
|---:|---|---|
| `0` | AI Only | One allied AI player; useful for observation and testing |
| `1` | Solo | One human player |
| `2` | Duo | Two human players |
| `3` | Solo + AI | One human and one allied AI |
| `4` | Duo + AI | Two humans and one allied AI |

Each player has an individual score, life count and Power Mode timer. Teammates cannot pass through or eat one another. A fruit benefits only the player who collects it.

When at least two players are active, the score of the unique leader pulses between 20% and 100% opacity. Nothing pulses when the scores are tied, and a single player's score does not pulse. The HUD life field flashes three times whenever that player earns an extra life.

## Controls

### Keyboard

| Situation | Controls |
|---|---|
| One human player | `WASD` or the arrow keys |
| Two human players | Player 1: `WASD`; Player 2: arrow keys |
| Pause or resume | `P` |
| Choose a game mode | `0` to `4` on the opening or Game Over screen |
| Experimental level jump | Enter `01` to `99`; enter `00` for Level 100 |

The player keeps moving until a wall or solid obstacle blocks the route. A direction command steers the character; it does not move it by a single step.

Holding two perpendicular direction keys activates staircase navigation. The character alternates between the two held directions whenever the maze permits, allowing diagonal and zigzag corridors to be climbed naturally. If one direction is temporarily unavailable, momentum is preserved until the next valid turn.

### Mouse and touch

Click or tap any reachable part of the maze. The central maze brain calculates a shortest route through the actual passages and guides the selected human player to that point.

In two-human modes, the game assigns the command to the player who is closest to the target by **maze distance**, not merely by straight-line screen distance. This makes touch control practical even in complicated labyrinths.

The two-digit level selector is currently retained as a development and balancing tool. It changes the level without changing the selected game mode, scores or remaining lives.

## Understanding the snakes

### Eating the tail

The triangular tail is the safest point of attack. Each eaten tail segment is worth **25 points**. A snake can be consumed all the way down to a single moving head.

Every bite also raises that snake's anger and its memory of the attacking player. Injured creatures become more willing to consult the shared maze intelligence, but their attention still fluctuates instead of remaining permanently fixed.

### Cutting through the middle

Crossing a body segment removes that segment for **10 points** and divides the snake:

1. the original head keeps the front portion;
2. the old tail becomes the head of a new creature;
3. both fragments inherit the original color, temperament, threat memory and anger;
4. both become more alert after the injury.

The split is fully directional. The new head immediately faces according to the old tail's real geometry, including after curved sections, so it never begins with a false one-frame direction.

### Meeting the head

Direction matters:

- a frontal collision is deadly;
- catching a full snake's head while moving in the same direction finishes it for **125 points**;
- a solitary head can be eaten from the side or rear for **125 points**, but its mouth remains dangerous;
- during Power Mode, a player can bite a snake head from the front.

When a powered player eats the head of a longer snake, the remaining body is not erased. The former tail becomes a new head and the surviving fragment escapes in the opposite direction, using the same split logic as a middle cut.

### Retreating snakes and the tail brain

Snakes cannot pass through one another. At a dead end or blockage, a snake retreats at half speed as if an invisible temporary leader were pulling it from the tail. The visible head remains the head while the body moves backward.

The tail now has its own escape logic. During a retreat it searches alternative branches, remembers recently attempted cells and can choose another route instead of repeating the same trapped movement forever. The snake returns to forward motion only after a genuine alternative becomes available from the visible head.

If a retreating tail meets a player or another obstruction, it cannot overwrite or pass through that occupant. The snake tries to escape from its visible head; if neither end has a legal move, it waits.

Solitary heads use the same maze awareness and path memory. Their straight movement, reversals and turns remain visually smooth, but a bite becomes active only after the head has physically turned toward the player.

### Safe level starts

Every new maze validates its player starts and snake placement. Snake bodies must occupy real floor cells, and a snake whose initial head direction would threaten a nearby player start is physically spawned the other way around: the **tail**, not the mouth, faces the player. This prevents unavoidable opening deaths in dead ends without delaying normal respawning.

## Fruit and Power Mode

The scorpion can leave four kinds of fruit:

| Fruit type | Points |
|---|---:|
| Fruit 1 | 50 |
| Fruit 2 | 100 |
| Fruit 3 | 150 |
| Fruit 4 | 200 |

Every fruit starts a fresh **five-second Power Mode**. Collecting another fruit restarts the timer at five seconds; durations do not accumulate.

During Power Mode:

- the collecting player moves at double speed;
- the character becomes brighter;
- enemy hunters can be eaten for **200 points**;
- snake heads can be attacked from the front;
- the final three seconds flash visibly and use warning ticks.

Power Mode rewards active attacks. If a powered player stands still, inhabitants do not automatically disappear into the character. They recognize that tile as dangerous and try to turn away.

## The scorpion

The scorpion is a two-cell creature with a head and tail. It is naturally evasive rather than aggressive, and its knowledge of escape routes improves with the level.

The scorpion:

- spawns only when both of its cells are valid and empty;
- moves through free passages while avoiding players, snakes, hunters and solid eggs;
- leaves a fruit or egg only after its tail has completely vacated the cell;
- never places a fruit on an egg or an egg on a fruit;
- can be eaten for **150 points**;
- returns later after being eaten.

Its general activity rises smoothly to approximately twice the Level 1 rate by Level 10. Long level duration and dwindling snake mass can increase the pressure for another scorpion to appear.

## Eggs and hunters

An egg begins as a background object. For its first **10 seconds**, players and creatures may pass over it, and it is rendered beneath every moving entity.

Incubation follows strict physical rules:

1. an egg cannot begin cracking while a player, snake, scorpion, hunter, fruit or another relevant object covers its cell;
2. its incubation clock waits just before the first crack until the cell is clear;
3. when cracking begins, the egg becomes a solid obstacle;
4. three visible cracking stages follow, one per second, each with a sound;
5. after the last stage, the egg hatches into an enemy hunter.

Hunters use the shared maze brain and have their own temperament and threat memory. They are moderately interested in the players while their scorpion mother is alive. If the scorpion is eaten, all surviving offspring recognize the loss and become substantially more aggressive.

## The central maze brain

All inhabitants share a navigation module that understands the real topology of the current maze. It builds reusable shortest-path distance fields through passages, around walls and around solid eggs. Each snake, hunter and scorpion may consult that knowledge according to its species, temperament, injuries, current mood and intelligence level.

The brain is not simple remote control. Individual character remains important:

- untouched snakes are mostly peaceful but never completely unaware;
- each tail bite raises anger toward the responsible player;
- short snakes are more alert;
- solitary heads have a much higher threat level;
- hunters become angrier when their mother is gone;
- the scorpion uses the brain primarily to flee;
- after a player dies, surviving creatures calm down significantly;
- aggression remembers previous attacks but still rises and falls.

In multiplayer, threat is tracked separately for each player. The inhabitants are more likely to focus on the player who has caused the most provocation, while still reacting to immediate opportunities and danger.

### Progressive collective intelligence

Shared intelligence develops from the beginning of the game:

- Level 1 begins with a faint but real shared awareness;
- every later level raises the baseline coordination;
- the initial calm period becomes shorter;
- tactical learning during the level becomes faster;
- by Level 100, the population begins fully coordinated and uncompromising.

At higher intelligence, attackers stop relying only on direct visual direction. They follow real shortest routes around walls. Groups can assign different approach routes, use opposite flanks and occasionally choose a longer passage for a surprise interception. These ambush routes remain an accent at ordinary levels but become a serious threat in the late game.

The system also detects unproductive local loops. Short snakes and solitary heads are discouraged from endlessly rotating around the same 2x2 structure when a useful route to the players exists.

## Allied AI

The allied character in modes `0`, `3` and `4` uses a specialized hunting system built on top of the central maze brain.

At the beginning of each level, it scans the maze and reuses route information for tactical decisions. It then:

- selects a concrete snake and searches for the fastest route to its tail;
- normally finishes a snake once it starts eating it;
- can switch immediately when another snake becomes clearly safer or more profitable;
- deliberately looks for useful opportunities to split a snake;
- abandons a split when nearby danger makes fast elimination more important;
- seeks nearby fruit as a high-priority Power Mode opportunity;
- during Power Mode, attacks heads and hunters directly and favors rapid clearance over point farming;
- avoids snake mouths and hostile hunters even while committed to another target;
- recognizes temporarily blocked routes and retreats far enough before trying again;
- avoids pointless two-cell oscillation and unnecessary dead ends.

The ally has its own score, lives and Power Mode. It cooperates by clearing the level, but retains a competitive personality and tries to outscore the human players.

## Dynamic encounter pressure

The game prevents a nearly completed level from becoming an endless safe scoring loop.

Encounter pressure combines three factors:

1. **Level:** scorpion activity grows from Level 1 to Level 10.
2. **Time:** the chance of a new scorpion and new eggs begins rising after a calm period. Above Level 10, that calm period becomes progressively shorter and the rise becomes faster.
3. **Snake depletion:** as the remaining snake body mass approaches zero, scorpion and egg pressure can gain an additional multiplier of up to approximately **2.25x**.

The first possible egg from a scorpion retains its original timing. After an egg has been laid, repeat egg attempts occur at half the former frequency to keep the busier procedural mazes fair. Fruit production keeps its own rhythm.

## Procedural maze progression

Every level receives a new connected maze. The generator creates one candidate directly, avoiding the old pause that could occur when several candidates were evaluated as the final snake disappeared.

Every generated maze must satisfy core rules:

- all walkable cells belong to one connected network;
- every player start is reachable from the rest of the maze;
- starting areas provide escape routes without becoming wide empty rooms;
- walls, point obstacles and diagonal structures remain compatible with the 16x16 sprite system;
- wide open spaces are deliberately limited so snakes cannot rotate freely everywhere.

### Progressive absurdity

The generator contains an **absurdity** parameter that changes both difficulty and visual character:

- early levels favor narrow, readable corridors and stronger symmetry;
- continuous staircase and diagonal corridors become longer and more frequent;
- diagonal structures gradually spread from the center into one or more corners;
- isolated point structures are divided approximately 50/50 between diagonal patterns and horizontal or vertical rows;
- later point rows can be approached and circled from multiple sides;
- symmetry gradually breaks down and local open zones become more dangerous;
- diagonal corridors begin one cell wide and gain a growing chance of wider variations toward Level 100.

Absurdity is almost invisible at Level 1, reaches approximately 98% at Level 30, and continues to grow aggressively rather than plateauing. Level 100 is intentionally extreme, dense and close to impossible, while still obeying the full-connectivity rules.

### Maze colors

Level 1 always uses the original green maze. Later levels choose from green, orange, blue-violet, ruby red, turquoise and gold. A new level avoids the colors used by the previous two levels and prefers strong hue contrast, so progression remains visually distinct.

## Population and speed balance

| Progression | Effect |
|---|---|
| Level 1 | 3 snakes with broadly similar lengths |
| Early levels | The population grows gradually |
| Level 11 | The maximum population of 9 snakes is reached |
| Later levels | Population remains at 9, with more varied but balanced lengths |
| Average length | Gradually falls from about 13 cells at 3 snakes to 7 cells at 9 snakes |
| Movement speed | Remains constant across levels |

Short and long snakes are paired around the target average, so a very short late-level snake is balanced by a longer one. Difficulty grows through maze structure, population, intelligence and encounter pressure - not through an endlessly accelerating movement timer.

## Score and extra lives

| Event | Points |
|---|---:|
| Cut a snake through the middle | 10 |
| Eat one tail segment | 25 |
| Eat a snake head | 125 |
| Eat the scorpion | 150 |
| Eat a hunter during Power Mode | 200 |
| Eat fruit | 50 / 100 / 150 / 200 |
| Complete a level | 500 for each active player |

Each player earns **one extra life every 5,000 points**. The rhythm never changes, so the next bonus is always predictable. No player can hold more than **9 lives**. Thresholds reached while already at the cap are consumed rather than stored as a hidden backlog.

## Death, respawning and spawn protection

Human Player 1 begins at the upper-left start, faces downward and waits for the first command before moving. Other players use their own separated starts.

After death:

- the original skeleton sprite remains and pulses for three seconds;
- the world continues moving;
- the player respawns immediately after that sequence as soon as the starting cell is clear;
- the player receives a three-second flashing spawn shield;
- the shield provides invulnerability only: it does not increase speed, enable frontal head bites or allow hunters to be eaten;
- unsafe inhabitants treat the protected player as a temporary obstacle and turn away.

When every active player has run out of lives, the Game Over menu appears and a new mode can be selected with `0` to `4`.

## Original presentation and modern rendering

The browser build preserves the visual and audio language of the Palm OS edition:

- original 16x16 character, snake, scorpion, egg, fruit and maze sprites;
- original bitmap letters and numbers for every in-game message and HUD element;
- original sound library, including randomized body-bite and fruit sounds;
- smooth arcade-style movement between logical grid cells;
- gentle pulses for Pause, Game Over, mode selection, leading scores and earned lives;
- gradual cached shading from each snake head toward its darker tail;
- cached head-to-tail shading for the scorpion;
- subtle depth shading for maze segments and bitmap text;
- all graphics, fonts, sounds and render atlases embedded in one standalone HTML file.

The game therefore runs directly in a modern browser without installation or external asset folders.

## Performance and mobile optimization

The current build is designed to remain responsive on desktop browsers and iPhone Safari:

- sprite gradients and visual variants are stored in prebuilt atlases rather than generated during play;
- the maze is rendered to a reusable static layer and rebuilt only when the level or color changes;
- HUD glyphs are redrawn only when their values or animation state require it;
- shortest-path maps and occupancy data are reused across AI decisions;
- repeated sound effects use prepared audio pools;
- the hottest snake-eating collision paths avoid unnecessary temporary objects;
- high-refresh displays are prevented from performing redundant 120 Hz game work;
- obsolete dynamic atlas builders, duplicate maze-generator experiments and unused helper functions have been removed.

The cleanup changes only implementation cost. The established movement, collision, AI, scoring and visual rules remain intact.

## Credits

- **Original MSX inspiration:** Hyper Viper
- **Palm OS adaptation, game design, graphics, fonts and sounds:** Georgii Cherkin
- **Browser restoration and expanded design:** Georgii Cherkin
- **Development assistance:** OpenAI Codex

Hyper Viper Remastered is an independent tribute and an evolving restoration project.
