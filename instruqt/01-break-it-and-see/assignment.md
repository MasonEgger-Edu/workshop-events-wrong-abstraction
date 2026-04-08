---
slug: break-it-and-see
id: nsvwl7tngyw3
type: challenge
title: Break It and See What Happens
teaser: Kill a running program and see how normal vs. durable execution handles it.
tabs:
- id: 90fltb0qwwb2
  title: Terminal
  type: terminal
  hostname: workstation
  workdir: /root
- id: uzzwrwo1miiv
  title: Code Editor
  type: service
  hostname: workstation
  path: ?folder=/root/durable-vs-normal-execution
  port: 8443
difficulty: basic
timelimit: 600
enhanced_loading: null
---

# The Code

Two programs that count to 10. Same logic, different runtimes.

**normal/counter.py:**

```python
async def main():
    for i in range(1, 11):
        logging.info(i)
        await asyncio.sleep(1)
```

**durable/counter.py:**

```python
@workflow.defn
class CountingWorkflow:
    @workflow.run
    async def run(self) -> None:
        number = 0
        while number < 10:
            number = await workflow.execute_activity(
                add_one, number,
                start_to_close_timeout=timedelta(seconds=5),
            )
            workflow.logger.info(number)
            await asyncio.sleep(1)
```

Let's kill them both and see what happens.

---

# Normal Execution

In the **Terminal** tab:

```bash
cd /root/durable-vs-normal-execution/normal
uv run python counter.py
```

Watch it count. After ~5, press **Ctrl+C**. Run it again:

```bash
uv run python counter.py
```

Starts over from 1. State gone.

---

# Durable Execution

Start the workflow:

```bash
cd /root/durable-vs-normal-execution/durable
uv run python starter.py
```

Start the worker:

```bash
uv run python worker.py
```

Watch it count. After ~5, **Ctrl+C** to kill the worker. Now restart it:

```bash
uv run python worker.py
```

**It picks up where it left off.**

Same logic. Same crash. But this time the state survived because Temporal recorded every step. When the worker restarted, it replayed the history and kept going.

**Want to explore?** Click the **Code Editor** tab to browse the source.

When the presenter moves on, click **Next**.
