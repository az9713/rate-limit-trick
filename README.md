# Theo’s Rate-Limit Trick

A concise explainer of Theo’s “keep the rate-limit clock moving” workflow for Claude Code / Fable usage.

## Source video

- **Title:** *Mythos is here, it’s time to start tokenmaxxing*
- **Link:** https://www.youtube.com/watch?v=3sTu8sSLVfg

This repo contains:

- [`theo_rate_limit_trick_writeup.html`](./theo_rate_limit_trick_writeup.html) — a self-contained HTML write-up explaining the theory, workflow, and decision tree.
- [`theo_s_rate_limit_trick_explained.png`](./theo_s_rate_limit_trick_explained.png) — an infographic visualization of the same idea.

## Core idea

Theo’s trick is based on a lazy-start reset-window assumption:

> If the 5-hour reset clock starts only after the first message, then a tiny “heartbeat” message can start the countdown earlier while using almost no budget.

The goal is **not** to bypass limits or create extra usage. The goal is to reduce idle reset latency:

```text
scheduled heartbeat → active reset clock → less idle waiting
```

## Practical rule

Run the first tiny heartbeat when:

```text
5-hour usage = 0%
and
5-hour countdown has not started
```

Do not bother if the countdown is already active.

## Workflow

1. Check account status.
2. If usage is fresh at `0%` and no countdown is active, send a tiny message or run `claude -p` / `claude-p`.
3. Run it in an empty directory to avoid accidental repo changes.
4. Let the timer count down in the background.
5. Start serious Fable workflows later, ideally when the next reset is already close.
6. If usage is high and reset is far away, stop, switch accounts, or route work to cheaper models.

## Suggested repo layout

```text
.
├── README.md
├── theo_rate_limit_trick_writeup.html
└── theo_s_rate_limit_trick_explained.png
```

## Caveat

This explanation is based on Theo’s described behavior. The trick only makes sense if the platform actually uses a lazy-start reset window. If the provider changes the rate-limit mechanics, the heartbeat may stop being useful.
