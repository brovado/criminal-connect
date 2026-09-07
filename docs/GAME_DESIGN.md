# Criminal Connect — Game Design Document

**Status:** Prototype specification  
**Version:** 0.1  
**Genre:** Single-player puzzle / roguelike / engine builder  
**Core inspiration:** Connect Four + roguelike run structure + board-game resource engines

---

## 1. High Concept

**Criminal Connect** is a short-session roguelike built around a persistent Connect Four board.

The player controls two intertwined criminal partners, represented by **Blue** and **Red** pieces. They are mechanically equal: neither color has an inherent resource advantage or special rule.

Each round, the player alternates placing Blue and Red pieces into the board. Connections are not a race against an opponent. Instead, completed connections become part of a growing economic engine that generates resources at the end of each round.

The player's long-term goal is to keep both partners alive while accumulating enough **Research / Intel** to complete the current objective. Upgrades introduce the unusual rules, synergies, multipliers, and build identities that make each run different.

### Design Pillar

> **Build the criminal operation, then keep it alive long enough to pull off the impossible score.**

---

## 2. Core Player Experience

The intended feeling is:

1. **Place a piece.**
2. See a possible connection forming.
3. Realize that a resource node can be incorporated into that connection.
4. Decide whether to extend an existing line or start a new one.
5. End the round and watch the board pay out.
6. Spend the resulting resources on upgrades.
7. Return to the increasingly valuable persistent board.
8. Slowly turn a simple Connect Four grid into an absurd criminal machine.

The player should frequently experience the thought:

> "If I can just extend this line one more space, this whole thing becomes much more valuable."

---

## 3. The Two Partners

### Blue

One member of the criminal pair.

### Red

The other member of the criminal pair.

### Mechanical Rule

**Blue and Red are fundamentally equal.**

Neither color inherently produces different resources, gets different placement rules, or has different base abilities.

All asymmetry comes from **upgrades, board state, special tiles, and temporary effects**.

This is intentional. The player should be thinking about the relationship between the two pieces rather than memorizing two character classes.

### Narrative Direction

The pair should evoke the archetype of:

- Bonnie & Clyde
- Butch Cassidy & Sundance
- outlaw lovers
- partners in crime
- two fugitives against the world

They should feel like two halves of the same operation: equal, dependent, and better together.

The final names, setting, era, and exact story are deliberately undecided.

---

## 4. Board

### Base Board

The core board behaves like a Connect Four board:

- Fixed columns.
- Pieces fall downward into available spaces.
- Each placement occupies a permanent cell.
- There is no opponent.
- The player controls both colors.
- The board persists between rounds.

### Persistent Board Principle

**The board does not reset after scoring.**

Connections remain part of the board and can contribute to future rounds.

This is the defining difference from ordinary Connect Four.

The board becomes a persistent engine rather than a sequence of disposable puzzles.

### Initial Board State

A run may begin with a partially populated board containing special resource nodes, obstacles, or other predefined features.

Examples:

- Coin
- Medicine
- Research
- Energy
- Empty cells
- Blocked cells / obstacles
- Future wildcard or special nodes

Procedural starting layouts are a major replayability lever.

---

## 5. Actions / Medicine

**Medicine** is currently the working name for the resource that determines how many placements the player receives during a round.

A more thematically appropriate name may be selected later (for example, **Favors**, **Connections**, or **Pull**).

### Base Loop

The simplest prototype can use a fixed starting number of actions per round.

Each action consists of:

1. Place Blue.
2. Place Red.
3. Repeat while the player has available placements.

The exact starting action count is a balancing variable.

### Board Resource Interaction

A resource node embedded in a completed connection may increase future action generation.

Example:

> A connection passes through a Medicine/Favor node → future rounds gain additional placement capacity.

The node is **not removed** when scored. Its value becomes part of the persistent board engine.

---

## 6. Resources

The initial resource set is deliberately small.

| Resource | Purpose |
|---|---|
| **Coin** | Currency used to purchase upgrades. |
| **Medicine** | Determines how many placement actions are available. Working name; likely renamed for theme. |
| **Research** | Progress toward completing the round/objective. |
| **Energy** | Survival resource. Both partners depend on it. |

### Resource Philosophy

Resources are not inherently tied to Blue or Red.

A connection generates resources based on:

- its length,
- resource nodes contained within it,
- upgrades affecting it,
- special board conditions,
- and other future modifiers.

