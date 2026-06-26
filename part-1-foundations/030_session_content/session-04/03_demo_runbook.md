# Demo Runbook — Session 4 · Control Flow & Pattern Matching

**Trainer-facing.** This is the exact, reproducible, on-screen runbook for the two live demos in
Week 4: **D7 — loops & `range` patterns** and **D8 — `match`/`case` command dispatcher**. Every
command, its expected output, the end state learners should see, a reset between runs, and a
live-failure fallback are below. All behaviour verified against **Python 3.14.6** (CPython), with
**uv** (Astral) available as the run path, as of **2026-06-25**.

> Both demos are best driven in the **REPL** (so the cohort sees instant results) plus one small
> saved file for the dispatcher. Re-open the running `taskapp` working folder you've used since Week 1
> so today's command-dispatcher feels like it belongs to the capstone you're all growing.

---

## Pre-flight checklist (do this before the session starts)

Run through this 10–15 minutes before you go live. Tick every box.

**Accounts / keys.** None required. Everything in Week 4 is free, offline, local — no logins, no
API keys, no third-party packages.

**Tools installed on the demo machine (verify versions):**

```bash
python3.14 --version      # -> Python 3.14.6  (or later 3.14.x)
uv --version              # -> uv 0.x.x  (fallback run path)
```

- Expected: `python3.14 --version` prints `Python 3.14.6` (or a later 3.14.x).
- If `python3.14` is not on PATH, use `uv run python` (uv supplies 3.14), or `py -3.14` on Windows.
- `match`/`case` needs Python **3.10+**; on 3.14 it is fully available and unchanged — no flags needed.

**Sample data / repo state.**

- Open a clean working folder for today, e.g. `~/pymastery-demos/week04`, and `cd` into it.
- Have the **dispatcher file** (D8, step 1) ready to type live, or in a scratch file you can paste from.
- Have the **command strings** for D8 step 3 ready: `add buy milk`, `done 7`, `done abc`, `ls`,
  `list`, an empty line, `frobnicate`.

**Window / zoom layout.**

- Terminal font **≥ 18pt**, high-contrast theme; terminal window large, roughly two-thirds screen.
- Editor (VS Code) open beside the terminal to show `dispatch.py` for D8.
- Hide notifications / Do-Not-Disturb on.

**Recording.**

- **Start the Teams recording before you speak the first word of Slide 11** (the D7 hand-off) —
  ideally start it at the very top of the session. Students rebuild both demos from this recording.

**Note on REPL multi-line blocks.** When you type a compound statement (a `for`, `while`, `if`, or
`match`) into the REPL, the prompt changes from `>>>` to `...` for the indented body. Finish the block
with a **blank line** (press Enter on an empty `...` line) to run it. Mention this to the cohort the
first time it happens so the `...` prompt isn't mysterious.

---

## Demo D7 — loops & `range` patterns

**Backs:** the `for`/`while`/`range` and `break`/`continue`/`pass`/loop-`else` concepts (Slides 7–15;
speaker-notes Segment 4). **Scenario:** iterate every common way and watch loop control behave.

**Preconditions:** REPL open (`python3.14`); colour-capable terminal preferred but not required.

### Steps

**1. Open the REPL.**

```bash
python3.14
```

Expected: the `Python 3.14.6 ...` banner and the `>>>` prompt.

> Windows: `py -3.14`. Inside a project: `uv run python`.

**2. A `for` over a `range` with a step.** Type each line; finish the block with a blank line.

```pycon
>>> for i in range(0, 6, 2):
...     print(i)
...
0
2
4
```

Expected: `0`, `2`, `4` — narrate that `range` started at 0, stepped by 2, and **stopped before 6**.

**3. A `for` walking a string.**

```pycon
>>> for ch in "abc":
...     print(ch)
...
a
b
c
```

Expected: `a`, `b`, `c` — the `for` walks the sequence item by item, no counting needed.

**4. A `while` countdown with a sentinel.**

```pycon
>>> count = 3
>>> while count > 0:
...     print(count)
...     count -= 1
...
3
2
1
```

Expected: `3`, `2`, `1`. Stress that `count -= 1` is what moves the loop toward its exit — omit it and
you get an infinite loop.

**5. `continue` to skip evens, `break` to stop early.**

```pycon
>>> for n in range(10):
...     if n % 2 == 0:
...         continue
...     if n > 5:
...         break
...     print(n)
...
1
3
5
```

Expected: `1`, `3`, `5`. Narrate: even numbers were skipped by `continue`; when `n` reached `7`
(`> 5`), `break` ended the whole loop before printing.

**6. The loop-`else`, miss case (no `break`).**

```pycon
>>> target = 7
>>> for n in [1, 3, 5]:
...     if n == target:
...         print("found")
...         break
... else:
...     print("not found")
...
not found
```

Expected: `not found` — the loop never hit `break`, so the `else` ran.

**7. The loop-`else`, hit case (with `break`).** Change the target to show the contrast.

```pycon
>>> target = 3
>>> for n in [1, 3, 5]:
...     if n == target:
...         print("found")
...         break
... else:
...     print("not found")
...
found
```

Expected: `found` only — the `break` fired, so the `else` was **skipped**. This side-by-side is the
moment loop-`else` clicks for the cohort.

### End state learners should see

Every loop construct run live: a counted `for` with `range`, a `for` over a string, a `while`
countdown, `continue`/`break` steering one loop, and the loop-`else` firing in the miss case but being
skipped in the hit case.

### Reset between runs

Nothing to undo — the REPL holds no files. To start fresh, type `exit()` and relaunch `python3.14`,
or just keep going (each block is self-contained). Re-define `target` between the two loop-`else`
runs as shown.

