# Demo Runbook — Session 6 · Lists & Tuples

**Trainer-facing.** This is the exact, reproducible, on-screen runbook for the two live demos in
Week 6: **D11 — list mutation & aliasing pitfalls** and **D12 — tuple unpacking & swap**. Every
command, its expected output, the end state learners should see, a reset between runs, and a
live-failure fallback are below. All behaviour verified by execution against **Python 3.14.6**
(CPython), with **uv 0.11.28** and **Ruff 0.15.21** available, as of **2026-07-16**.

> Both demos run entirely in the **REPL** — there is no file to prepare and no project to scaffold.
> That is deliberate: today's lesson is about what happens to objects *as you touch them*, and the
> REPL is the only place the cohort can watch a list change one line at a time. Re-open the running
> `taskapp` working folder you've used since Week 1 and start the REPL from inside it, so today's
> task tuples feel like they belong to the capstone the cohort is growing.

---

## Pre-flight checklist (do this before the session starts)

Run through this 10–15 minutes before you go live. Tick every box.

**Accounts / keys.** None required. Everything in Week 6 is free, offline, and local — no logins, no
API keys, no third-party packages, no network.

**Tools installed on the demo machine (verify versions):**

```bash
python3.14 --version      # -> Python 3.14.6  (or later 3.14.x)
uv --version              # -> uv 0.11.28     (fallback run path)
```

- Expected: `python3.14 --version` prints `Python 3.14.6` (or a later 3.14.x).
- If `python3.14` is not on PATH, use `uv run python` (uv supplies 3.14), or `py -3.14` on Windows.
- Nothing in this session is version-sensitive: lists and tuples are **unchanged in 3.13 and 3.14**.
  Any 3.10+ interpreter would behave identically. The 3.14 pin is for cohort consistency only.

**Sample data / repo state.**

- Open a clean working folder for today, e.g. `~/pymastery-demos/week06`, and `cd` into it.
- Nothing else to prepare — every value in both demos is typed live.
- Have the task titles ready so you don't stall inventing them mid-demo: `write report`,
  `buy milk`, `call bank`, `pay rent`, `water plants`, `book flights`, `renew passport`.

**Window / zoom layout.**

- Terminal font **≥ 18pt**, high-contrast theme; terminal window large, roughly two-thirds screen.
- You do **not** need VS Code today — resist the temptation to open it. Full-screen REPL keeps every
  eye on the one list that is changing.
- Hide notifications / Do-Not-Disturb on.

**Recording.**

- **Start the Teams recording before you speak the first word of the session** — well before the
  Demo 11 hand-off. Students rebuild both demos from this recording.

**Note on REPL multi-line blocks.** When you type a compound statement (a `for` loop) into the REPL,
the prompt changes from `>>>` to `...` for the indented body. Finish the block with a **blank line**
(press Enter on an empty `...` line) to run it. This bites in D12 step 6.

**Note on tracebacks.** The default 3.14 REPL reports errors against a filename like
`<python-input-7>`, and the number increments with every line you type — so **the exact number in your
session will differ from this document**. It also draws `~~~^^^` anchor carets under the failing part
of the expression. Both are cosmetic. Read the cohort the **last line** — the error type and message —
which is what this runbook pins and what is stable.

---

## Demo D11 — list mutation & aliasing pitfalls

**Backs:** the list-methods, slicing, `del`, mutability and aliasing concepts (Slides 6–17;
speaker-notes Segments 2–3, checkpoint Slide 21). **Scenario:** manage a growing to-do list, then walk
straight into the aliasing bug and fix it.

**Preconditions:** REPL open (`python3.14`) in today's working folder. No prior state.

This is the most important demo in Part 1 so far. Steps 1–3 are brisk — the cohort has seen the method
names on the slides and needs to watch them bite, not hear them explained again. **Slow right down at
step 4.** That step is the one the whole session is built around, and the cohort needs silence and
screen time on it, not narration speed. If you are running late, cut step 3 (slicing), never step 4.

