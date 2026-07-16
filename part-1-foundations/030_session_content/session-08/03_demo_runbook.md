# Demo Runbook — Session 8 · Comprehensions & Looping Idioms

**Trainer-facing.** This is the exact, reproducible, on-screen runbook for the two live demos in
Week 8: **D15 — rewrite loops as comprehensions** and **D16 — `enumerate`/`zip`/`sorted(key=...)`**.
Every command, its expected output, the end state learners should see, a reset between runs, and a
live-failure fallback are below. All behaviour was **executed and verified against Python 3.14.6**
(CPython, the current stable release) on **2026-07-16**.

> Both demos run in the **REPL**, seeded from one small file (`tasks_data.py`) so you don't waste live
> minutes retyping the task dictionary. That dictionary is the Week 7 model — tasks keyed by id, each
> with a title, a done flag, and tags — so the cohort recognises it as *their own* data. Re-open the
> running `taskapp` working folder you've used since Week 1.

**Two things in this session behave in ways that will surprise you if you haven't rehearsed them.**
First, **set print-order is not stable** — a set comprehension prints its items in a different order
between runs, because CPython randomises string hashes per process. Never promise the cohort a
specific set order; always wrap a set in `sorted(...)` when showing it. Second, an ordinary `for`
loop **leaks its loop variable** into the session, which will silently mask the `NameError` you want
to demonstrate in D15 step 7 — hence the deliberate `del t` in that step. Both are turned into
teaching beats below rather than worked around.

---

## Pre-flight checklist (do this before the session starts)

Run through this 10–15 minutes before you go live. Tick every box.

**Accounts / keys.** None required. Everything in Week 8 is free, offline, and local — no logins, no
API keys, no third-party packages.

**Tools installed on the demo machine (verify versions):**

```bash
python3.14 --version      # -> Python 3.14.6  (or later 3.14.x)
uv --version              # -> uv 0.x.x  (fallback run path)
uvx ruff --version        # -> ruff 0.x.x  (only needed if you format live)
```

- Expected: `python3.14 --version` prints `Python 3.14.6` (or a later 3.14.x).
- If `python3.14` is not on PATH, use `uv run python` (uv supplies 3.14), or `py -3.14` on Windows.
- Nothing today needs a version newer than 3.10 — comprehensions, `enumerate`, `zip`, `reversed` and
  `sorted` are **unchanged in 3.14**. The only 3.10-or-newer thing you'll type is `zip(strict=True)`
  (added in 3.10, PEP 618). It is safe on 3.14.

**Sample data / repo state.**

- Open a clean working folder for today, e.g. `~/pymastery-demos/week08`, and `cd` into it.
- Create the seed file **`tasks_data.py`** with exactly this content — both demos start from it:

```python
tasks = {
    1: {"title": "Buy milk", "done": False, "tags": ["shopping", "home"]},
    2: {"title": "Write report", "done": True, "tags": ["work"]},
    3: {"title": "Call plumber", "done": False, "tags": ["home", "urgent"]},
    4: {"title": "Archive photos", "done": True, "tags": ["home"]},
}
```

- This data is chosen deliberately, so don't casually edit it. `Archive photos` is **done** but sorts
  **first alphabetically** — that is the only reason the by-title sort and the by-done sort produce
  visibly different output in D16 step 6. With friendlier data the two sorts look identical and the
  whole point of the tuple key is lost.
- `home` appears on three tasks, so the set comprehension in D15 has real duplicates to collapse.

**Window / zoom layout.**

- Terminal font **≥ 18pt**, high-contrast theme; terminal window large, roughly two-thirds screen.
- Editor (VS Code) open beside the terminal showing `tasks_data.py`, so the cohort can see the data
  the whole time while you work in the REPL.
- Hide notifications / Do-Not-Disturb on.

**Recording.**

- **Start the Teams recording before you speak the first word of the session** — well before Slide 14
  (the D15 hand-off). Students rebuild both demos from this recording.

**Note on REPL multi-line blocks.** When you type a compound statement (a `for` or an `if`) into the
REPL, the prompt changes from `>>>` to `...` for the indented body. Finish the block with a **blank
line** (press Enter on an empty `...` line) to run it. The cohort met this in Week 4; a one-line
reminder the first time it happens is plenty.

---

## Demo D15 — rewrite loops as comprehensions

