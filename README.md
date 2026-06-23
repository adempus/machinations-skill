# machinations-diagrams

An agent skill (Claude Code / Hermes) for modeling and balancing a game's **internal economy** with [Machinations](https://machinations.io) diagrams.

> Draw the economy, see the feedback, tune the game.

## What it does

Teaches the agent to translate an informal game description into a structured Machinations economy model — sources, drains, pools, gates, converters, traders, and the feedback loops among them — and to critique or balance an existing one using a named design-pattern vocabulary (dynamic engine, attrition, arms race, etc.).

It auto-loads whenever you model game mechanics, design a feedback loop, diagram resource flow, or name a design pattern — even without saying the word "Machinations."

## Layout

```
machinations/
├── SKILL.md                      # core: nodes, connections, syntax, time modes, expressing a diagram
└── references/
    └── design-patterns.md        # ~18 economy patterns: intent / structure / trade-offs / examples
```

## Provenance

Conceptual grounding follows the 2012 Machinations framework. Functional syntax (intervals `1|5`, ranges `3..6`, math.js registers, the `=` overwrite operator, single-letter node codes) was **verified against the live machinations.io docs** — which render client-side, so the pages were read via a JS-executing browser rather than a plain HTTP fetch.

## Install

Drop the `machinations/` directory into your skills folder (e.g. `~/.hermes/skills/gaming/`). The agent picks it up automatically.

## License

MIT
