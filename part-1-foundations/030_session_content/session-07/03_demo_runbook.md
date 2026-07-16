# Demo Runbook — Session 7 · Sets & Dictionaries

**Trainer-facing.** This is the exact, reproducible, on-screen runbook for the two live demos in
Week 7: **D13 — set algebra dedupe** and **D14 — dict-of-tasks + `Counter`/`defaultdict`**. Every
command, its expected output, the end state learners should see, a reset between runs, and a
live-failure fallback are below. All behaviour was executed and verified against **Python 3.14.6**
(CPython) on **2026-07-16**, with **uv 0.11.28** and **Ruff 0.15.21** (Astral) available.

> Both demos run in the **REPL** so the cohort sees instant results, plus one saved file at the end of
> D14 that becomes the lab's starting shape. Re-open the running `taskapp` working folder you've used
> since Week 1 — today's keyed model is the one the capstone keeps, so it should feel like it belongs
> there.

---

## ⚠️ The one thing that will bite you today: set display order

**Sets print in an arbitrary order that changes between interpreter runs.** Python randomises string
hashes at startup (a security measure), so `{"work", "urgent", "email"}` may display as
`{'work', 'email', 'urgent'}` on one run and `{'email', 'urgent', 'work'}` on the next — same set,
same machine, no code change.

**What this means for you:**

- **Do not** promise the cohort a specific set-display order, and do not read the expected outputs
  below as fixed for the raw-set lines — **the contents are guaranteed, the order is not**.
- Every output in this runbook that must be reproducible is wrapped in **`sorted(...)`**. That is
  deliberate, and it is also the habit you are teaching.
- If a raw set prints in a different order than you rehearsed, **that is the lesson, not a failure** —
  say so out loud. The official tutorial itself now warns about exactly this.
- **`Counter` and `dict` output *is* stable** (dicts guarantee insertion order), so D14's outputs below
  are exact and will reproduce.

---

## Pre-flight checklist (do this before the session starts)

Run through this 10–15 minutes before you go live. Tick every box.

**Accounts / keys.** None required. Everything in Week 7 is free, offline, and local — no logins, no
API keys, no third-party packages. `collections` ships with Python.

**Tools installed on the demo machine (verify versions):**

```bash
python3.14 --version      # -> Python 3.14.6  (or later 3.14.x)
uv --version              # -> uv 0.11.28  (fallback run path)
```

- Expected: `python3.14 --version` prints `Python 3.14.6` (or a later 3.14.x).
- If `python3.14` is not on PATH, use `uv run python` (uv supplies 3.14), or `py -3.14` on Windows.
- **3.14 matters today.** The improved unhashable-key `TypeError` message shown in D13 step 8 and
  D14 step 9 exists **only on 3.14+**. On 3.13 or earlier you get the old, terser text — the demo
  still works, but the "look how much better this is" beat lands flat. Check the version.
- Everything else today (sets, dicts, `collections`) is long-stable and behaves identically on 3.9+.

**Sample data / repo state.**

- Open a clean working folder for today, e.g. `~/pymastery-demos/week07`, and `cd` into it.
- Have the **`tasks` dict literal** (D14, step 1) ready to paste — it is long to type live.
- Have `tasks.py` (D14, step 10) ready in a scratch file, or accept that you'll type it.

**Window / zoom layout.**

- Terminal font **≥ 18pt**, high-contrast theme; terminal window roughly two-thirds of the screen.
- VS Code open beside the terminal for the `tasks.py` save at the end of D14.
- Hide notifications / Do-Not-Disturb on.

**Recording.**

- **Start the Teams recording before you speak the first word of Slide 11** (the D13 hand-off) —
  ideally start it at the very top of the session. Students rebuild both demos from this recording.

**Note on REPL multi-line blocks.** When you type a compound statement (a `for`, or the `defaultdict`
loop in D14) into the REPL, the prompt changes from `>>>` to `...` for the indented body. Finish the
block with a **blank line** (press Enter on an empty `...` line) to run it. The cohort met this in
Week 4, but re-mention it the first time it happens today.