**Backs:** the comprehension concepts — list/set/dict, the filtering `if`, the conditional expression,
the position rule, and nesting (Slides 5–13; speaker-notes Segment 2). **Scenario:** take genuinely
verbose loops over the Week 7 task model and watch them collapse to single readable lines.

**Preconditions:** `tasks_data.py` present in the current folder (see pre-flight).

### Steps

**1. Open the REPL with the data pre-loaded.**

```bash
python3.14 -i tasks_data.py
```

Expected: the `Python 3.14.6 ...` banner, then a `>>>` prompt with `tasks` already defined. (The `-i`
flag means "run the file, then stay interactive".)

> Windows: `py -3.14 -i tasks_data.py`. Inside a project: `uv run python -i tasks_data.py`.

**2. Write the loop the long way first.** Do this honestly — the payoff depends on the cohort seeing
the verbose version exist before it disappears.

```pycon
>>> open_titles = []
>>> for t in tasks.values():
...     if not t["done"]:
...         open_titles.append(t["title"])
...
>>> open_titles
['Buy milk', 'Call plumber']
```

Expected: `['Buy milk', 'Call plumber']` — the two unfinished tasks.

**3. Replace all four lines with one comprehension.**

```pycon
>>> [t["title"] for t in tasks.values() if not t["done"]]
['Buy milk', 'Call plumber']
```

Expected: the **identical** list. Read it aloud left to right as you type it — "the title of each
task, for every task, where it isn't done" — the sentence *is* the syntax.

**4. Add a conditional expression (a transform, not a filter).**

```pycon
>>> [f"{t['title']} [x]" if t["done"] else f"{t['title']} [ ]" for t in tasks.values()]
['Buy milk [ ]', 'Write report [x]', 'Call plumber [ ]', 'Archive photos [x]']
```

Expected: **four** labels from four tasks. Contrast with step 3 out loud: step 3 asked for four tasks
and returned two titles (the filter **dropped** items); step 4 returned four (the conditional
expression **transformed** every item, dropping none). This is the position rule made visible.

**5. A dict comprehension.**

```pycon
>>> {tid: t["title"] for tid, t in tasks.items()}
{1: 'Buy milk', 2: 'Write report', 3: 'Call plumber', 4: 'Archive photos'}
```

Expected: exactly that mapping, in that order (dicts keep insertion order).

**6. A set comprehension — and why you must sort it to show it.**

```pycon
>>> sorted({tag for t in tasks.values() for tag in t["tags"]})
['home', 'shopping', 'urgent', 'work']
```

Expected: `['home', 'shopping', 'urgent', 'work']` — stable, alphabetical, every run.

