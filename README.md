# Hyper Viper Remastered

**Bite from the tail. Split the body. Survive the heads.**

Hyper Viper Remastered is a fast maze arcade game in which every collision has a direction, every creature has a temperament, and every safe-looking route can become a trap.

[Play the current browser version](https://tcherkin.github.io/hyper-viper-remastered/)

## From MSX to Palm OS to the browser

The project traces its roots to the classic **Hyper Viper for MSX**. Years later, Georgii Cherkin created his own Palm OS interpretation in NS Basic, with original graphics, sounds, fonts, levels and additional mechanics.

This browser edition restores that Palm-era visual and audio identity while expanding the game with more expressive movement, evolving creature behavior, tactical navigation, power fruit, dynamic difficulty and new interactions inside the maze.

The game is still in active development. The current public build is **v0.98.54 — Evolving Coordinated Brain**.

## The objective

Clear each level by eliminating every snake and every fragment created from it.

That sounds simple, but a snake is not removed merely because you touch its body:

- approach the tail and eat it segment by segment;
- cross the middle and the snake splits into two independent creatures;
- catch a head from behind and you can finish it;
- collide with a head from the front and the snake eats you;
- reduce a snake to a solitary head and it becomes smaller, angrier and far more dangerous.

When no vipers remain, the level is complete, a 500-point bonus is awarded, and the next level begins with a more demanding population.

## Controls

| Input | Action |
|---|---|
| Arrow keys or `WASD` | Change direction |
| Mouse click or screen tap | Move toward the selected point |
| `P` | Pause or resume |
| `R` | Start a new game |

The player keeps moving until a wall or a solid obstacle stops the route. A direction command therefore steers the character; it does not move it by a single step.

### Click and touch navigation

A click or tap may indicate a straight or diagonal destination. For a diagonal target, the game uses only the two requested directions and prioritizes the axis that dominates the gesture. If the preferred turn is not yet available, the character preserves its momentum and takes the first suitable passage instead of stopping abruptly.

## Understanding the snakes

### Eating the tail

The triangular tail is the safest point of attack. Each tail segment is worth **25 points**. A snake can be eaten all the way down to a single moving head.

Every bite also increases that snake's anger. It becomes more willing to use the shared maze intelligence to find the player, although its aggression still rises and falls rather than remaining permanently at maximum.

### Cutting through the middle

Crossing a body segment removes that segment for **10 points** and divides the snake:

1. the original head keeps the front portion;
2. the old tail becomes the head of a new creature;
3. both fragments inherit the original color, temperament and memory;
4. both become more aggressive after the injury.

The rule also applies at the extremes. An original head may survive alone, and even a lone triangular tail can transform into a new solitary head.

### Meeting the head

Direction matters:

- a frontal collision is deadly;
- catching a full snake's head while moving in the same direction finishes that snake and awards **125 points**;
- a solitary head can be eaten from the side or rear for **125 points**, but its mouth remains dangerous;
- during Power Mode, the player can bite a snake head from the front.

When a powered player eats the head of a longer snake, the remaining body is not simply erased. It reverses its identity: the former tail becomes a new head and the surviving fragment escapes in the opposite direction.

### Retreating snakes

Snakes do not pass through one another. At a dead end or blockage, a snake can retreat at half speed, as if an invisible temporary leader were pulling it from the tail. It continues backward until the visible head reaches a genuine alternative junction.

The retreat is deliberately imperfect. A snake may ignore a valid escape for a moment, continue too far, or change its mind. If the tail encounters the player or another obstruction, the snake attempts to escape from its original head. It waits only when neither end has a legal move.

Solitary heads remember their recent path and use it to reverse out of dead ends. They retain the slightly foolish personality of the original design, but their movement and turns are visually smooth.

## Fruit and Power Mode

The scorpion can leave four kinds of fruit. They award:

| Fruit type | Points |
|---|---:|
| Fruit 1 | 50 |
| Fruit 2 | 100 |
| Fruit 3 | 150 |
| Fruit 4 | 200 |

Every fruit activates **Power Mode for 10 seconds**:

- the player moves at double speed;
- the character becomes brighter;
- enemy hunters can be eaten for **200 points**;
- snake heads can be attacked from the front;
- the final three seconds flash visibly and are accompanied by warning ticks.

Power Mode rewards active attacks. If the player stands still, creatures do not automatically fall into the character's mouth. They recognize the powered player as a dangerous obstacle and attempt to turn away.

## The scorpion

The scorpion is a two-cell creature with a head and tail. It is not a direct hunter: its natural instinct is to avoid the player, and its understanding of the maze improves with every level.

The scorpion:

- travels through free passages without occupying walls or other creatures;
- leaves a fruit or egg only after its tail has completely left the cell;
- never places a fruit on an egg or an egg on a fruit;
- can be eaten for **150 points**;
- returns later after being eaten.

Its activity rises gradually with the level and reaches approximately twice the initial rate by Level 10.

## Eggs and hunters

An egg begins as a background object. For its first **10 seconds**, creatures and the player may pass over it, and it is rendered underneath them.

The incubation follows strict physical rules:

1. an egg cannot begin cracking while the player, a snake, the scorpion, a hunter, a fruit or another relevant object covers its cell;
2. once the cell is clear, the first crack appears and the egg becomes a solid obstacle;
3. three cracking stages follow, one per second, each with its own sound;
4. after the final stage, the egg hatches into a hunter.

Hunters move independently and use the same shared navigation intelligence as the snakes. They are moderately interested in the player while their scorpion mother is alive. If the scorpion is eaten, all of its surviving offspring recognize the loss and become substantially more aggressive.

Hunters are dangerous in normal play, but can be eaten during Power Mode.

## The evolving maze brain

The inhabitants share a central navigation system that understands the actual passages of the maze. It builds shortest-path distance maps around walls and solid eggs, then offers each creature a route based on its current behavior.

The brain is advice, not remote control. Every inhabitant still has its own temperament, attention cycle and occasional poor decision.

### Aggression is never permanently 100%

Aggression is modulated over time:

- an untouched snake is mostly peaceful, but may occasionally become curious;
- every eaten segment increases the injured snake's anger;
- short snakes are more alert;
- a solitary head has a much higher general threat level;
- hunters become more aggressive when their mother is gone;
- after the player loses a life, surviving creatures calm down significantly;
- injured snakes begin to cool after roughly 20–30 seconds, while retaining part of the memory of the attack.

Even the most furious inhabitant has moments of distraction. This prevents the game from becoming a predictable procession of enemies following exactly the same path.

### Intelligence evolves with time

For the first 30 seconds of a level, enemies use the familiar mixture of instinct, temperament and occasional brain guidance. After 30 seconds, the tactical influence grows smoothly. By approximately three minutes, attackers have access to the strongest version of their maze intelligence.

- a lone attacker increasingly favors the true shortest route;
- groups of at least three can coordinate center, left-flank and right-flank roles;
- flankers may deliberately accept a slightly longer route to approach from another passage;
- surprise detours and ambushes remain possible, but are uncommon rather than constant.

The result is a level that gradually becomes more dangerous if the player tries to remain indefinitely and farm easy points.

## Dynamic encounter pressure

The game also increases the pressure to produce a new scorpion and new eggs.

- the first 30 seconds keep the normal frequency;
- immediately after 30 seconds, the time multiplier becomes 2×;
- it then continues to rise without a hard cap;
- the fewer snake body cells remain, the stronger the additional pressure becomes;
- almost complete snake depletion can add up to approximately 2.25× on top of the time and level multipliers.

Fruit production keeps its own normal rhythm. The increased egg and scorpion pressure specifically prevents a nearly completed level from becoming an endless safe scoring loop.

## Level progression

| Progression | Effect |
|---|---|
| Level 1 | 3 snakes, with broadly similar lengths |
| Each new level | Adds one snake until the population reaches 9 |
| By Level 7 | 9 snakes with an average length of 7 cells |
| Later levels | Keep the same average mass but use more varied, balanced lengths |
| Every level | Player, snakes, hunters and scorpion become faster |
| Speed curve | Approximately 9% additional speed per level, with no late-game cap |

Later snake groups contain both shorter and longer individuals. A very short snake is balanced by a longer one so the total body mass remains fair rather than changing randomly.

## Score and extra lives

| Event | Points |
|---|---:|
| Cut a snake through the middle | 10 |
| Eat one tail segment | 25 |
| Eat a snake head | 125 |
| Eat the scorpion | 150 |
| Eat a hunter during Power Mode | 200 |
| Eat fruit | 50 / 100 / 150 / 200 |
| Complete a level | 500 |

Extra lives follow an escalating schedule:

- the first five extra lives are earned at 1,000-point intervals;
- the next five are earned at 2,000-point intervals;
- every later extra life requires another 5,000 points.

## Death and respawning

The player begins at the upper-left starting position, facing downward, and waits for the first command before moving.

After death:

- the original skeleton sprite remains and pulses for three seconds;
- the world continues moving;
- the player does not respawn until the starting cell is clear;
- a three-second flashing spawn shield protects the new life;
- the shield does not increase speed and does not grant Power Mode attacks.

When no lives remain, the Game Over screen pulses gently and `R` starts a new game.

## Original presentation

The browser build preserves the visual and audio language of the Palm OS edition:

- original 16×16 character, snake, scorpion, egg, fruit and maze sprites;
- original bitmap letters and numbers for every in-game message and HUD element;
- original sound effects, including multiple randomized body-bite and fruit sounds;
- pixel-perfect logical movement with fast arcade-style visual transitions;
- all graphics, fonts and sounds embedded into a single standalone HTML file.

The game can therefore run directly in a modern browser without installation or external asset folders.

## Credits

- **Original MSX inspiration:** Hyper Viper
- **Palm OS adaptation, game design, graphics, fonts and sounds:** Georgii Cherkin
- **Browser restoration and expanded design:** Georgii Cherkin
- **Development assistance:** OpenAI Codex

Hyper Viper Remastered is an independent tribute and evolving restoration project.