---

## Demo D13 — set algebra dedupe

**Backs:** the set concepts — uniqueness, set algebra, the dedupe idiom, membership speed, and
unordered-ness (Slides 4–10; speaker-notes Segment 3). **Scenario:** compare two tag lists, collapse
duplicates, and prove sets are fast.

**Preconditions:** REPL open (`python3.14`); nothing else.

### Steps

**1. Open the REPL.**

```bash
python3.14
```

Expected: the `Python 3.14.6 ...` banner and the `>>>` prompt.

> Windows: `py -3.14`. Inside a project: `uv run python`.

**2. Build the two tag sets.**

```pycon
>>> mine  = {"work", "urgent", "email"}
>>> yours = {"urgent", "home", "email", "email"}
>>> yours
{'email', 'urgent', 'home'}
```

Expected: `yours` shows **three** elements, not four — the duplicate `"email"` collapsed silently on
creation. **The display order will likely differ from the line above** — that is expected; point at it
and say so. Narrate: no error, no complaint, the duplicate simply had nowhere to live.

**3. The four operators.** Wrap each in `sorted(...)` so the output is stable and readable.

```pycon
>>> sorted(mine | yours)
['email', 'home', 'urgent', 'work']
>>> sorted(mine & yours)
['email', 'urgent']
>>> sorted(mine - yours)
['work']
>>> sorted(yours - mine)
['home']
>>> sorted(mine ^ yours)
['home', 'work']
```

Expected: exactly as above (these **are** stable — `sorted` guarantees it). Narrate each as a question:
union = "what do we use between us"; intersection = "what do we share"; difference = "what's mine
alone" — and show `yours - mine` immediately after to prove difference is **not** symmetric;
symmetric difference = "what do we disagree on".

**4. Operator vs method.** Show the strictness difference.

```pycon
>>> set("abc") & "cbs"
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    set("abc") & "cbs"
    ~~~~~~~~~~~^~~~~~~
TypeError: unsupported operand type(s) for &: 'set' and 'str'
>>> sorted(set("abc").intersection("cbs"))
['b', 'c']
```

Expected: the operator **rejects** a non-set; the method **accepts any iterable**. Narrate: the
operators are deliberately strict so you can't accidentally do set algebra against a stray string.

**5. The dedupe idiom.**

```pycon
>>> tags = ["work", "urgent", "work", "email", "urgent", "work"]
>>> len(tags)
6
>>> len(set(tags))
3
>>> sorted(set(tags))
['email', 'urgent', 'work']
```

Expected: `6`, then `3`, then the sorted list. Narrate: one function call replaced a loop and a
condition — and `sorted(set(...))` is the pair to remember (dedupe, then get an order back).

**6. Prove sets are unordered.** This is the money step for the ordering lesson. **Exit and re-enter
the REPL** — the randomisation is per-interpreter-start, so it will not change within one session.

```pycon
>>> exit()
```

```bash
python3.14 -c 'print({"work", "urgent", "email"})'
python3.14 -c 'print({"work", "urgent", "email"})'
python3.14 -c 'print({"work", "urgent", "email"})'
```

Expected: **the same three strings in (probably) different orders** across the three runs, e.g.
`{'work', 'email', 'urgent'}` then `{'email', 'urgent', 'work'}` then `{'work', 'urgent', 'email'}`.

> **If all three happen to print identically** — possible, there are only six orderings — just run it
> two or three more times; it will diverge. If you are unlucky or short on time, say the line and move
> on: "the order is arbitrary and I'm not allowed to promise you one." Don't burn 90 seconds here.

Then show the fix:

```bash
python3.14 -c 'print(sorted({"work", "urgent", "email"}))'
python3.14 -c 'print(sorted({"work", "urgent", "email"}))'
```

Expected: `['email', 'urgent', 'work']` **both times** — identical. Narrate: `sorted` is the fix, it's
one word, and it's always available.

