---
slug: break-it-and-see
id: nsvwl7tngyw3
type: challenge
title: Break It and See What Happens
teaser: Kill a running program and see how normal vs. durable execution handles it.
tabs:
- id: uzzwrwo1miiv
  title: Code Editor
  type: service
  hostname: workstation
  path: ?folder=/root/durable-vs-normal-execution&openFile=/root/durable-vs-normal-execution/normal/counter.py&openFile=/root/durable-vs-normal-execution/durable/counter.py
  port: 8443
- id: 90fltb0qwwb2
  title: Terminal
  type: terminal
  hostname: workstation
  workdir: /root
difficulty: basic
timelimit: 600
enhanced_loading: null
---

# Normal Execution: It Forgets Everything

In the **Terminal** tab, run the normal counter:

```bash
cd /root/durable-vs-normal-execution/normal
uv run python counter.py
```

Watch it count up. After it reaches **~5**, press **Ctrl+C** to kill it.

Now run it again:

```bash
uv run python counter.py
```

It starts over from 1. That's normal execution — when the process dies, the state is gone.

---

# Durable Execution: It Remembers

Now let's try the durable version. In the terminal, start the workflow:

```bash
cd /root/durable-vs-normal-execution/durable
uv run python starter.py
```

Then start the worker (the thing that actually runs the code):

```bash
uv run python worker.py
```

Watch it count. After it reaches **~5**, press **Ctrl+C** to kill the **worker**.

Now restart just the worker:

```bash
uv run python worker.py
```

**It picks up where it left off.** That's durable execution — the workflow's state survived the crash.

---

# What Just Happened?

Same logic. Same kill. But the durable version resumed because **Temporal recorded every step**. When the worker restarted, it replayed the history to rebuild state automatically.

**Optional:** Click the **Code Editor** tab to compare `normal/counter.py` with `durable/counter.py` side by side.

When the presenter moves on, click **Next**.