### Steps

**1. Open the REPL.**

```bash
python3.14
```

Expected: the `Python 3.14.6 ...` banner and the `>>>` prompt.

> Windows: `py -3.14`. Inside a project: `uv run python`.

**2. Build the list and run the core methods.**

```pycon
>>> tasks = ["write report", "buy milk"]
>>> tasks
['write report', 'buy milk']
>>> tasks.append("call bank")
>>> tasks
['write report', 'buy milk', 'call bank']
>>> tasks.insert(0, "pay rent")
>>> tasks
['pay rent', 'write report', 'buy milk', 'call bank']
>>> tasks.extend(["water plants", "book flights"])
>>> tasks
['pay rent', 'write report', 'buy milk', 'call bank', 'water plants', 'book flights']
>>> len(tasks)
6
```

Expected: exactly the lists above. Call out that `append` added **one** item while `extend` added
**two** — that is the distinction the cohort most often gets wrong.

```pycon
>>> tasks.remove("buy milk")
>>> tasks
['pay rent', 'write report', 'call bank', 'water plants', 'book flights']
>>> tasks.pop()
'book flights'
>>> tasks.pop(0)
'pay rent'
>>> tasks
['write report', 'call bank', 'water plants']
>>> tasks.index("call bank")
1
>>> tasks.count("call bank")
1
```

Expected: `pop()` **echoes the item it removed** (`'book flights'`), `remove` echoes nothing. That
contrast is the point of the step — say it out loud as it happens.

**3. Show the two error paths, then slicing and `del`.**

```pycon
>>> tasks.remove("nope")
Traceback (most recent call last):
  File "<python-input-11>", line 1, in <module>
    tasks.remove("nope")
    ~~~~~~~~~~~~^^^^^^^^
ValueError: list.remove(x): x not in list
```

Expected: a `ValueError`. Point out that `remove` does **not** fail politely — it raises.

```pycon
>>> tasks[0]
'write report'
>>> tasks[-1]
'water plants'
>>> tasks[1:3]
['call bank', 'water plants']
>>> tasks[1:3] = ["ring the bank"]
>>> tasks
['write report', 'ring the bank']
```

Expected: the slice assignment replaced **two** items with **one** and the list shrank from three
items to two. That resize is the beat to emphasise — strings cannot do this.

```pycon
>>> a = [-1, 1, 66.25, 333, 333, 1234.5]
>>> del a[0]
>>> a
[1, 66.25, 333, 333, 1234.5]
>>> del a[2:4]
>>> a
[1, 66.25, 1234.5]
>>> del a[:]
>>> a
[]
```

Expected: exactly these three lists. (This is the official tutorial's own example, so it is worth
using verbatim — students who go and read the docs will meet it again.)

**4. THE ALIASING BUG — the centrepiece. Slow down here.**

```pycon
>>> a = ["write report", "buy milk"]
>>> b = a
>>> b.append("call bank")
>>> a
['write report', 'buy milk', 'call bank']
```

Expected: **`a` has three items** — despite `a` never being mentioned after line 1. Stop talking for a
second and let that land. Ask the cohort out loud what they expected before you explain it.

```pycon
>>> b
['write report', 'buy milk', 'call bank']
>>> a is b
True
```

Expected: `True`. This is the proof: one list, two names.

**5. The fix.**

```pycon
>>> c = a.copy()
>>> c is a
False
>>> c.append("only in c")
>>> a
['write report', 'buy milk', 'call bank']
>>> c
['write report', 'buy milk', 'call bank', 'only in c']
```

Expected: `a` is **unchanged** while `c` grew — the two lists now move independently.

```pycon
>>> d = a[:]
>>> d is a
False
>>> e = list(a)
>>> e is a
False
>>> e == a
True
```