### Live-failure fallback

- **The `...` continuation prompt confuses the room or a block won't run:** remind everyone (and
  yourself) to end an indented block with a **blank line**. If a paste mangles indentation, retype the
  block by hand a line at a time.
- **No colour / highlighting in the REPL:** purely cosmetic; mention it needs an ANSI-capable
  terminal and carry on — the loop behaviour is the lesson, not the colours.
- **`python3.14` not found mid-demo:** fall back to `uv run python` or `py -3.14`. If all else fails,
  the loop blocks above are short enough to paste into any 3.14 REPL on a backup machine.

---

## Demo D8 — `match`/`case` command dispatcher

**Backs:** the `match`/`case` concept — literal, capture, wildcard, sequence, or-, mapping patterns,
and guards (Slides 16–26; speaker-notes Segments 5–6). **Scenario:** route a typed command to an
action by its shape — the exact dispatcher the capstone's menu reuses.

**Preconditions:** Python 3.14 available; editor open to type `dispatch.py`. Best shown as a saved
file (so the whole `match` is visible at once) imported into the REPL, or run directly.

### Steps

**1. Create `dispatch.py`.** Type it live (it reinforces "specific cases above general ones"):

```python
def dispatch(command):
    match command.split():
        case ["add", *words]:
            return f"added: {' '.join(words)}"
        case ["done", id] if id.isdigit():
            return f"completed #{id}"
        case ["list"] | ["ls"]:
            return "showing all tasks"
        case []:
            return "type a command"
        case _:
            return "unknown command"
```

> Optional: add a mapping case to show dict-shaped input too. Place it above `case _:`:
> `case {"cmd": c}: return f"running: {c}"` — then call `dispatch` differently (see step 4).

**2. Load it into the REPL.** From the folder containing `dispatch.py`:

```bash
python3.14 -i dispatch.py
```

Expected: the file runs (defining `dispatch`) and drops you at a `>>>` prompt with `dispatch` ready to
call. (The `-i` flag means "run the file, then stay interactive".)

> Alternative: open `python3.14`, then `from dispatch import dispatch`.

**3. Route several inputs live.**

```pycon
>>> dispatch("add buy milk")
'added: buy milk'
>>> dispatch("done 7")
'completed #7'
>>> dispatch("done abc")
'unknown command'
>>> dispatch("ls")
'showing all tasks'
>>> dispatch("list")
'showing all tasks'
>>> dispatch("")
'type a command'
>>> dispatch("frobnicate")
'unknown command'
```

Expected, line by line:

- `add buy milk` → `'added: buy milk'` (literal `"add"` + `*words` tail capture).
- `done 7` → `'completed #7'` (the guard `id.isdigit()` passes).
- `done abc` → `'unknown command'` — narrate this one: the shape `["done", id]` matched, but the
  **guard failed** because `"abc"` isn't digits, so Python moved on and hit the catch-all.
- `ls` and `list` → both `'showing all tasks'` (the `|` or-pattern handles both spellings).
- `""` → `'type a command'` (empty `split()` is `[]`, matched by `case []`).
- `frobnicate` → `'unknown command'` (single word, no case fits, falls to `case _:`).

**4. (Optional) Show a mapping pattern.** If you added the `{"cmd": c}` case in step 1, demonstrate it
by calling a variant that matches a dict — easiest is to define a second tiny function in the REPL:

```pycon
>>> def run(payload):
...     match payload:
...         case {"cmd": c}:
...             return f"running: {c}"
...         case _:
...             return "unknown"
...
>>> run({"cmd": "sync", "force": True})
'running: sync'
```

Expected: `'running: sync'` — narrate that the extra `"force"` key was simply **ignored**; the mapping
pattern only requires the `"cmd"` key to be present.

### End state learners should see

One `match` statement routing five-plus distinct command shapes — literal, capture, sequence,
or-pattern, (optional) mapping — with a guard demonstrating that a matched shape can still be rejected,
and a `case _:` catch-all so nothing crashes. Students recognise this as the menu dispatcher their
capstone will use.

### Reset between runs

```bash
# nothing persists; to start clean from the shell:
rm -f dispatch.py        # only if you want to retype it fresh
```

(The REPL holds no state once you exit. If re-running step 2, just relaunch `python3.14 -i dispatch.py`.)

### Live-failure fallback

- **`SyntaxError` on the `match` block:** confirm you're on Python **3.10+** (`python3.14 --version`);
  on 3.14 it's always available. If you somehow launched an older Python, switch to `python3.14`
  explicitly or `uv run python`.
- **Indentation breaks when typing the `match` live in the REPL:** type the function into the **editor
  file** instead and load it with `python3.14 -i dispatch.py` — a saved file sidesteps all REPL
  continuation-prompt fiddliness, and the whole `match` is visible at once.
- **A case routes unexpectedly (e.g. `done 7` returns "unknown"):** the usual cause is case **order** —
  a broader case placed above a narrower one. Show the cohort the order rule live by pointing at the
  cases top to bottom; keep `case _:` last. Have a **pre-captured screenshot** (`assets/d8-runs.png`)
  of the correct seven-line output to talk over if the live run derails.
- **Whole demo derails:** a known-good `dispatch.py` lives in the `checkpoint/` folder on the demo
  machine — `python3.14 -i checkpoint/dispatch.py` and re-run the calls to show the end state.

---

## Post-demo state to leave on screen

Leave `dispatch.py` open in the editor alongside the REPL showing the seven routed calls, so the
cohort sees the whole `match` statement and its outputs together while you transition into Q&A and the
Lab 4 brief. It's a clean visual bridge to the menu they'll build this week.
