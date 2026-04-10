# session-briefing

**Session briefing automation for context continuity across Claude sessions.**

## Goal

session-briefing ensures no work is lost between sessions. It automatically captures decisions, open items, and next actions in a timestamped briefing, enabling seamless continuation.

## When & How to Use

Triggers automatically at: session end, task completion, 15+ turns, or explicit "continue" command. Saves to Agent-Ops/_session-briefs/ with timestamped filename.

## Use Cases

| Scenario | Prompt | What Happens |
|---|---|---|
| Session ends | (Auto-triggered) | Extracts decisions, open items, next actions→saves briefing→ready for next session |
| Task completion | (Auto-triggered) | Captures deliverable, blockers, remaining work→saves briefing |
| Continuation | `"Continue on this project."` | Reads latest brief→loads all context→resumes without repeating setup |

## Key Features

- Automatic capture at session boundaries and milestones
- 4 trigger points: session end, task completion, 15+ turns, "continue"
- Timestamped chronological storage
- Structured format: decisions, open items, next actions
- Context restoration for seamless continuation

## Works With

- **[up-manager](https://github.com/jasonnamii/up-manager)** — records UP changes for context
- **[deliverable-engine](https://github.com/jasonnamii/deliverable-engine)** — tracks deliverable status

## Installation

```bash
git clone https://github.com/jasonnamii/session-briefing.git ~/.claude/skills/session-briefing
```

## Update

```bash
cd ~/.claude/skills/session-briefing && git pull
```

Skills placed in `~/.claude/skills/` are automatically available in Claude Code and Cowork sessions.

## Part of Cowork Skills

This is one of 25+ custom skills. See the full catalog: [github.com/jasonnamii/cowork-skills](https://github.com/jasonnamii/cowork-skills)

## License

MIT License — feel free to use, modify, and share.
