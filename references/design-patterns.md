# Machinations Design-Pattern Library

Recurring economy structures, expressed as Machinations patterns. Each entry gives the
**intent**, the **structure** (what it's built from), **when to use** it, its main
**consequences/trade-offs**, and example games. Diagrams here are generic — flows are
often just `n` and modifiers just `+`/`-`; supply concrete numbers when you build.

Patterns are a vocabulary, not a checklist. Name the ones you use, then verify that any
amplifying (positive-feedback) pattern is balanced by a throttling one.

---

## Category: Engines — produce the resources other mechanics consume

### Static engine
**Intent:** a steady, never-drying flow of resources to spend or collect.
**Structure:** a single source producing energy at a fixed (or unpredictable-but-fixed-
average) rate, plus actions to spend it on.
**Use when:** you want to limit player actions without much long-term planning; players
just decide how to spend a predictable income.
**Consequences:** very predictable and easy to balance; imbalance only arises if the
rate differs between players. Rarely inspires long-term strategy. Needs more than one
thing to spend on. Adding randomness to the rate forces players to plan for lean
stretches.
**Examples:** per-turn action budgets in turn-based games; regenerating energy you
allocate among systems in a space combat sim.

### Dynamic engine
**Intent:** an adjustable flow — players invest resources to raise production.
**Structure:** a source whose rate is raised by upgrades; an *invest* action that buys
upgrades; a positive **constructive** feedback loop at its core. Investment can raise
either the *frequency* of production (steady flow) or the *amount per burst* (bursty).
**Use when:** you want a long-term-investment vs. short-term-gain trade-off and more
player control than a static engine gives.
**Consequences:** a strong positive loop that usually *must* be balanced with friction
or escalation. Tune carefully to avoid a dominant "always invest first" (or "never
invest") strategy.
**Examples:** building up a resource-harvesting economy in strategy games where workers
beget more workers/production.

### Converter engine
**Intent:** two converters looped so the cycle nets a surplus to use elsewhere.
**Structure:** converter A → B and B → A arranged so each pass yields more than it
consumed; typically two interactive elements, so it's harder to read and operate than a
dynamic engine.
**Use when:** you want a richer, harder-to-assess production loop with more knobs to
tune the loop's profile.
**Consequences:** powerful but riskier — easy to misjudge; can deadlock if a converter
starves (a weak static engine priming the loop can prevent deadlock).
**Examples:** economies where two goods are repeatedly refined into each other at a
profit.

### Engine building
**Intent:** make *constructing and tuning the engine itself* the main game activity.
**Structure:** the general case of which a dynamic engine is one instance — many
production steps the player assembles and optimizes, without one dictated implementation.
**Use when:** the game is about building/construction and long-term planning.
**Consequences:** deep strategy and replayability; demands the rest of the economy
support sustained optimization, and risks runaway leaders without escalation/friction.
**Examples:** build-and-optimize economic strategy and tableau-building games.

---

## Category: Friction — drain resources or cut productivity

### Static friction
**Intent:** a drain that automatically consumes part of the player's production.
**Structure:** an automatic drain on a produced resource at a fixed rate.
**Use when:** you want a constant counter-force the player can eventually outgrow, or to
exaggerate the payoff of investing in a dynamic engine.
**Consequences:** simple and predictable; once production outpaces it, it stops
mattering.
**Examples:** fixed upkeep/maintenance costs.

### Dynamic friction
**Intent:** a drain whose rate *scales* with the player's state (progress, power, holdings).
**Structure:** an automatic drain modulated by a state connection from the thing it
scales against.
**Use when:** production runs too hot, or you want a self-scaling counter to progress
that automatically reins in leaders and favors short-term over long-term play.
**Consequences:** a strong negative-feedback stabilizer; the central balancing tool for
runaway dynamic engines. Over-tuned, it can punish progress and stall the game.
**Examples:** costs/penalties that grow with empire size or population.

### Stopping mechanism (law of diminishing returns)
**Intent:** each activation makes the mechanism less effective.
**Structure:** repeated use feeds back to reduce the action's own output.
**Use when:** you want to stop players abusing one action, counter a dominant strategy,
or weaken a positive feedback loop.
**Consequences:** discourages spamming and pushes variety; if too aggressive it can feel
punitive.
**Examples:** diminishing yield from over-applying the same input (the fertilizer
analogy — past a point, more reduces the result).

### Attrition
**Intent:** players actively steal or destroy each other's resources.
**Structure:** dynamic friction applied *symmetrically* between players — each drains
the other's stockpile, and those resources are needed for further actions.
**Use when:** you want direct, strategic player-vs-player interaction whose character is
set by player choices.
**Consequences:** introduces volatile, player-driven feedback; pairs naturally with an
arms race for depth. Can snowball without counter-mechanisms.
**Examples:** combat economies in strategy games where forces grind each other down.

---

## Category: Escalation — mounting pressure

### Escalating challenge
**Intent:** progress toward the goal makes further progress harder.
**Structure:** a feedback link from progress back to difficulty.
**Use when:** you want a fast, skill-based game that self-tightens as the player
advances, or emergent difficulty that partly replaces hand-authored level progression.
**Consequences:** naturally produces a difficulty ramp and a skill ceiling without
scripted levels; needs tuning so the ramp is fair.
**Examples:** action games that speed up or add hazards as you succeed.

### Escalating complexity
**Intent:** players hold back growing complexity until positive feedback overwhelms them.
**Structure:** an accumulating positive loop the player fights to keep in check; loss
occurs when it grows too strong.
**Use when:** you want a high-pressure, skill-based game with emergent difficulty.
**Consequences:** tension rises naturally toward an eventual break point; the "you will
eventually lose" framing must be intended.
**Examples:** falling-block games where the stack and its messes compound until you
can't keep up.

### Arms race
**Intent:** players invest to improve offense and defense against each other.
**Structure:** parallel investment tracks layered on top of an attrition economy.
**Use when:** you want more strategic options on top of attrition, and longer matches.
**Consequences:** adds depth and length; can balloon complexity and game time if
unbounded.
**Examples:** tech/upgrade escalation between competing strategy-game players.

---

## Miscellaneous patterns

### Playing-style reinforcement
**Intent:** slow, positive, constructive feedback that adapts the game to how a player
likes to play.
**Use when:** you want investment spanning multiple sessions, to reward planning and
personal strategy, or to let players grow into a role.
**Consequences:** strong identity and long-term engagement; slow by design, so effects
take many sessions to read.
**Examples:** character builds that deepen along the lines the player favors.

### Multiple feedback
**Intent:** one mechanism feeds several feedback loops with different profiles.
**Use when:** you want higher difficulty and to reward players who read the game state
well.
**Consequences:** rich, layered dynamics; harder for players (and you) to predict.

### Trade
**Intent:** player-to-player exchange that adds multiplayer dynamics and negative,
constructive feedback.
**Use when:** you want social/commerce interaction (rather than combat) and a
stabilizing force among players.
**Consequences:** encourages negotiation and interdependence; depends on players having
complementary needs.

### Worker placement
**Intent:** commit a limited pool of agents to activate or improve different mechanisms.
**Use when:** you want constant micromanagement, adaptation to changing conditions,
timing as a core skill, and subtle indirect conflict (contesting limited slots).
**Consequences:** tight tactical decisions each turn; competition for slots creates
indirect player conflict.
**Examples:** board games where placing limited workers claims actions others then can't.

### Slow cycle
**Intent:** a mechanism that moves slowly through states, periodically changing the
game's conditions.
**Use when:** you want periodic variation, to break a dominant strategy, to force
players to re-adapt, to lengthen the path to mastery, or to add subtle interaction by
letting players nudge the cycle's period/amplitude.
**Consequences:** keeps a game from settling into one optimal line; because cycles recur
rarely, players get fewer reps to learn them (raising the skill ceiling).
**Examples:** day/night, seasonal, or market cycles that shift what's optimal over time.

---

## Combining and nesting

Real games stack patterns. Examples of nesting/specialization:

- A **dynamic engine** is a concrete instance of **engine building**.
- **Attrition** is **dynamic friction** applied symmetrically between players.

Examples of genre-defining combinations:

- **Real-time strategy** ≈ dynamic engine (production) + attrition (combat), often plus
  an arms race (and occasionally engine building) for depth and length.
- **Many RPGs** ≈ playing-style reinforcement (character building) + escalating
  challenge (tougher fights as you progress).
- **Falling-block puzzles** ≈ escalating complexity (the stack compounds) + escalating
  challenge (it speeds up as you clear lines).

Design heuristic: for every amplifying pattern (dynamic/converter engine, arms race),
make sure something throttles it (friction, stopping mechanism, escalation) — an
unopposed positive loop is the usual root cause of a broken economy.