---

## 7. Connections

Connections are the primary scoring structure.

A connection is a contiguous sequence of the same color in one of the standard Connect Four directions:

- Horizontal
- Vertical
- Diagonal rising
- Diagonal falling

### Minimum Connection

**4 matching pieces** creates a valid connection.

### Growing Line Value

Longer connections must be significantly more valuable than the minimum.

Prototype baseline:

| Length | Suggested multiplier |
|---:|---:|
| 4 | x1 |
| 5 | x2 |
| 6 | x4 |
| 7 | x8 |
| 8+ | Continue scaling / special handling |

The exact curve is a tuning parameter.

### Important Rule

A long connection should reward planning without making every shorter connection feel worthless.

The player should constantly weigh:

> Take the guaranteed 4 now, or spend actions trying to extend the line?

### Persistent Scoring

At the end of each round, the game evaluates the board and determines the active connections.

Connections continue to exist afterward.

The implementation must define how previously scored connections are prevented from paying infinitely every round unless that is explicitly intended.

Possible models:

1. **Permanent generators:** a completed connection produces every round.
2. **Fresh-completion scoring:** a connection only pays when newly completed.
3. **Generator activation:** the first completion turns it into a permanent generator.
4. **Decay / charge:** a generator pays for a limited number of rounds before needing reinforcement.

**Prototype recommendation:** test model #3 first. It best matches the intended persistent engine-building fantasy while keeping scoring comprehensible.

---

## 8. Resource Nodes

Special resources can occupy board cells before or during a run.

A connection passing through a node incorporates that node into its output.

### Coin Node

A connection containing Coin generates additional Coin.

The Coin node remains on the board.

### Medicine Node

A connection containing Medicine generates additional placement capacity for future rounds.

The node remains on the board.

### Research Node

A connection containing Research generates Research toward the current objective.

### Energy Node

A connection containing Energy generates Energy needed to keep the pair alive.

### Key Principle

**Resource nodes are infrastructure, not consumables.**

The player is building a network of productive locations.

Potential future rule:

> A node may change state after being activated rather than disappearing.

For example, a Coin node could visually become an established cash operation once connected.

---

## 9. Round / Day Structure

A run is divided into repeating rounds. The thematic term may eventually become **Days**, **Jobs**, **Scores**, or another criminal-themed term.

### Proposed Round Flow

**1. Start Round**

- Restore / calculate available placement actions.
- Apply persistent generators.
- Check Energy state.

**2. Placement Phase**

- Player places Blue and Red according to available actions.
- Board changes permanently.
- Newly completed connections are identified.

**3. Resolution**

- Score newly completed connections.
- Apply connection multipliers.
- Activate resource nodes.
- Generate Coin, Medicine, Research, and Energy.

**4. Survival Check**

- Apply the round's Energy requirement.
- If Energy is insufficient, one or both partners may be at risk.
- If the survival condition is failed, the run ends.

**5. Victory Check**

- If Research reaches the required target, the current objective/round is won.

**6. Shop / Upgrade Phase**

- The player may purchase any affordable upgrade.
- The shop is intended to remain available rather than being restricted to a single mandatory shop phase.
- Shop refresh timing is currently undecided; a likely option is a refresh once per day/round while purchased upgrades remain permanent for the run.

**7. Continue**

- Advance to the next round.

---

## 10. Survival

The two partners share a survival economy rather than having separate health systems.

### Energy

Energy is the number the player must maintain to keep the pair alive.

The initial design intentionally avoids separate health bars, status effects, wounds, etc.

**Keep this system to one meaningful survival number during the prototype.**

Potential future rule:

> Energy requirement increases each round, forcing the player to continually improve the board engine.

This creates the central pressure:

**Build faster than the cost of surviving.**

---

## 11. Research / Objective

Research is the primary progression-to-victory resource.

A round/objective has a Research target.

Example:

> Research: 73 / 100

When the player reaches the target, the objective is completed.

A larger run may require completing multiple objectives/days/jobs in sequence.

This creates a structure similar to a roguelike's escalating floors or seasons without requiring complicated mission systems.

---

## 12. Coin Shop

Coin is spent on upgrades.

### Shop Philosophy

The shop should be:

- simple,
- always accessible when the player can afford something,
- focused on changing the rules rather than giving small statistical boosts,
- capable of producing synergistic builds.

The player should frequently look at the board and think:

> "That upgrade completely changes what I can do with this board."