Expected: all three copy techniques give `is` → `False`, and `==` → `True`. Land the distinction:
**`is` asks "same object?", `==` asks "same contents?"**

**6. `.sort()` vs `sorted()` (plants the Segment 6 gotcha).**

```pycon
>>> nums = [3, 1, 2]
>>> sorted(nums)
[1, 2, 3]
>>> nums
[3, 1, 2]
>>> nums.sort()
>>> nums
[1, 2, 3]
>>> print(nums.sort())
None
```

Expected: `sorted()` returns a new list and leaves `nums` alone; `.sort()` rearranges `nums` and
prints `None`. Do **not** fully unpack the trap here — it is Segment 6's slide. Just show it and move on.

### End state

The cohort has seen a list grow, shrink, resize under slice assignment, and lose items to `del`; has
watched `a` change when only `b` was touched; has seen `a is b` return `True` and then `False` after a
copy; and has seen `.sort()` return `None`. The REPL holds `a`, `b`, `c`, `d`, `e`, `nums`, `tasks`.

### Reset between runs

```pycon
>>> exit()
```

Then relaunch `python3.14`. A fresh REPL is the only reset needed — no files were written. If you
prefer to stay in the same session, `del a, b, c, d, e, nums, tasks` clears every name this demo bound.

### Live-failure fallback

- **Highest risk: you accidentally break the aliasing reveal** by typing `b = a.copy()` out of muscle
  memory in step 4. If `a` does not grow, you have copied instead of aliased. Recover in-place — it
  makes a *better* demo: "look, nothing happened — that's because I typed `.copy()`. Watch what
  happens when I don't," then redo it with a bare `b = a`. Never restart the session for this.
- **You mistype and the list is in an unexpected state.** Do not debug it live. Rebind from scratch:
  `a = ["write report", "buy milk"]` and carry on. The list contents are never load-bearing — only the
  *behaviour* is.
- **The whole REPL dies or gets unrecoverable.** The entire demo is retypeable from this document in
  under three minutes. Relaunch and jump straight to step 4; steps 2–3 are revision of the slides and
  can be skipped without loss.
- **You are short on time.** Steps 4 and 5 are the demo. Everything else is optional.

---

## Demo D12 — tuple unpacking & swap

**Backs:** the tuple, packing/unpacking, nesting and sequence-comparison concepts (Slides 23–28;
speaker-notes Segments 4–5, checkpoint Slide 32). **Scenario:** model a task as `(id, title, done)`
and unpack it fluently.

**Preconditions:** REPL open (a fresh one is cleanest — see the D11 reset). No prior state needed.

Step 6 is the payoff of the entire session: the first thing in the course that looks like the app the
cohort was promised in Week 1. Give it room, and name the fact that every piece of it — the list, the
tuple, the unpacking, the conditional expression, the f-string — comes from a different week.

### Steps

**1. Open a fresh REPL.**

```bash
python3.14
```

Expected: banner and `>>>` prompt.

**2. Build a task tuple and prove it is immutable.**

```pycon
>>> task = (1, "write report", False)
>>> task
(1, 'write report', False)
>>> task[1]
'write report'
>>> len(task)
3
>>> task[0] = 9
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    task[0] = 9
    ~~~~^^^
TypeError: 'tuple' object does not support item assignment
```

Expected: reading works exactly like a list; **assignment raises `TypeError`**. Call back to Week 3 —
this is the same error shape strings gave, for the same reason.

```pycon
>>> del task[0]
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    del task[0]
        ~~~~^^^
TypeError: 'tuple' object doesn't support item deletion
```

Expected: `del` is refused too. (Note CPython's own inconsistency: "does not" for assignment,
"doesn't" for deletion. Not worth a detour unless a sharp-eyed student spots it.)

**3. The one-element trap.**

```pycon
>>> type((1))
<class 'int'>
>>> type((1,))
<class 'tuple'>
>>> x = 1, 2, 3
>>> x
(1, 2, 3)
>>> type(())
<class 'tuple'>
```