> **Do not run this without `sorted(...)` and promise an order.** The bare set
> `{tag for t in tasks.values() for tag in t["tags"]}` prints its four tags in an order that **changes
> between runs** — CPython randomises string hashing per process, so you may legitimately see
> `{'home', 'work', 'urgent', 'shopping'}` on one run and `{'urgent', 'home', 'work', 'shopping'}` on
> the next. If you want to *make* that point live (it's a good one), run the bare set two or three
> times and let the cohort watch the order move — but frame it as "a set has no order", never as a bug.
> Then wrap it in `sorted` and show it settle.

Narrate the free dedupe: `home` is on three tasks and appears once, with no dedupe code written.

**7. The leak contrast, then the nested-order mistake.** This is the subtlest and best beat in the
demo — run it exactly in this order.

```pycon
>>> tag
NameError: name 'tag' is not defined
```

Expected: `NameError`. The comprehension's variable **did not leak** — `tag` evaporated when the
comprehension finished.

```pycon
>>> t
{'title': 'Archive photos', 'done': True, 'tags': ['home']}
```

Expected: `t` **is** still bound, holding the last task. It leaked out of the ordinary `for` loop you
wrote in step 2. Name the contrast explicitly: the plain loop left its variable lying around; the
comprehension cleaned up after itself.

```pycon
>>> del t
>>> t
NameError: name 't' is not defined
```

Expected: `NameError`. **This `del t` is mandatory** — without it, the stale `t` from step 2 makes the
next step return a wrong-looking-but-silent `['home', 'home', 'home', 'home']` instead of the error
you want to show.

```pycon
>>> [tag for tag in t["tags"] for t in tasks.values()]
NameError: name 't' is not defined
```

Expected: `NameError`, with a caret pointing at `t`. The `for`s are backwards — you asked for the tags
of `t` before saying what `t` was.

```pycon
>>> [tag for t in tasks.values() for tag in t["tags"]]
['shopping', 'home', 'work', 'home', 'urgent', 'home']
```

Expected: that six-item list. Correct order now — outer `for` first, inner second, left to right.
Point out that `home` appears **three times** here because a list keeps duplicates, which is exactly
why step 6 reached for a set instead.

### End state learners should see

A four-line filtering loop collapsed into one line with an identical result; a trailing `if` dropping
items versus an `if/else` transforming all of them; dict and set comprehensions built from the same
shape; a set silently deduplicating tags; proof that a comprehension's variable never leaks while a
plain `for` loop's does; and a nested comprehension failing loudly when its `for`s are reversed and
working when they're not.

### Reset between runs

```bash
exit()                                  # leave the REPL — no state persists
python3.14 -i tasks_data.py             # relaunch clean
```

`tasks_data.py` is never modified by this demo, so there is nothing to restore. If you re-run only
part of the demo, remember step 7 depends on step 2 having leaked `t`.

### Live-failure fallback

- **The set prints in an order you didn't expect (or that contradicts your slide):** this is *normal*
  and is covered above — say "a set has no order", wrap it in `sorted(...)`, move on. Slide 17 only
  ever shows the `sorted(...)` form, so the deck can't contradict you.
- **Step 7's wrong-order comprehension returns `['home', 'home', 'home', 'home']` instead of a
  `NameError`:** you skipped `del t`. Run `del t` and retype the line. This is the single most likely
  stumble in the demo — rehearse step 7 start to finish at least once.
- **A `SyntaxError` while typing a comprehension in the REPL:** almost always a missing closing
  bracket. Comprehensions are one-liners, so retype rather than trying to repair a mangled line.
- **The `...` continuation prompt confuses the room in step 2:** remind everyone to end an indented
  block with a **blank line** (they met this in Week 4).
- **`python3.14` not found mid-demo:** fall back to `uv run python -i tasks_data.py` or
  `py -3.14 -i tasks_data.py`.
- **Whole demo derails:** a known-good transcript of every step above lives in `checkpoint/d15.txt` on
  the demo machine — open it and talk over the outputs rather than fighting the terminal.

---

## Demo D16 — `enumerate` / `zip` / `sorted(key=...)`

**Backs:** the looping-idiom concepts — `enumerate`, `zip` (including `strict=`), `reversed`,
`sorted(key=...)`, and `sorted()` vs `.sort()` (Slides 19–25; speaker-notes Segment 4). **Scenario:**
present the task list the way a real application would — numbered, aligned, and sorted usefully.

**Preconditions:** REPL open with `tasks_data.py` loaded (relaunch if you exited after D15).

### Steps

**1. Number the list with `enumerate`.**

```pycon
>>> for n, t in enumerate(tasks.values(), start=1):
...     print(f"{n}. {t['title']}")
...
1. Buy milk
2. Write report
3. Call plumber
4. Archive photos
```

Expected: a clean 1-based numbered list, with **no counter variable anywhere** in the code.

> **Say `.values()` out loud and explain it.** `enumerate(tasks, start=1)` — iterating the dict
> directly — hands you the dict's **keys**, so `t` becomes an `int` and `t['title']` raises
> `TypeError: 'int' object is not subscriptable`. Students will hit this in the lab. If you want to
> show it deliberately, it's a fast, high-value 20 seconds.

**2. `zip` two lists that match.**

```pycon
>>> titles = ["Buy milk", "Write report", "Call plumber"]
>>> due = ["Mon", "Tue"]
>>> list(zip(titles, due))
[('Buy milk', 'Mon'), ('Write report', 'Tue')]
```

Expected: **two** pairs from a three-item list and a two-item list. `Call plumber` has **silently
vanished** — no error, no warning. Let that land; it's the point of the next step.

**3. The same `zip`, made strict.**

```pycon
>>> list(zip(titles, due, strict=True))
ValueError: zip() argument 2 is shorter than argument 1
```

Expected: exactly that `ValueError`. Narrate the trade: silent truncation became a loud, precise error
at the moment it happened. Recommend `strict=True` whenever the lists are *meant* to line up.

**4. The sorting surprise — why you usually want a key.**

```pycon
>>> sorted(["banana", "apple", "Cherry"])
['Cherry', 'apple', 'banana']
>>> sorted(["banana", "apple", "Cherry"], key=str.lower)
['apple', 'banana', 'Cherry']
```

Expected: `Cherry` **first** without the key (capital `C` is codepoint 67, lowercase `a` is 97 — every
capital sorts before every lowercase), and correct human alphabetical order with it. Stress that the
returned strings keep their **original capitalisation** — the key is a lens for comparing, not an edit.

**5. Sort the real tasks by title.**

```pycon
>>> [t["title"] for t in sorted(tasks.values(), key=lambda t: t["title"])]
['Archive photos', 'Buy milk', 'Call plumber', 'Write report']
```

Expected: that order. (The list comprehension here is only to keep the output readable — you're
sorting whole task dicts; call that out, it reinforces D15.)

**6. A two-key sort with a tuple.**

```pycon
>>> [(t["title"], t["done"]) for t in sorted(tasks.values(), key=lambda t: (t["done"], t["title"]))]
[('Buy milk', False), ('Call plumber', False), ('Archive photos', True), ('Write report', True)]
```

Expected: **open tasks first** (`False` before `True`), alphabetical **within** each group. Compare it
side by side with step 5's output — `Archive photos` moved from first to third. That contrast is the
whole reason the seed data is what it is.

Two things to narrate: a **tuple key** sorts on the first element and uses the second only to break
ties; and `False` sorts before `True` because booleans *are* integers (Week 2) — `False` is `0`.

**7. `reversed`, and the `sorted()` vs `.sort()` trap.**

```pycon
>>> list(reversed([1, 2, 3]))
[3, 2, 1]
```

Expected: `[3, 2, 1]`. Mention that without `list(...)` you'd see
`<list_reverseiterator object at 0x...>` — a lazy view, not an error.

```pycon
>>> nums = [3, 1, 2]
>>> sorted(nums)
[1, 2, 3]
>>> nums
[3, 1, 2]
>>> nums.sort()
>>> nums
[1, 2, 3]
```

Expected, in order: `sorted(nums)` returns `[1, 2, 3]`; `nums` is **still** `[3, 1, 2]` (untouched);
`nums.sort()` prints **nothing at all** (it returned `None`); `nums` is **now** `[1, 2, 3]` (mutated).
The silent line after `nums.sort()` is the demo — point at the blank REPL echo and say "it returned
None, that's why nothing printed", then name the classic bug: `nums = nums.sort()` leaves you holding
`None`.

### End state learners should see

A numbered task list built with no counter variable; `zip` silently truncating and then `strict=True`
catching it; a key function changing sort order without touching the data; a tuple key sorting on two
levels at once with open tasks first; and, with their own eyes, `sorted()` returning a new list while
`.sort()` returns `None` and mutates in place.

### Reset between runs

```bash
exit()
python3.14 -i tasks_data.py
```

Nothing persists and `tasks_data.py` is never written to. If you re-run step 7 alone, re-create
`nums = [3, 1, 2]` first — it's mutated by the end of the step.

### Live-failure fallback

- **`TypeError: 'int' object is not subscriptable` in step 1:** you wrote `enumerate(tasks, ...)`
  instead of `enumerate(tasks.values(), ...)`. If it happens by accident, keep it — it's the exact
  mistake the lab warns about. Explain it, fix it, move on.
- **`zip(strict=True)` raises `TypeError: zip() takes no keyword arguments`:** you are on Python 3.9
  or older. Confirm with `python3.14 --version` and relaunch with `python3.14`, `py -3.14`, or
  `uv run python`. On 3.14 `strict=` is always available.
- **Step 5 or 6 raises `TypeError: '<' not supported between instances of 'dict' and 'dict'`:** you
  dropped the `key=` argument. Python genuinely cannot compare two dicts — which is exactly *why* the
  key exists. A good accidental teaching moment; add the key back.
- **The two sorts in steps 5 and 6 look identical:** the seed data has been edited. Restore
  `tasks_data.py` from the pre-flight block — `Archive photos` must be `done: True` while sorting
  first alphabetically, or the contrast disappears.
- **Whole demo derails:** a known-good transcript lives in `checkpoint/d16.txt` on the demo machine —
  open it and talk over the outputs.

---

## Post-demo state to leave on screen

Leave the REPL showing the step-5 and step-6 sort outputs one above the other, with `tasks_data.py`
open in the editor beside it. That single screen — the same four tasks, two different orders, produced
by two one-line key functions — is the clearest possible bridge into the readability discussion and
the Lab 8 brief, and it's exactly what students are asked to reproduce this week.