### Upgrade Categories

Potential categories:

- Connection modifiers
- Resource multipliers
- Placement manipulation
- Board manipulation
- Diagonal/vertical/horizontal specialization
- Resource-node manipulation
- Color interaction
- Long-line scaling
- Risk/reward mechanics
- Combo mechanics
- Wildcards

### Example Upgrades

**Long Con**  
Connections of 5+ receive an additional multiplier.

**Inside Man**  
Resource nodes contained within a completed connection generate additional resources.

**Getaway Driver**  
Certain connection orientations generate bonus Medicine/actions.

**Loaded Dice**  
The first resource node encountered in a connection counts twice.

**Double-Cross**  
Once per round, the player may place the opposite color from the current placement.

**Safecracker**  
Connections containing Coin receive a bonus Coin multiplier.

**Ride or Die**  
Once per run, prevent a fatal Energy failure.

These are examples only. Upgrade design should remain data-driven and easy to expand.

---

## 13. Roguelike Replayability

Replayability should primarily come from **different board conditions + upgrade combinations**, not from an enormous number of rules.

Potential sources of variation:

- Random starting resource-node locations
- Different starting board layouts
- Random upgrade shop offerings
- Random upgrade costs
- Different Research targets
- Different Energy curves
- Rare special board tiles
- Run-specific modifiers
- Unlockable upgrade pools
- Increasingly difficult objectives

### Build Identity

A run should naturally develop a build identity based on upgrades.

Examples:

- Long-line build
- Diagonal build
- Resource-node build
- Coin economy build
- Action-generation build
- Multiplier build
- Wildcard build
- High-risk survival build

The player should not choose a class before the run. The **board and upgrades create the class.**

---

## 14. Special Tiles / Future Expansion

Potential future board elements:

### Wild

Counts as either Blue or Red for connection purposes.

### Locked Cell

Cannot receive pieces.

### Multiplier

A connection passing through it receives a score multiplier.

### Directional Tile

Only affects horizontal, vertical, or diagonal connections.

### Resource Converter

Changes one generated resource into another.

### Bomb / Collapse

Destroys or disables a section of the board after activation.

### Growing Resource

A node increases in value every round it remains active.

These should **not** be part of the first prototype unless required to test the core loop.

---

## 15. Core Design Tensions

The game needs several competing decisions to remain interesting.

### Short vs Long

Take a 4-line now or spend placements trying to create a 5/6/7-line?

### Immediate vs Permanent

Generate resources now or establish a better long-term engine?

### Survival vs Victory

Spend board opportunities generating Energy or pursue Research?

### Economy vs Board Power

Buy an upgrade now or save Coin for a more powerful one?

### Blue vs Red Placement

Even though the colors are mechanically equal, their positions compete for the same limited board space.

### Setup vs Payoff

A move may produce little immediately but create a powerful connection several turns later.

---

## 16. MVP Scope

The first playable prototype should contain only:

### Board

- Standard Connect Four-sized grid.
- Falling pieces.
- Blue and Red pieces.
- Permanent board.

### Core Loop

- Fixed number of placements.
- Player controls both colors.
- Round ends after placements are exhausted.
- Detect connections.
- Score connections.
- Continue to next round.

### Resources

- Coin
- Medicine/actions
- Research
- Energy

### Board Nodes

- At least Coin.
- Ideally all four resource types for early testing.

### Progression

- One Research target.
- One Energy requirement.
- Basic shop.
- 5–10 simple upgrades.

### Win/Lose

- Win by reaching Research target.
- Lose by failing Energy requirement.

Nothing else is required to prove the concept.

---

## 17. Explicit Non-Goals for MVP

Do **not** initially build:

- Story campaign
- Dialogue system
- Complex character stats
- Separate health for each partner
- Enemy AI
- Combat
- Multiple currencies beyond the four core resources
- Complex animations
- Elaborate procedural generation
- Dozens of tile types
- Meta-progression
- Multiplayer
- Online features

The question the MVP must answer is simply:

> **Is it fun to permanently build and exploit a Connect Four board as an engine?**

If yes, everything else can grow around it.

---

## 18. Technical Direction

The prototype should be simple enough to implement with straightforward data structures.

### Board Cell

Suggested conceptual data:

```text
Cell
- occupied: bool
- piece_color: NONE / BLUE / RED
- resource_type: NONE / COIN / MEDICINE / RESEARCH / ENERGY
- special_type: NONE / future values
- activation_state: inactive / active
```