Expected: `(1)` is an `int`; `(1,)` is a `tuple`; bare `1, 2, 3` is a tuple with no brackets at all.
Land the rule: **the comma makes the tuple, not the brackets.**

**4. Unpack it.**

```pycon
>>> tid, title, done = task
>>> tid
1
>>> title
'write report'
>>> done
False
>>> a, b = task
Traceback (most recent call last):
  File "<python-input-14>", line 1, in <module>
    a, b = task
    ^^^^
ValueError: too many values to unpack (expected 2, got 3)
```

Expected: three names populate in one line; a count mismatch raises a `ValueError` that **names the
expectation and the reality**. Read that message aloud — it is unusually helpful.

**5. The swap and star-unpacking.**

```pycon
>>> a, b = 1, 2
>>> a, b = b, a
>>> a, b
(2, 1)
>>> numbers = [10, 20, 30, 40]
>>> first, *rest = numbers
>>> first
10
>>> rest
[20, 30, 40]
>>> *most, last = numbers
>>> most
[10, 20, 30]
>>> last
40
```

Expected: the swap needs no temporary; `rest` and `most` come back as **lists**. If the cohort has
Java/C# developers in it, pause on the swap — it reliably gets a reaction.

**6. A list of tuples, unpacked in the `for` — the payoff.**

```pycon
>>> tasks = [(1, "write report", False), (2, "buy milk", True), (3, "call bank", False)]
>>> for tid, title, done in tasks:
...     mark = "x" if done else " "
...     print(f"[{mark}] {tid}. {title}")
...
[ ] 1. write report
[x] 2. buy milk
[ ] 3. call bank
```

Expected: exactly those three lines. **Finish the block with a blank line** on the `...` prompt.

**7. Sequence comparison and `sorted(key=...)`.**

```pycon
>>> (1, 2, 3) < (1, 2, 4)
True
>>> (1, 2) < (1, 2, 3)
True
>>> sorted(tasks, key=lambda t: t[1])
[(2, 'buy milk', True), (3, 'call bank', False), (1, 'write report', False)]
```

Expected: item-by-item comparison; `key=` reorders by title, so id 2 leads. This is the exact shape
Lab 6's `sorted_by_title` needs — say so explicitly.

### End state

The cohort has seen a tuple refuse assignment and deletion, seen the trailing-comma rule, unpacked a
tuple into three names, swapped two variables in one line, star-unpacked a list, printed a real
to-do list from a list of tuples, and sorted it by title. The REPL holds `task`, `tasks`, `a`, `b`,
`numbers`, `first`, `rest`, `most`, `last`.

### Reset between runs

```pycon
>>> exit()
```

Relaunch `python3.14`. No files were written, so nothing else needs undoing.

### Live-failure fallback

- **Highest risk: the multi-line `for` block in step 6.** A stray indent or a missed blank line leaves
  you stuck at `...`. Press **Ctrl-C** to abandon the block and get a clean `>>>` back, then retype.
  If it fails twice, fall back to the **one-liner** — it produces the same three lines without any
  indentation risk:
  ```pycon
  >>> for tid, title, done in tasks: print(f"[{'x' if done else ' '}] {tid}. {title}")
  ```
- **The f-string or conditional expression fights you.** Drop to `print(tid, title, done)` — plain,
  ugly, and it still demonstrates the unpacking, which is the actual teaching point.
- **You are short on time.** Steps 4 and 6 are the demo. Step 3 (the comma trap) and step 7 can be
  left to the slides, which already carry the verified output.
- **A student insists `(1)` should be a tuple.** Do not litigate it live beyond the `type()` call —
  the REPL has already answered, and it is on Slide 24 in writing.

---

## Post-demo tidy

Nothing to clean up: no files created, no packages installed, no environment changed. Close the REPL
with `exit()` and stop the recording at the end of the session as usual.
