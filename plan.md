# Durable Execution: 20-Minute Interactive Sponsor Presentation

## Context

Mason is giving a 20-minute interactive sponsor presentation at a conference. Not a pitch — a cool demo of Temporal (open-source durable execution). Follows the narrative arc of his "Events are the Wrong Abstraction" talk, condensed. Attendees follow along in Instruqt (VS Code + browser). Mason presents from stage with Slidev. Fewer slides, more doing.

Two deliverables: (1) Slidev slide deck, (2) Instruqt track with 2 challenges.

---

## Narrative Arc (from "Events are the Wrong Abstraction" CFP)

1. **The Wrong Frame of Reference** — Copernican revolution analogy. We built systems around the wrong center.
2. **The Wrong Thing is in the Center** — EDA diagram, scattered logic, ad-hoc state machines, no APIs, race conditions, tight coupling.
3. **Let's Put the Right Thing in the Center** — Durable Execution: business logic in one place, crash-proof, state survives failures.
4. **See It for Yourself** — Two interactive demos proving the point.
5. **The Secret** — It's still event-driven underneath. Complexities hidden by the platform layer.

---

## Slide Outline (~13 slides, ~20 min)

Since this is interactive, slides are minimal — they frame the narrative, then hand off to Instruqt.

| # | Title | Content | Time |
|---|---|---|---|
| 1 | Title | Talk title / Mason's name / Temporal logo | 0:00–0:30 |
| 2 | QR Code | Large QR + short URL. "Open this now — you'll be following along." | 0:30–1:30 |
| 3 | The Wrong Frame of Reference | Copernican analogy: Earth as center vs. Sun. What happens when you build around the wrong center? | 1:30–2:30 |
| 4 | EDA Diagram | Sample app using Event-Driven Architecture. Queues, topics, consumers, producers. "This is how we've been told to build reliable systems." | 2:30–3:30 |
| 5 | What's Wrong with This? | No APIs, scattered logic, tracing nightmares, ad-hoc state machines, race conditions, tight coupling. | 3:30–4:30 |
| 6 | Put the Right Thing in the Center | Durable Execution diagram. Business logic in one place, consistent state, crash-proof. Brief intro to Temporal as the open-source platform. | 4:30–5:30 |
| 7 | Let's Prove It | "Open your Instruqt tab. We're going to break some code." Transition to Challenge 1. | 5:30–6:00 |
| 8 | Normal vs. Durable | Slide with side-by-side code (normal counter vs. durable counter) while attendees run the demo. Minimal — just enough to see the shape. | 6:00–6:30 |
| — | **Challenge 1 runs** | Attendees run normal counter, kill it (restarts from 0). Run durable counter, kill it (resumes). | 6:30–10:00 |
| 9 | Now Let's Play a Game | "That was a counter. Let's see durable execution power a real app." Transition to Challenge 2. | 10:00–10:30 |
| — | **Challenge 2 runs** | Attendees play Wordle, kill the worker, restart, game state survives. | 10:30–16:30 |
| 10 | How Did That Work? | Brief: Temporal records every step. When the worker restarts, it replays history to rebuild state. The event history IS the persistence. | 16:30–17:30 |
| 11 | Want to Know a Secret? | "It's still event-driven. All the complexity is hidden under the platform layer." The punchline from the CFP. | 17:30–18:30 |
| 12 | Where to Go From Here | Temporal docs, GitHub repos, community Slack, full workshop link, QR codes | 18:30–19:30 |
| 13 | Thank You | Contact info | 19:30–20:00 |

---

## Instruqt Track Design

### Track: "Durable Execution with Temporal"

### Environment

Single container, everything pre-running. No attendee setup.

- **Python 3.14**, uv, Temporal CLI pre-installed
- Temporal dev server already running
- Both demo projects cloned and dependencies installed:
  - `/root/durable-vs-normal-execution/` (from MiaB)
  - `/root/durable-wordle/` (full app, worker + server already running)

**Tabs:**
1. **VS Code** (code editor) — browse source files
2. **Terminal** — run/kill processes for Challenge 1
3. **Wordle** — web service tab (port 8000, for Challenge 2)
4. **Temporal UI** — web service tab (port 8233, optional exploration)

### Challenge 1: Normal vs. Durable Execution (~3.5 min)

**Title:** "Break It and See What Happens"

**Instructions (markdown, left panel):**