### Game State

```text
GameState
- round
- board
- actions_remaining
- coin
- medicine
- research
- research_target
- energy
- energy_requirement
- upgrades[]
- shop[]
- run_modifiers[]
```

### Connection

A connection should be represented as data rather than merely a score.

```text
Connection
- cells[]
- color
- direction
- length
- base_value
- multiplier
- resources_contained[]
- activated
```

This will make future upgrade effects much easier to implement.

---

## 19. Critical Prototype Question: Scoring Persistence

This is currently the most important unresolved mechanical decision.

If a 4-line is worth resources every round forever, the player may quickly create runaway exponential economies.

That might actually be desirable — **if the game is designed around breaking the economy.**

However, the prototype should explicitly test three models:

### A. Permanent Generator

Every completed connection pays every round.

**Pros:** strongest engine-builder feeling.  
**Cons:** potentially exponential runaway.

### B. One-Time Score

A connection pays only when first completed.

**Pros:** easy balancing.  
**Cons:** weakens the persistent-board fantasy.

### C. Activated Generator

A connection pays every round after it is first completed.

**Pros:** preserves the fantasy while clearly distinguishing setup from payoff.  
**Cons:** requires generator balancing.

**Recommended first test: C.**

---

## 20. Design Philosophy

The game should remain deceptively simple.

The player should understand the rules within minutes but continue discovering interactions for many runs.

The desired progression of understanding is:

> **"It's Connect Four."**
>
> **"Oh, the board doesn't reset."**
>
> **"Oh, these resources stay on the board."**
>
> **"Oh shit, I can build an engine."**
>
> **"WAIT. THIS UPGRADE DOES THAT?"**
>
> **"I can break this game."**

That final realization is the target.

---

## 21. Open Design Questions

These should be answered through prototype testing rather than theory alone.

1. Does each action place one piece, or does one action place a Blue/Red pair?
2. What is the starting number of placements?
3. Should Blue and Red strictly alternate, or can upgrades alter turn order?
4. Can a connection contain another color as long as the four matching pieces are contiguous elsewhere?
5. How are overlapping connections scored?
6. If a 5-long line contains two separate 4-length windows, is it one connection or two?
7. Do resource nodes count as occupied board cells while still allowing a colored piece to connect through them?
8. Can a resource node itself become Blue/Red after being incorporated?
9. Which persistence model feels best: one-time, permanent, or activated generator?
10. Should generated resources happen immediately after placement or only at round end?
11. Does Energy have a maximum capacity?
12. Does Energy requirement increase every round?
13. What exactly causes the two partners to die?
14. Is Research reset between objectives or carried forward?
15. When does the shop refresh?
16. Can upgrades stack?
17. Can the same upgrade appear multiple times?
18. How many upgrades should a typical successful run contain?
19. How large should the board become before it feels strategically crowded?
20. Should the board ever expand?
21. What happens when a column becomes full?
22. Should special starting layouts be handcrafted, procedural, or both?
23. What does a "run" consist of — fixed number of rounds, escalating jobs, or one endless score?
24. What is the ultimate failure fantasy: running out of Energy, getting caught, or something else?

---

## 22. Immediate Build Order

Recommended implementation order:

1. Render Connect Four board.
2. Implement falling Blue/Red pieces.
3. Allow the player to control both colors.
4. Implement persistent board state.
5. Detect horizontal/vertical/diagonal connections.
6. Calculate connection length and reward.
7. Add round progression.
8. Add Coin, Medicine, Research, and Energy.
9. Add resource nodes to board cells.
10. Make activated connections generate resources.
11. Add Research victory condition.
12. Add Energy failure condition.
13. Add simple upgrade shop.
14. Add 5–10 upgrades.
15. Playtest the core loop.
16. Only then add special tiles, procedural layouts, narrative, and polish.

---

## 23. Current Working Pitch

> **Criminal Connect** is a roguelike engine-builder disguised as Connect Four. You play both halves of an outlaw partnership, dropping Blue and Red pieces onto a board that never resets. Build longer connections, capture valuable resource nodes, and turn your board into a permanent criminal operation. Spend the proceeds on increasingly ridiculous upgrades while balancing Energy and Research — because the job doesn't end until you either make the score or the two of you don't make it out.

**Prototype priority:** Make the board fun before making the criminals beautiful.
