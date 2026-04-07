---
slug: play-a-game-that-cant-die
type: challenge
title: Play a Game That Can't Die
teaser: Play Wordle, kill the backend, bring it back — your game survives.
tabs:
  - title: Code Editor
    type: code
    hostname: workstation
    path: /root/durable-wordle/src/durable_wordle
  - title: Worker
    type: terminal
    hostname: workstation
    workdir: /root/durable-wordle
  - title: Web Server
    type: terminal
    hostname: workstation
    workdir: /root/durable-wordle
  - title: Wordle
    type: service
    hostname: workstation
    port: 8000
  - title: Temporal UI
    type: service
    hostname: workstation
    port: 8233
difficulty: basic
timelimit: 900
---

# Start the App

In the **Worker** tab, start the Temporal worker:

```bash
uv run python -m durable_wordle.worker
```

In the **Web Server** tab, start the web server:

```bash
uv run uvicorn --factory durable_wordle.api:create_production_app --host 0.0.0.0 --port 8000
```

---

# Play Wordle

Click the **Wordle** tab above. Play a few guesses — it's a real game of Wordle.

---

# Now Break It

Go to the **Worker** tab and press **Ctrl+C** to kill the worker.

Go back to the **Wordle** tab and try to make a guess. It won't respond — the worker is dead.

---

# Bring It Back

In the **Worker** tab, restart the worker:

```bash
uv run python -m durable_wordle.worker
```

Go back to the **Wordle** tab. Make another guess.

**Your game is exactly where you left it.** Every guess you made before the crash is still there. No database, no Redis — the Temporal workflow IS the state.

---

# Explore (Optional)

- Click the **Temporal UI** tab to see your workflow's event history — every guess is recorded as a workflow event.
- Click the **Code Editor** tab to browse `workflow.py` and `activities.py` — the entire game logic lives there.

When the presenter moves on, click **Next**.
