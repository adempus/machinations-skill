# Machinations.io functional syntax (verified lookup reference)

Load this when **emitting concrete syntax** for Machinations.io — the exact label
characters, formula types, and machine formats. The conceptual model (what nodes/
connections/feedback *mean*) lives in `SKILL.md`; this file is the token-level lookup.

All tables below are **verified against the live machinations.io docs** (browser-rendered,
2026). The docs are a client-side SPA — a plain HTTP fetch returns only the app shell, so
these were read by rendering the JS. For the workflow to re-verify, see
`accessing-live-docs.md`.

---

## Book (2012) → Machinations.io character mapping

The book and the live tool share identical *concepts* but differ in a few *label
characters*. When output is destined for Machinations.io, emit the tool's form:

| Concept | Book (2012) | Emit for Machinations.io |
|---|---|---|
| Interval | `1/5` (slash) | `1\|5` (**pipe**) — 1 resource every 5 steps |
| Range | `3-6` | `3..6` (**double-dot**) |
| Dice | `D6`, `2D6` | `Dx` — `D6`, `2D10`, `2D6+3` |
| All resources | `all` | `all` (unchanged) |
| Register / formula | letters + `max`/`min` | **math.js** syntax (full function library) |

Everything else (rates as plain numbers, `%` probabilities, gate weights, `+`/`-`
modifiers, `*` triggers, `!` reverse triggers, the `p`/`&`/`*`/`s` node markers) carries
over directly. When in doubt about a token, treat the tool's own docs / in-app behavior as
ground truth over the book.

### Book notation NOT verified in Machinations.io — do not emit
- **`n*` multipliers** (`3*50%`, `2*D3`) — the tool's Resource-Connection Formula Type list
  (below) has no multiplier type. Express with the **Formula** type (math.js) or N separate
  connections/probable outputs instead.
- **`+Nm` multiplier-modifiers** (`+2m`) — same reason; only interval modifiers (`+xi`) are
  in the verified formula table.
- **Slash-fraction modifier labels** (`+1/3`, `-2/4`) — the verified modifier format is a
  signed decimal (`-0.3`). Write `+0.333` or a math.js expression.

---

## Resource Connection Formula Types (verified — `/docs/resource-connections`)

The number of resources moved per step is set by the connection's **Type + Formula**.
⚠️ Connection formulas should contain **positive integers only** (resource tokens are
real-world entities).

| Type | What it does | Example |
|---|---|---|
| **Number** | Deterministic token count (static, or driven by modifiers). The go-to type. | `3` |
| **Chance** | `X%` chance to move 1 token. Over 100% splits: `250%` = 2 guaranteed + 50% chance of a 3rd. | `25%`, `250%` |
| **All** | Move *all* available resources this step. Input must be a **Pool**. | `all` |
| **Dice Roll** | Random; re-rolled each step. `nDx` = roll n x-sided dice. `2D6` (2–12) ≠ `D12` — `2D6` is bell-shaped (7 most likely), `D12` is flat. | `2D6` |
| **Formula** | Jack-of-all-trades; preferred for **custom-variable names** and **math.js** functions. Combines with dice: `10+D6` → [11,16]. | `10+D6`, `randomInt()` |
| **Weights** (Gate only) | Positive-integer weight per gate output; relative likelihood of each outcome. | `1`, `3` |

---

## State-Connection formula table (verified — `/docs/labels`)

| Formula type | Format | Examples | Applies to |
|---|---|---|---|
| modifiers | `+x` `-x` `+x%` `-x%` | `+2` `-0.3` `+50%` `-2%` | value (label) modifiers; node modifiers |
| interval modifiers | `+xi` `-xi` | `+2i` `-1i` | value (label) modifiers |
| probabilities | `x%` `x` | `20%` `3` | triggers after a gate |
| conditions | `==x` `!=x` `>=x` … | `==0` `!=2` `>=4` | activators; triggers after a gate |
| range (conditions) | `x..y` | `2..5` `4..7` | activators; triggers after a gate |
| trigger | `*` | `*` | triggers |
| reverse trigger | `!` | `!` | reverse triggers |
| **overwrite** | `=` | `=` | value modifiers; node modifiers |