**7. Time membership — set vs list.** Re-open the REPL is *not* needed; run these two from the shell.

```bash
python3.14 -m timeit -s "data = list(range(100_000)); target = 99_999" "target in data"
python3.14 -m timeit -s "data = set(range(100_000)); target = 99_999" "target in data"
```

Expected (order of magnitude, machine-dependent):

```
500 loops, best of 5: 924 usec per loop
10000000 loops, best of 5: 30.4 nsec per loop
```

Expected: the list in **microseconds**, the set in **nanoseconds** — roughly a 30,000× gap on the
verified machine. **Do not promise exact numbers**; promise the *units*. Narrate: the list walks all
100,000 items; the set hashes once and jumps. Make the growth point — ten times more data makes the
list ten times slower and leaves the set flat.

> This is the trainer's own measurement, not a documented guarantee. The official tutorial says only
> "fast membership testing" — it makes no complexity claim. Present the numbers as measured, which
> they are.

**8. The hashable rule, with 3.14's better error.** Back in the REPL (`python3.14`):

```pycon
>>> {["a", "b"]}
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    {["a", "b"]}
TypeError: cannot use 'list' as a set element (unhashable type: 'list')
>>> {("a", "b")}
{('a', 'b')}
```

Expected: the `TypeError` naming **'list' as a set element**, then the tuple version working fine.
Narrate: this message is **new in Python 3.14** — 3.13 and earlier said only
`TypeError: unhashable type: 'list'` and left you to guess where. The fix is a tuple, because tuples
can't change, so they can be hashed.

### End state

The cohort has seen: duplicates collapse on set creation; four operators answer four questions in one
line each; `set(...)` dedupe with `sorted(...)` to restore order; **the same set printing in different
orders across runs**; membership timed in nanoseconds vs microseconds; and the hashable rule with
3.14's improved error. They should leave this demo believing two things: *sets are for uniqueness and
membership*, and *never trust a set's order*.

### Reset between runs

```pycon
>>> exit()
```

Nothing is persisted — no files are written in D13. Just re-launch `python3.14` for a clean namespace.
(If you re-run step 6 in the same shell session, remember the ordering only re-randomises on a **new
interpreter start**.)

### Live-failure fallback

- **Set order prints "boringly" identical every time (step 6).** Say the line, show the `sorted(...)`
  fix, and move on — the teaching point survives without the coincidence. It is already stated on
  Slide 10 with two contrasting outputs, so the slide carries it.
- **`timeit` is slow / hangs / the numbers look wrong (step 7).** Cut the list count to `10_000`, or
  skip the live timing entirely and read the numbers off **Slide 9**, which carries the verified
  figures. Do not debug `timeit` on air.
- **You're on Python 3.13 by accident (step 8).** The old message
  (`TypeError: unhashable type: 'list'`) still proves the hashable rule perfectly. Say "on 3.14 this
  message is much clearer" and carry on — do not try to fix your PATH live.
- **Whole demo unavailable (terminal dead / screen-share fails).** Slides 12–13 carry the full set
  algebra with its exact expected output; teach directly from them and point the cohort at the lab
  guide's Section 3, which reproduces the entire sequence step by step.

---

## Demo D14 — dict-of-tasks + `Counter` / `defaultdict`

**Backs:** the dict concepts and the `collections` first look (Slides 15–26; speaker-notes Segment 5).
**Scenario:** key the task app by id and summarise its tags. **This demo produces the exact model the
lab starts from and the capstone keeps.**

**Preconditions:** a fresh REPL (`python3.14`); VS Code open for the final file save.

> **All outputs in D14 are exact and reproducible** — dicts guarantee insertion order, and `Counter`
> inherits it. Unlike D13, you can rehearse these to the character.

### Steps

**1. Build the keyed task model.** Paste this — it's too long to type live comfortably.

```pycon
>>> tasks = {
...     1: {"title": "Buy milk", "done": False, "tags": ["shopping", "urgent"]},
...     2: {"title": "Write report", "done": False, "tags": ["work", "urgent"]},
...     3: {"title": "Call plumber", "done": True, "tags": ["home"]},
...     4: {"title": "Pay bills", "done": False, "tags": ["home", "urgent"]},
... }
```