> In the terminal, run the normal counter:
> ```
> cd /root/durable-vs-normal-execution/normal
> python counter.py
> ```
> Watch it count. After it reaches ~5, press **Ctrl+C** to kill it.
>
> Now run it again:
> ```
> python counter.py
> ```
> It starts over from 1. That's normal execution.
>
> ---
>
> Now let's try the durable version. In the terminal:
> ```
> cd /root/durable-vs-normal-execution/durable
> python starter.py
> python worker.py
> ```
> Watch it count. After ~5, press **Ctrl+C** to kill the worker.
>
> Now restart just the worker:
> ```
> python worker.py
> ```
> It picks up where it left off. That's durable execution.
>
> **Optional:** Open `counter.py` in the VS Code tab to see the code. Click Next when the presenter moves on.

**What this proves:** Same logic, same kill. Normal restarts from scratch. Durable resumes. No tricks.

### Challenge 2: Durable Wordle (~6 min)

**Title:** "Play a Game That Can't Die"

Wordle worker + FastAPI server are already running (started by the setup script).

**Instructions:**

> Click the **Wordle** tab above. Play a few guesses.
>
> Now let's break it. In the terminal:
> ```
> kill $(pgrep -f "durable_wordle.worker")
> ```
> Try to make a guess in the Wordle tab — it won't respond. The worker is dead.
>
> Now bring it back:
> ```
> cd /root/durable-wordle && uv run python -m durable_wordle.worker &
> ```
> Go back to the Wordle tab. Make another guess. Your game is exactly where you left it.
>
> **Optional:** Click the **Temporal UI** tab to see your workflow's event history — every guess recorded.
>
> **Optional:** Open `workflow.py` or `activities.py` in VS Code to browse the code.

**What this proves:** A real application with real state (a game in progress) survives a worker crash. No database. The workflow IS the state.

---

## Slides ↔ Instruqt Sync

| Time | Stage (Slidev) | Audience (Instruqt) |
|---|---|---|
| 0:00–6:00 | Narrative slides (Copernican → EDA problems → Durable Exec) | Open Instruqt, read Challenge 1 intro |
| 6:00–10:00 | Slide 8 (side-by-side code) + live narration | Challenge 1: run/kill normal & durable counters |
| 10:00–16:30 | Slide 9 transition, then narrate along | Challenge 2: play Wordle, kill worker, restart |
| 16:30–20:00 | Slides 10-13 (explanation, secret, resources) | Done — optional browsing |

---

## Risk Mitigation

- **Instruqt slow to start:** QR on slide 2, 5.5 min of narrative slides as buffer before first interaction.
- **Attendees fall behind:** Copy-pasteable commands. Challenges are independent — can skip ahead.
- **Dictionary API rate limiting:** Pre-configure Instruqt env to validate guesses against bundled word list (no external API calls).
- **Time pressure:** Buffer is in Challenge 2 Wordle playtime. Cut from 6 min to 4 if behind.

---

## Implementation Steps

### Step 1: Slide Deck (Keynote)
- Build deck in Keynote (~13 slides following outline above)
- Temporal brand colors, architecture diagrams for EDA vs. Durable Execution
- Presenter notes with timing cues
- Slidev was explored but too painful for diagram-heavy slides — no visual layout control, SVG coordinate wrangling, animation issues. Keynote is the right tool here.

### Step 2: Instruqt Track
- Track config (track.yml, config.yml)
- 2 challenge assignment files (markdown)
- Setup/lifecycle scripts

### Step 3: Container Image
- Dockerfile for Instruqt sandbox
- Python 3.14, uv, Temporal CLI
- Both demo projects cloned with deps installed
- Startup script: Temporal dev server, Wordle worker, FastAPI server
- Expose ports 8000 (Wordle) and 8233 (Temporal UI)

### Step 4: Dictionary API Mitigation
- Local word validation for Instruqt env (avoid 100+ attendees hitting external API)

### Step 5: Test End-to-End
- Walk through both challenges
- Time the full flow
- Verify kill/restart works cleanly

---

## Key Files

| File | Role |
|---|---|
| `miab-build-invincible-apps-python/durable-vs-normal-execution/` | Challenge 1 source (normal + durable counters) |
| `durable-wordle/src/durable_wordle/workflow.py` | Core workflow for Challenge 2 |
| `durable-wordle/src/durable_wordle/activities.py` | Activities for Challenge 2 |
| `cfps/2025/Talks/eda-wrong/eda-wrong.md` | Talk track / narrative arc reference |
| `durable-wordle-instruqt/` | Target repo for Slidev deck + Instruqt track |