**Modifier operators come in three forms** — reactive `+`/`-` (accumulate the signed delta
from each input) and **overwrite `=`** (set the target's formula/value *to* the origin's
value). `=` is a variant of the label/node modifier behaviors, not a separate connection
type. Use `=` to clamp/overwrite; `+`/`-` for incremental response to several inputs.

**Combining conditions in one formula** — multiply for AND, add for OR (a condition
evaluates to `1` if true, `0` if false):
- `largerEq(a,100)*smaller(a,200)` == `and(largerEq(a,100),smaller(a,200))`
- `largerEq(a,2)*(c+4)` computes `(c+4)` only when `a>=2` (else the whole thing is `0`)

Works in registers and custom variables.

---

## Intervals (verified verbatim — `/docs/labels`)

The pipe sets cadence: `count|every-N-steps`.

| Form | Meaning |
|---|---|
| `3\|2` | 3 resources every 2 steps |
| `D6\|3` | 1–6 resources every 3 steps |
| `1\|(3..6)` | 1 resource every 3 to 6 steps |
| `D6\|D6` | 1–6 resources every 1 to 6 steps |

`+1i` / `-3i` modifiers dynamically change a target's **interval**.

---

## Registers (verified — `/docs/registers`)

- **Passive** register: holds a **math.js formula**. Each input state connection is
  auto-assigned a letter (`a`, `b`, …) used as the variable. Shows the computed result when
  running, `fx` otherwise.
- **Interactive** register: **does NOT support formulas**. Exposes **Initial Value** (value
  at run start) and **Step Value** (the +/- amount per click). Player-driven.
- Both: **Limits** (min/max) clamp the value regardless of inputs; **Show in Chart** plots
  it per step.

---

## Custom variables (verified — `/docs/randomness`, `/docs/math-expression`)

Named values referenced by name (case-sensitive) in any node/connection formula.

- **Randomness variable** — a named RNG. Two types: **Interval** (closed `From`/`To` integer
  range) or **Array** (comma-separated pick list). Each has a **Distribution** (`Gaussian`
  bell-curve, or `Uniform`) and **Update** timing (`On Play` = fixed for the run, or
  `Each Step` = re-rolled each step).
- **Math Expression variable** — a math.js-defined value; can reference *other* custom
  variables (e.g. `Modifier = RNG + 1`). Supports 1-D and 2-D **arrays**, indexed
  `oneDimensional[5]`.

Rule of thumb: inline dice (`D6`, `2D6+3`) for one-off rolls on a single connection; a named
**randomness variable** when the distribution is reused or needs Gaussian/Array; a **math
expression variable** for shared computed constants.

---

## Element type abbreviations (verified — `/docs/framework-basics`, `/docs/shortcuts`)

| Type | Abbr. | Type | Abbr. |
|---|---|---|---|
| Source | **S** | Converter | **V** |
| Pool | **P** | Trader | **X** |
| Drain | **D** | Gate | **G** |
| Resource Connection | **C** | State Connection | **Alt+C** |

⚠️ These are the doc's **type shorthand**, **not** keyboard placement hotkeys (nodes are
placed by double-clicking the Components Panel — the Shortcuts page lists no single-letter
node hotkeys) and **not** confirmed to appear in exports or to be understood by the AI
builder. Use them only as human-readable type tags; spell the type out (`source`, `pool`)
when output must be machine-consumed. `D6` is read "Die 6" in-app.

---

## Machine formats (for programmatic moves, not the AI builder)

- **XML** — Unity plugin local cache; human-readable, version-controllable.
- **JSON** — the WebSocket/socket.io API (`game-init` / `get-all-elements` return node and
  connection IDs, types, labels, resource counts).
- **Google Sheets** — export/import as a component list with parameters.
