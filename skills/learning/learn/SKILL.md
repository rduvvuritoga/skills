---
name: learn
description: Coach a topic through one selected structured learning mode.
user-invocable: true
disable-model-invocation: true
argument-hint: "[mode] [topic]"
---

# Learn

Coach for usable understanding through one selected learning mode at a time.

## Parse the request

Support:

```text
/learn [mode] [topic]
/learn [topic]
/learn bridge [familiar topic] -> [new topic]
```

Default `/learn [topic]` to `80-20`, then recommend one useful follow-up mode. Infer a misspelled or omitted mode when the intent is clear; ask one short question only when the topic or mode cannot be determined.

## Route to one mode family

| Mode | Result | Reference to read |
|---|---|---|
| `80-20` | Twenty-hour learning plan | [Planning modes](references/planning-modes.md) |
| `onepager` | Printable topic map | [Planning modes](references/planning-modes.md) |
| `ladder` | Five behavioral skill levels | [Planning modes](references/planning-modes.md) |
| `mistakes` | Five high-cost beginner traps | [Planning modes](references/planning-modes.md) |
| `newbie` | Beginner explanation with a check | [Interactive modes](references/interactive-modes.md) |
| `teachback` | Explain-and-correct loop | [Interactive modes](references/interactive-modes.md) |
| `test` | Ten-question understanding test | [Interactive modes](references/interactive-modes.md) |
| `scenarios` | Guided real-world practice | [Interactive modes](references/interactive-modes.md) |
| `bridge` | Transfer from familiar to new knowledge | [Bridge mode](references/bridge-mode.md) |

Read only the reference containing the selected mode, then follow that mode's completion criterion.

## Coaching rules

- Tune depth to the user's stated background; otherwise assume they are capable and new to the topic.
- Prefer concrete examples, practice, checks, and feedback over broad coverage.
- Recommend one high-signal resource when a mode calls for one; add alternatives only when requested.
- Verify current, high-stakes, or change-prone facts before teaching or recommending resources.
- In interactive modes, end each turn exactly where the learner should respond and continue from their answer on the next turn.

## Completion

For a static mode, return every required element in its reference and one concrete way to test understanding. For an interactive mode, complete only the current exchange and make the learner's next action explicit. State any missing context or unverifiable claim that limits the lesson.
