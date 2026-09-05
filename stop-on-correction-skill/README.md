# stop-on-correction

🛑 An OpenClaw skill that stops the current task **only** when the user sends an explicit stop command — an isolated English word `stop` or the Chinese word `停止`.

> Prevents accidental interruptions: words embedded in a sentence, quotes, code samples, or Chinese synonyms like 停 / 别做了 / 算了 do **not** trigger it.

## Why this skill

When an AI agent runs a long multi-step task in a chat app, users often want to correct or halt it mid-way. Generic "stop detection" is too noisy — ordinary conversation contains words like "stop" in quotes, code, or discussion. This skill defines a **strict, predictable trigger** so the agent only halts when the user clearly commands it to.

## Trigger conditions (ALL must be true)

1. The user message contains the English word **`stop`** (case-insensitive: stop / Stop / STOP) **or** the Chinese word **`停止`**.
2. It appears as a **standalone word** — surrounded by whitespace/punctuation or at message boundaries. Not part of another word (`stopped`, `stops`, `stopwatch`, `don'tstop` don't count).
3. It is used as a **stop command**, not:
   - a word embedded in a sentence (e.g. "please change `stop` to something else");
   - discussion / translation / quoting (e.g. "what does `stop` mean?", "there's a `stop` variable in the code");
   - code, command output, error logs, or file contents containing `stop`.
4. The intent is to halt the task currently being executed.

Strictest trigger form: **the whole message is `stop` or `停止`** (optionally with punctuation, e.g. `stop!` / `停止！`).

## What does NOT trigger

- Chinese halt words other than `停止` (停, 停下, 别做了, 算了, 错了, 不对, …) — do not trigger by default, unless `stop` or `停止` also appears standalone in the same message.
- `stop` / `停止` embedded inside a natural sentence (e.g. "please stop this approach" wording embedded in longer text) unless clearly used as the halt command.
- Questions/discussion while the task still matches the user's goal.

## What happens when triggered

1. **Stop immediately** — no new tool calls, no remaining plan steps, no intermediate reports.
2. **Clean up running work** — kill background processes / exec sessions, kill spawned sub-agents, mark remaining `update_plan` steps as cancelled.
3. **Short confirmation + status** — 1–2 sentences confirming the halt, one sentence on what was completed, note any cleaned-up background tasks.
4. **Wait for new instructions.**

## Installation

```bash
# Option A: copy the folder into your workspace skills
# <workspace>/skills/stop-on-correction/SKILL.md

# Option B: install from git (if published)
openclaw skills install <repo-url>
```

Workspace skills directory takes highest precedence — see OpenClaw skill docs.

## Files

```
stop-on-correction/
├── SKILL.md   # skill definition
└── README.md
```

## License

MIT
