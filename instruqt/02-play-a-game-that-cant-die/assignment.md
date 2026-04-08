---
slug: play-a-game-that-cant-die
id: v5hedudt8ajb
type: challenge
title: Play a Game That Can't Die
teaser: Play Wordle, kill the backend, bring it back — your game survives.
tabs:
- id: sia1gwr3eyrr
  title: Worker
  type: terminal
  hostname: workstation
  workdir: /root/durable-wordle
- id: ztwtzo2prbfi
  title: Web Server
  type: terminal
  hostname: workstation
  workdir: /root/durable-wordle
- id: rlgmwjeidfdp
  title: Wordle
  type: service
  hostname: workstation
  port: 8000
- id: bgemzsx0txkd
  title: Temporal UI
  type: service
  hostname: workstation
  port: 8233
- id: 52rfmpjmvfet
  title: Code Editor
  type: service
  hostname: workstation
  path: ?folder=/root/durable-wordle/src/durable_wordle
  port: 8443
difficulty: basic
timelimit: 900
enhanced_loading: null
---

# Start the App

That was a counter. Now let's try a real application — a full game of Wordle, powered by Temporal.

In the **Worker** tab:

```bash
uv run python -m durable_wordle.worker
```

In the **Web Server** tab:

```bash
uv run uvicorn --factory durable_wordle.api:create_production_app --host 0.0.0.0 --port 8000
```

---

# Play Wordle

Click the **Wordle** tab. Try to solve it.

---

# Peek Behind the Curtain

Click the **Temporal UI** tab. Find your game's workflow and click into it.

Every guess, every word validation, every piece of game state is recorded as an event. Look at the `select_word` activity result — that's the answer, in plain text.

---

# Start a Random Game

Go back to the **Wordle** tab. If your game is finished, click the **↻** button. If it's still going, refresh the page.

Toggle **Word Selection** from **Daily** to **Random**, then submit a word.

---

# Break It

Go to the **Worker** tab and **Ctrl+C** to kill the worker.

Go back to the **Wordle** tab and submit a guess. The page hangs — the worker is dead.

---

# Bring It Back

Restart the worker:

```bash
uv run python -m durable_wordle.worker
```

Go back to the **Wordle** tab. Your guess went through. The game is exactly where you left it — Temporal replayed the history and rebuilt the state.

---

# Win by Cheating

Go to the **Temporal UI** tab. Find your random game's workflow. Dig into the event history and find the target word.

Go back and type it in. You win.

**Want to explore the code?** Click the **Code Editor** tab.

When the presenter moves on, click **Next**.