Expected: no output, back to `>>>`. Narrate the shape: a dict of dicts — the key is the id, the value
is everything about that task.

**2. Direct access — the loop that disappeared.**

```pycon
>>> tasks[1]["title"]
'Buy milk'
```

Expected: `'Buy milk'`. **This is the emotional beat of the session** — last week this needed a `for`
loop scanning ids. Say that out loud: the loop isn't shorter, it's *gone*.

**3. Missing keys — the fork in the road.**

```pycon
>>> tasks[99]
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    tasks[99]
    ~~~~~^^^^
KeyError: 99
>>> tasks.get(99)
>>> tasks.get(99, "no such task")
'no such task'
>>> 99 in tasks
False
```

Expected: `KeyError: 99`; then `.get(99)` prints **nothing** (the REPL doesn't echo a bare `None`);
then the default string; then `False`. Narrate: `[...]` claims "it must be there", `.get` says "it
might be" — both are right in the right place.

**4. Iterate with `.items()`.**

```pycon
>>> for tid, t in tasks.items():
...     print(f"{tid}: {t['title']:<15} done={t['done']}")
...
1: Buy milk        done=False
2: Write report    done=False
3: Call plumber    done=True
4: Pay bills       done=False
```

Expected: exactly the four lines above, in that order (insertion order, guaranteed). Narrate: `.items()`
hands you key and value together, and the `for` line unpacks them — that's Week 6's tuple unpacking
showing up where it's useful.

**5. Views are live.**

```pycon
>>> ks = tasks.keys()
>>> ks
dict_keys([1, 2, 3, 4])
>>> tasks[5] = {"title": "Book flights", "done": False, "tags": ["travel", "urgent"]}
>>> ks
dict_keys([1, 2, 3, 4, 5])
```

Expected: `ks` gains `5` **without being reassigned**. Narrate: a view is a window, not a photograph.
Mention the consequence — don't add/remove keys while looping a dict; wrap in `list(...)` if you must.

**6. Flatten the tags, then rank with `Counter`.**

```pycon
>>> from collections import Counter, defaultdict
>>> all_tags = [tag for t in tasks.values() for tag in t["tags"]]
>>> all_tags
['shopping', 'urgent', 'work', 'urgent', 'home', 'home', 'urgent', 'travel', 'urgent']
>>> counts = Counter(all_tags)
>>> counts
Counter({'urgent': 4, 'home': 2, 'shopping': 1, 'work': 1, 'travel': 1})
>>> counts.most_common(2)
[('urgent', 4), ('home', 2)]
```

Expected: exactly as above. **These counts include task 5**, added in step 5 — so `urgent` is **4**
(tasks 1, 2, 4, 5) and `home` is **2** (tasks 3, 4). If you skipped step 5, `urgent` is 3 and
`travel` is absent. Narrate: `Counter` counted everything in one line, no loop, no `if`. Note the
output arrived **ranked, highest first** — worth calling out after all the "sets have no order" talk:
a `Counter` is a **dict**, so its output is stable and reproducible.

> The list comprehension in `all_tags` is next week's material. Say "that's a comprehension, we do
> them properly next week — for now read it as 'every tag from every task'" and move on. Do not teach
> it here; it costs you four minutes you don't have.

**7. Missing key on a `Counter` → `0`, and it does *not* insert.**

```pycon
>>> counts["never-seen"]
0
>>> "never-seen" in counts
False
```

Expected: `0`, then `False` — the key was **not** added by reading it. Narrate: a `Counter` is a dict,
so you'd expect a `KeyError`; it's deliberately friendlier, because for counting, zero is the right
answer. **Flag this now** — `defaultdict` is about to do the opposite, and the contrast is the point.

**8. Group with `defaultdict(list)`.**

```pycon
>>> by_tag = defaultdict(list)
>>> for tid, t in tasks.items():
...     for tag in t["tags"]:
...         by_tag[tag].append(tid)
...
>>> dict(by_tag)
{'shopping': [1], 'urgent': [1, 2, 4, 5], 'work': [2], 'home': [3, 4], 'travel': [5]}
```

Expected: exactly as above. Narrate: **no key-checking anywhere** — no "is this tag here yet, if not
make an empty list". The factory (`list`, passed **without parentheses**) makes one on demand.

Then show the sharp edge, live:

```pycon
>>> list(by_tag)
['shopping', 'urgent', 'work', 'home', 'travel']
>>> by_tag["does-not-exist"]
[]
>>> list(by_tag)
['shopping', 'urgent', 'work', 'home', 'travel', 'does-not-exist']
>>> by_tag.get("also-missing")
>>> list(by_tag)
['shopping', 'urgent', 'work', 'home', 'travel', 'does-not-exist']
```

Expected: **reading** `by_tag["does-not-exist"]` adds it; `.get("also-missing")` returns `None`
(prints nothing) and adds **nothing**. Narrate the contrast with step 7 explicitly: same action,
opposite behaviour, two dict subclasses from the same module. Use `in` or `.get` to peek, `[...]` to
create.

**9. Keys must be hashable (3.14's better error).**

```pycon
>>> {["a", "b"]: 1}
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    {["a", "b"]: 1}
TypeError: cannot use 'list' as a dict key (unhashable type: 'list')
>>> {("a", "b"): 1}
{('a', 'b'): 1}
```

Expected: the `TypeError` naming **'list' as a dict key** (note: "dict key" here, "set element" back
in D13 step 8 — 3.14 tailors the message to what you were doing), then the tuple key working.

**10. The `namedtuple` sketch.**

```pycon
>>> from collections import namedtuple
>>> Task = namedtuple("Task", ["id", "title", "done"])
>>> t = Task(1, "Buy milk", False)
>>> t.title
'Buy milk'
>>> t
Task(id=1, title='Buy milk', done=False)
>>> t._asdict()
{'id': 1, 'title': 'Buy milk', 'done': False}
```

Expected: exactly as above. Narrate: `task[1]` became `task.title`, and printing it now tells you what
it is. It's still a tuple underneath — immutable, unpackable. A stepping stone to Week 14's classes.

**11. Save the model to a file.** Switch to VS Code, create `tasks.py` in the demo folder:

```python
from collections import Counter, defaultdict

tasks = {
    1: {"title": "Buy milk", "done": False, "tags": ["shopping", "urgent"]},
    2: {"title": "Write report", "done": False, "tags": ["work", "urgent"]},
    3: {"title": "Call plumber", "done": True, "tags": ["home"]},
    4: {"title": "Pay bills", "done": False, "tags": ["home", "urgent"]},
}


def tag_counts(store):
    """Counter of tag -> number of tasks carrying it."""
    return Counter(tag for task in store.values() for tag in task["tags"])


def group_by_tag(store):
    """Map each tag to the list of task ids carrying it."""
    grouped = defaultdict(list)
    for task_id, task in store.items():
        for tag in task["tags"]:
            grouped[tag].append(task_id)
    return dict(grouped)


if __name__ == "__main__":
    for tag, count in tag_counts(tasks).most_common():
        print(f"{tag:<10} {count}")
```

Run it:

```bash
python3.14 tasks.py
```

Expected:

```
urgent     3
home       2
shopping   1
work       1
```

Expected: `urgent` first with 3, `home` 2, then the singletons. (This file has the **original four**
tasks — task 5 lived only in the REPL — so `urgent` is 3 here, not 4.) Narrate: this is the file the
lab starts from.

### End state

A keyed `tasks` dict where fetching a task takes **no searching**; `.items()` looping cleanly; a live
`Counter` tag ranking; a `defaultdict` grouping ids by tag with no key-checking; the
`Counter`-doesn't-insert vs `defaultdict`-does contrast seen live; the hashable rule with 3.14's
tailored message; a `namedtuple` sketch; and a saved `tasks.py` the cohort will extend in Lab 7. The
cohort should leave believing the dict-of-dicts **is** the task app now — through JSON in Week 12 and
on to the Week 16 capstone.

### Reset between runs

```bash
rm -f tasks.py          # or leave it — the lab starts from this exact file
```

```pycon
>>> exit()
```

Re-launch `python3.14` for a clean namespace. If you re-run D14 in the same REPL, **remember `tasks[5]`
from step 5 persists** and will shift every count in steps 6–8 by one — that's precisely why the file
in step 11 prints `urgent 3` while the REPL printed `urgent 4`. Start a fresh REPL if you want the
counts to match the file.

### Live-failure fallback

- **The `tasks` paste mangles indentation (step 1).** REPL paste of a multi-line dict is the single
  most likely failure today. Fallback: have `tasks.py` (step 11) **pre-written** in the demo folder
  and run `python3.14 -i tasks.py` — that executes the file and drops you into a REPL with `tasks`
  already defined. Rehearse this path; it's the safest way to run the whole demo.
- **The comprehension in step 6 draws questions you don't have time for.** Deflect once — "next week,
  I promise" — and if pressed, use the explicit loop instead:
  `all_tags = []` then `for t in tasks.values(): all_tags.extend(t["tags"])`. Same result, no new
  syntax.
- **Counts don't match the runbook.** Almost certainly `tasks[5]` from step 5 is still in the dict
  (see Reset). Don't debug — say "I added a task a moment ago, so the numbers moved", which is true
  and is itself a good point about live data.
- **You're on 3.13 (step 9).** The terser `TypeError: unhashable type: 'list'` still proves the rule.
  Note the 3.14 improvement verbally and move on.
- **Whole demo unavailable.** Slides 28–29 carry the `Counter`/`defaultdict` shapes with exact output;
  teach from them and point the cohort at the lab guide's Sections 4–6, which rebuild the entire model
  step by step.

---

## Sources

All commands, outputs, and version-specific behaviour executed and verified against **Python 3.14.6**
on **2026-07-16**:

- [Python 3.14 Tutorial — Data Structures (sets, dicts, comprehensions)](https://docs.python.org/3.14/tutorial/datastructures.html) — the empty-set `{}` rule and the "sets are unordered… different order than you expect" warning live here.
- [Python 3.14 — Set types (`set`, `frozenset`)](https://docs.python.org/3.14/library/stdtypes.html#set-types-set-frozenset) — `add`/`discard`/`remove`, and the operator-vs-method iterable rule.
- [Python 3.14 — Mapping types (`dict`)](https://docs.python.org/3.14/library/stdtypes.html#mapping-types-dict) — insertion order guaranteed since 3.7.
- [Python 3.14 — Dictionary view objects](https://docs.python.org/3.14/library/stdtypes.html#dictionary-view-objects) — views are dynamic.
- [Python 3.14 — `collections`](https://docs.python.org/3.14/library/collections.html) — `Counter`, `defaultdict` (`__missing__` is not called by `get()`), `namedtuple`.
- [What's New in Python 3.14 — improved unhashable-type error messages (gh-132828)](https://docs.python.org/3.14/whatsnew/3.14.html) — the `cannot use 'list' as a dict key` / `as a set element` text.
- [Python 3.14 — `timeit`](https://docs.python.org/3.14/library/timeit.html) — the membership-timing step.
- Membership-speed figures are the **trainer's own measurement** on Python 3.14.6, not a documented guarantee; the tutorial claims only "fast membership testing". Indicative complexity: [Python wiki — TimeComplexity](https://wiki.python.org/moin/TimeComplexity) (community wiki, **not** official docs).

> **Version note.** Material is pinned to **Python 3.14.6** (current stable, released 2026-06-10).
> **3.14.7 is scheduled for 2026-08-04** ([PEP 745](https://peps.python.org/pep-0745/)) — nothing in
> this session is expected to change, but re-check the banner version if you deliver after that date.
