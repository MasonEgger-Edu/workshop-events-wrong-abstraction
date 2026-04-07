---
slug: play-a-game-that-cant-die
id: v5hedudt8ajb
type: challenge
title: Play a Game That Can't Die
teaser: Play Wordle, kill the backend, bring it back — your game survives.
tabs:
- id: 52rfmpjmvfet
  title: Code Editor
  type: service
  hostname: workstation
  path: ?folder=/root/durable-wordle/src/durable_wordle&openFile=/root/durable-wordle/src/durable_wordle/workflow.py
  port: 8443
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
difficulty: basic
timelimit: 900
enhanced_loading: null
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

Click the **Wordle** tab. Try to solve it — it's a real game of Wordle.

---

# Peek Behind the Curtain

Click the **Temporal UI** tab. You should see your game's workflow listed. Click into it and explore the event history.

Find the first **UpdateWorkflowExecution** event — look at the `select_word` activity result inside it. That's the target word, right there in plain text. Every step of your game is recorded as a Temporal event.

---

# Start a Random Game

Go back to the **Wordle** tab. If your game is finished, click the **↻** button. If it's still in progress, refresh the page.

Before submitting your first guess, toggle the **Word Selection** switch from **Daily** to **Random**.

Now submit a word to start the game.

---

# Break It

Go to the **Worker** tab and press **Ctrl+C** to kill the worker.

Go back to the **Wordle** tab and submit another guess. The page will hang — the worker is dead, so Temporal can't process your guess.

---

# Bring It Back

Go to the **Worker** tab and restart the worker:

```bash
uv run python -m durable_wordle.worker
```

Now go back to the **Wordle** tab. Your guess went through — the page updated with your result.

**What just happened?** When the worker restarted, Temporal replayed the workflow's event history to rebuild the game state in memory. Then it processed your pending guess as if nothing had gone wrong. No database, no Redis — the workflow IS the state.

---

# Win by Cheating

Go to the **Temporal UI** tab. Find your new random game's workflow and dig into the event history. Find the target word in the `select_word` activity result.

Go back to the **Wordle** tab and type it in. You win.

When the presenter moves on, click **Next**.
