# Demo Runbook — Session 5 · Functions, Scope & Type Hints

**Trainer-facing.** This is the exact, reproducible, on-screen runbook for the two live demos in
Week 5: **D9 — the argument-grammar tour** and **D10 — scope/LEGB + type hints in VS Code**. Every
command, its expected output, the end state learners should see, a reset between runs, and a
live-failure fallback are below. All behaviour verified by running it against **Python 3.14.6**
(CPython), **Ruff 0.15.21**, **uv 0.11.28**, and the **Pyright 1.1.411** engine that Pylance ships,
on **2026-07-16**.

> **Two settings will silently break this session if you skip the pre-flight.** Read this before
> anything else:
>
> 1. **Pylance reports nothing at its default setting.** `python.analysis.typeCheckingMode` ships as
>    **`off`**. At `off` the wrong-typed call in D10 produces **zero** diagnostics — no squiggle, no
>    hover, nothing. You must set it to **`basic`** (verified: `off` → 0 errors, `basic` → 1 error).
> 2. **Ruff reports nothing at its default rule set.** `B006` (mutable default) is **not** in Ruff's
>    defaults (`E4`/`E7`/`E9`/`F`). Plain `ruff check` on the trap file prints `All checks passed!`.
>    You must pass `--select B006`.
>
> Both are handled in the pre-flight below, and both are *taught out loud* in the demos rather than
> hidden — "the useful checks are the ones you switch on" is a genuinely good lesson for the cohort.

> D9 is best driven in the **REPL** (instant feedback, and the error messages are the teaching).
> D10 starts in the REPL for scope and then **switches to VS Code** for the type-hint half — that
> editor switch is the whole point of the second demo, so have the window ready before you start.

---

## Pre-flight checklist (do this before the session starts)

Run through this 10–15 minutes before you go live. Tick every box.

**Accounts / keys.** None required. Everything in Week 5 is free, offline, and local — no logins, no
API keys, no third-party packages.

**Tools installed on the demo machine (verify versions):**

```bash
python3.14 --version      # -> Python 3.14.6  (or later 3.14.x)
uv --version              # -> uv 0.11.28     (fallback run path)
ruff --version            # -> ruff 0.15.21   (or: uvx ruff --version)
```

- Expected: `python3.14 --version` prints `Python 3.14.6` (or a later 3.14.x).
- If `python3.14` is not on PATH, use `uv run python` (uv supplies 3.14), or `py -3.14` on Windows.
- If `ruff` is not installed globally, every `ruff` command below works as `uvx ruff ...`.

**⚠️ Switch Pylance's type checking on — D10 fails silently without this.**

Open VS Code → `Ctrl/Cmd + Shift + P` → **Preferences: Open Workspace Settings (JSON)** and set:

```json
{
  "python.analysis.typeCheckingMode": "basic"
}
```

- Valid values are **`off`, `basic`, `standard`, `strict`** — and **only** those four. (`recommended`
  and `all` are *basedpyright* values; Pylance/Pyright reject them outright with
  `Config "typeCheckingMode" entry must contain "off", "basic", "standard", or "strict".`)
- **The default is `off`.** Verified across all four modes on the Pyright engine:

  | `typeCheckingMode` | Diagnostics on `greet(123, "Hello")` |
  |---|---|
  | **`off`** ← Pylance default | **0** — nothing is reported |
  | `basic` | 1 × `reportArgumentType` |
  | `standard` | 1 × `reportArgumentType` |
  | `strict` | 1 × `reportArgumentType` |

- **Verify it took** before you go live: open `typed.py` (below), and confirm a red squiggle appears
  under `123`. If there's no squiggle, the setting hasn't applied — reload the window
  (`Developer: Reload Window`) and check you edited the *workspace* settings for the folder you
  actually have open.
- Confirm the **Python interpreter** is 3.14 (status bar, bottom right) — `Python: Select Interpreter`.

> **Teaching note, not a problem to hide.** Turning this on *live* is worth 30 seconds of screen time:
> the cohort must do exactly this in their lab, and the lab guide walks them through it. Slide 31's
> talk track already says the default is `off`, so you're aligned either way.

**Sample data / repo state.**

- Open a clean working folder for today, e.g. `~/pymastery-demos/week05`, and `cd` into it.
- Create the three small files below **before** the session (D9 step 7 and all of D10 use them).
  Keeping them pre-made means you type the *teaching* code live and never fumble the scaffolding.

```bash
mkdir -p ~/pymastery-demos/week05 && cd ~/pymastery-demos/week05
```

`trap.py` — the mutable-default trap, for the Ruff step (D9, step 7):

```python
def add_item(item, basket=[]):
    basket.append(item)
    return basket
```

`typed.py` — the Pylance target (D10, step 5). Note the deliberately wrong call on the last line:

```python
def greet(name: str, greeting: str = "Hi") -> str:
    """Return a friendly greeting."""
    return f"{greeting}, {name}!"


greet(123, "Hello")
```

`scope.py` — the LEGB + closure file (D10, steps 1–4), if you prefer a file to the REPL:

```python
name = "global name"
debug = False


def outer():
    name = "enclosing name"

    def inner():
        return name

    return inner()


def make_counter():
    count = 0

    def increment():
        nonlocal count
        count += 1
        return count

    return increment


def enable_debug():
    global debug
    debug = True
```

**Window / zoom layout.**

- Terminal font **≥ 18pt**, high-contrast theme; terminal roughly two-thirds screen for D9.
- **VS Code open beside the terminal**, `typed.py` already loaded, editor font **≥ 16pt**
  (`Ctrl/Cmd + =` a few times). D10 lives in this window — don't go hunting for it mid-demo.
- Hide notifications / Do-Not-Disturb on. Close any other Python project folder (a stray
  `pyrightconfig.json` elsewhere can override your setting).

**Recording.**

- **Start the Teams recording before you speak the first word of Slide 9** (the D9 hand-off) —
  ideally start it at the very top of the session. Students rebuild both demos from this recording,
  and Lab 5 is a refactor that leans on D9 heavily.

**Note on REPL multi-line blocks.** When you type a `def` into the REPL, the prompt changes from
`>>>` to `...` for the indented body. Finish the block with a **blank line** (press Enter on an empty
`...` line) to run it. The cohort met this in Week 4, but re-mention it the first time it appears.

---

## Demo D9 — the argument-grammar tour

**Backs:** the argument-grammar concepts — positional, keyword, default, `*args`, `**kwargs`,
keyword-only, positional-only, and the mutable-default trap (Slides 9–16; speaker-notes Segment 3).
**Scenario:** one function, every parameter kind — grow a signature until it uses everything Python
has, then walk into the language's most famous trap on purpose.

**Preconditions:** REPL open (`python3.14`); `trap.py` present in the working folder (step 7).

### Steps

**1. Open the REPL.**

```bash
python3.14
```

Expected: the `Python 3.14.6 ...` banner and the `>>>` prompt.

> Windows: `py -3.14`. Inside a project: `uv run python`.

**2. Positional, keyword, and default arguments.**

```pycon
>>> def greet(name, greeting="Hi"):
...     return f"{greeting}, {name}!"
...
>>> greet("Asha")
'Hi, Asha!'
>>> greet("Asha", "Hello")
'Hello, Asha!'
>>> greet(greeting="Welcome", name="Ravi")
'Welcome, Ravi!'
```

Expected, line by line:

- `greet("Asha")` → `'Hi, Asha!'` — one positional argument; `greeting` falls back to its default.
- `greet("Asha", "Hello")` → `'Hello, Asha!'` — the second positional overrides the default.
- `greet(greeting="Welcome", name="Ravi")` → `'Welcome, Ravi!'` — **narrate the order**: the keywords
  are deliberately written back-to-front and it doesn't matter. Matching is by name, not position.

**3. Break it, and read the error out loud.**

```pycon
>>> greet()
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    greet()
    ~~~~~^^
TypeError: greet() missing 1 required positional argument: 'name'
>>> greet("Asha", tone="loud")
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    greet("Asha", tone="loud")
    ~~~~~^^^^^^^^^^^^^^^^^^^^^
TypeError: greet() got an unexpected keyword argument 'tone'
```

Expected: exactly those two `TypeError` messages. Point out how precise they are — the function name,
what's missing, and what kind of parameter it is. `name` has no default → required. `greeting` has one
→ optional.

> **Bonus if it comes up naturally:** a near-miss keyword gets a suggestion —
> `greet("Asha", greting="Yo")` → `TypeError: greet() got an unexpected keyword argument 'greting'. Did you mean 'greeting'?`

**4. `*args` and `**kwargs`.**

```pycon
>>> def report(*args, **kwargs):
...     return f"args={args} kwargs={kwargs}"
...
>>> report(1, 2, 3)
'args=(1, 2, 3) kwargs={}'
>>> report(1, 2, mode="fast", debug=True)
"args=(1, 2) kwargs={'mode': 'fast', 'debug': True}"
>>> report()
'args=() kwargs={}'
```

Expected: `*args` collects leftover **positionals into a tuple**; `**kwargs` collects leftover
**keywords into a dict**; both are optional and come back empty. Stress that the **stars are the
magic**, the names `args`/`kwargs` are only convention.

**5. Keyword-only parameters (after `*`).**

```pycon
>>> def make_task(title, *, priority="normal", done=False):
...     return f"{title!r} priority={priority} done={done}"
...
>>> make_task("Buy milk")
"'Buy milk' priority=normal done=False"
>>> make_task("Buy milk", priority="high")
"'Buy milk' priority=high done=False"
>>> make_task("Buy milk", "high")
Traceback (most recent call last):
  File "<python-input-11>", line 1, in <module>
    make_task("Buy milk", "high")
    ~~~~~~~~~^^^^^^^^^^^^^^^^^^^^
TypeError: make_task() takes 1 positional argument but 2 were given
```

Expected: the bare `*` fences off everything after it as keyword-only.

> **⚠️ Call this gotcha out — it's counter-intuitive.** The error does **not** say "keyword-only". It
> says `takes 1 positional argument but 2 were given`, because Python is counting positionals. If a
> student ever hits that error while "passing the right number of things", the fix is to go looking for
> a bare `*` in the signature. Slide 13's talk track already makes this point.

**6. Positional-only parameters (before `/`).**

```pycon
>>> def power(base, exp, /):
...     return base ** exp
...
>>> power(2, 10)
1024
>>> power(base=2, exp=10)
Traceback (most recent call last):
  File "<python-input-14>", line 1, in <module>
    power(base=2, exp=10)
    ~~~~~^^^^^^^^^^^^^^^^
TypeError: power() got some positional-only arguments passed as keyword arguments: 'base, exp'
>>> len(obj=[1, 2])
Traceback (most recent call last):
  File "<python-input-15>", line 1, in <module>
    len(obj=[1, 2])
    ~~~^^^^^^^^^^^^
TypeError: len() takes no keyword arguments
```

Expected: the `/` forbids naming. **The `len` line is the payoff** — connect it back to Week 1:
built-ins are positional-only, which is why `len(obj=...)` was never allowed. Note the message quotes
both names inside one string: `'base, exp'`.

> **Optional, if the room is with you — the full signature.** Shows the fixed order in one shot:
>
> ```pycon
> >>> def full(a, b, /, c, d="dee", *args, e, f="eff", **kwargs):
> ...     return f"a={a} b={b} c={c} d={d} args={args} e={e} f={f} kwargs={kwargs}"
> ...
> >>> full(1, 2, 3, "D", 9, 9, e="E", z=1)
> "a=1 b=2 c=3 d=D args=(9, 9) e=E f=eff kwargs={'z': 1}"
> >>> full(1, 2, 3)
> TypeError: full() missing 1 required keyword-only argument: 'e'
> ```

**7. The mutable-default trap — the centrepiece.**

```pycon
>>> def add_item(item, basket=[]):
...     basket.append(item)
...     return basket
...
>>> add_item("a")
['a']
>>> add_item("b")
['a', 'b']
>>> add_item("c")
['a', 'b', 'c']
```

Expected: `['a']`, then `['a', 'b']`, then `['a', 'b', 'c']`. **Pause on the second call** — that's the
moment of surprise. Let it land before explaining.

Then show *where the list lives*, which is the proof:

```pycon
>>> add_item.__defaults__
(['a', 'b', 'c'],)
```

Expected: the accumulating list, sitting on the function object. Narrate the rule: **defaults are
evaluated once, at `def` time** — one list, created when Python read the `def` line, shared by every
call that doesn't pass a basket.

**8. The `None` fix.**

```pycon
>>> def add_item(item, basket=None):
...     if basket is None:
...         basket = []
...     basket.append(item)
...     return basket
...
>>> add_item("a")
['a']
>>> add_item("b")
['b']
>>> add_item("c")
['c']
```

Expected: `['a']`, `['b']`, `['c']` — fresh every call, because the `[]` now runs **inside the body**,
on every call. Note *why* `None` is the right sentinel: it's immutable, and it unambiguously means
"the caller gave me nothing".

**9. Let Ruff catch it — and show that you must ask.** Leave the REPL (`exit()`) for this.

```bash
ruff check trap.py
```

Expected — and this is the point:

```console
All checks passed!
```

Now ask for the rule explicitly:

```bash
ruff check --select B006 trap.py
```

Expected (verified verbatim on Ruff 0.15.21):

```console
B006 Do not use mutable data structures for argument defaults
 --> trap.py:1:27
  |
1 | def add_item(item, basket=[]):
  |                           ^^
2 |     basket.append(item)
3 |     return basket
  |
help: Replace with `None`; initialize within function

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

Narrate both halves: Ruff's **default rule set is small and quiet** (`E4`/`E7`/`E9`/`F`), so the trap
sailed straight through the first time. `B006` is a real, stable rule you **opt in** to — either
`--select B006` on the command line, or in `pyproject.toml`:

```toml
[tool.ruff.lint]
extend-select = ["B006"]
```

> Use `uvx ruff check --select B006 trap.py` if Ruff isn't installed globally — identical output.

### End state learners should see

One function grown through every parameter kind — positional, keyword, default, `*args`, `**kwargs`,
keyword-only behind a `*`, positional-only behind a `/` — with each failure mode's exact `TypeError`
read aloud and understood. Then the mutable-default trap demonstrated *live* (`['a','b','c']` from
three "fresh" calls), its cause located on `__defaults__`, the `None` idiom fixing it, and Ruff's
`B006` catching it **only once explicitly selected**.

### Reset between runs

```bash
# The REPL holds no state once you exit:
exit()

# trap.py must stay BROKEN for step 9 to work. If you fixed it live, restore it:
cat > trap.py <<'EOF'
def add_item(item, basket=[]):
    basket.append(item)
    return basket
EOF
```

Re-defining `add_item` in step 8 simply rebinds the name — the old function (and its polluted
`__defaults__`) is discarded. To re-run step 7 cleanly after step 8, just retype the original `def`.

### Live-failure fallback

- **`ruff: command not found`:** use `uvx ruff check --select B006 trap.py` — same output, no install.
  If the network is down and `uvx` can't fetch Ruff, **skip the Ruff step entirely** and say so: the
  trap and the `None` fix are the lesson; Ruff is the bonus. Have a **screenshot** of the `B006` block
  (`assets/d9-b006.png`) to talk over.
- **`ruff check --select B006` prints `All checks passed!`:** you're pointing at the *fixed* file.
  Restore the broken `trap.py` from the reset block above.
- **The trap doesn't reproduce (each call returns a single-item list):** you've redefined `add_item`
  with the `basket=None` version already. Retype the original `def` with `basket=[]` and start step 7
  again.
- **Indentation breaks when typing a `def` live in the REPL:** finish the block with a blank line. If a
  paste mangles it, retype by hand a line at a time — these functions are two or three lines each.
- **`python3.14` not found mid-demo:** fall back to `uv run python` or `py -3.14`. Every block above is
  short enough to retype on a backup machine.
- **Whole demo derails:** known-good copies of all steps live in `checkpoint/d9.py` on the demo machine
  — `python3.14 -i checkpoint/d9.py` and re-run the calls to show the end state.

---

## Demo D10 — scope/LEGB + type hints in VS Code

**Backs:** scope and LEGB, `global`/`nonlocal`, closures, and the payoff of type hints (Slides 27–33;
speaker-notes Segment 6). **Scenario:** see where names live, then let the editor help.

**Preconditions:** REPL open for steps 1–4; **VS Code open with `typed.py` loaded and
`python.analysis.typeCheckingMode` set to `basic`** for steps 5–7 (see pre-flight — *verify the
squiggle exists before the session*).

### Steps

**1. A name resolving through the Enclosing scope.**

```pycon
>>> name = "global name"
>>> def outer():
...     name = "enclosing name"
...     def inner():
...         return name
...     return inner()
...
>>> outer()
'enclosing name'
```

Expected: `'enclosing name'`. Walk LEGB out loud: `inner` doesn't assign `name` → not Local → look
**Enclosing** → found in `outer` → **stop**. The global `name` is never consulted.

> To show the **G** step too, define a version with no enclosing `name`:
>
> ```pycon
> >>> def outer_no_e():
> ...     def inner():
> ...         return name
> ...     return inner()
> ...
> >>> outer_no_e()
> 'global name'
> ```
>
> And the **B** step: `>>> def uses_builtin(): return len([1, 2, 3])` → `uses_builtin()` → `3`.

**2. `UnboundLocalError` — the error every beginner hits.**

```pycon
>>> count = 0
>>> def broken():
...     count = count + 1
...     return count
...
>>> broken()
Traceback (most recent call last):
  File "<python-input-9>", line 1, in <module>
    broken()
    ~~~~~~^^
  File "<python-input-8>", line 2, in broken
    count = count + 1
            ^^^^^
UnboundLocalError: cannot access local variable 'count' where it is not associated with a value
```

Expected: exactly that `UnboundLocalError`. This is the highest-value 60 seconds of the demo —
narrate it fully: the **assignment** made `count` local for the *whole function*, decided at compile
time. So reading it on the right-hand side looks up the **local** `count`, which has no value yet. It
does **not** fall back to the global. That's what `UnboundLocalError` means.

**3. The `nonlocal` counter (a closure).**

```pycon
>>> def make_counter():
...     count = 0
...     def increment():
...         nonlocal count
...         count += 1
...         return count
...     return increment
...
>>> c = make_counter()
>>> c()
1
>>> c()
2
>>> c()
3
```

Expected: `1`, `2`, `3` — the count **survives between calls**, even though `make_counter` returned
long ago. That's the closure holding its enclosing scope alive.

Then prove each closure is independent:

```pycon
>>> c2 = make_counter()
>>> c2()
1
```

Expected: `1`, **not** `4`. A separate call to `make_counter` → a separate `count` → its own closure.

**4. Rip out `nonlocal` and watch it collapse.**

```pycon
>>> def make_counter_broken():
...     count = 0
...     def increment():
...         count += 1
...         return count
...     return increment
...
>>> make_counter_broken()()
Traceback (most recent call last):
  File "<python-input-20>", line 1, in <module>
    make_counter_broken()()
    ~~~~~~~~~~~~~~~~~~~~~^^
  File "<python-input-19>", line 4, in increment
    count += 1
    ^^^^^
UnboundLocalError: cannot access local variable 'count' where it is not associated with a value
```

Expected: the **same** `UnboundLocalError` from step 2 — and now the cohort can explain it themselves.
One word is the difference between a working closure and this error.

**Then, briefly, the `global` toggle:**

```pycon
>>> debug = False
>>> def enable_debug():
...     global debug
...     debug = True
...
>>> debug
False
>>> enable_debug()
>>> debug
True
```

Expected: `False` → `True`. Keep this short and keep the framing from Slide 20 — it works, it's loud,
and it's rare. Prefer a parameter and a return.

> **Optional — lambdas, if you have the time** (Slide 22 already covers it in the talk track):
>
> ```pycon
> >>> tasks = [("write report", 1), ("buy milk", 3), ("call bank", 2)]
> >>> sorted(tasks, key=lambda t: t[1])
> [('write report', 1), ('call bank', 2), ('buy milk', 3)]
> >>> sorted(tasks, key=lambda t: t[0])
> [('buy milk', 3), ('call bank', 2), ('write report', 1)]
> ```
>
> The two sorts give **genuinely different orders** — that's deliberate, it's what makes `key=` land.

**5. Switch to VS Code — Pylance flags the wrong-typed call.**

Bring up `typed.py` (created in pre-flight). It contains:

```python
def greet(name: str, greeting: str = "Hi") -> str:
    """Return a friendly greeting."""
    return f"{greeting}, {name}!"


greet(123, "Hello")
```

Expected: a **red squiggle under `123`**. Hover it and read the tooltip aloud:

> `Argument of type "Literal[123]" cannot be assigned to parameter "name" of type "str" in function "greet"`

**The point to hammer:** nothing has been run. No save, no run button, no terminal. The mistake is
flagged **while typing**. That's the payoff of the annotation.

> If there is **no squiggle**, `typeCheckingMode` is still `off` — see the fallback below. Fix it on
> screen; it's a 20-second teaching moment, not an embarrassment.

**6. Autocomplete on a typed parameter.**

Inside the function body, type `name.` and let the completion list appear.

Expected: the full **`str` method list** — `upper`, `lower`, `strip`, `split`, `replace`, `startswith`,
… — offered because Pylance knows `name` is a `str`.

Now delete the `: str` annotation on `name` and type `name.` again.

Expected: a markedly **vaguer, less useful** list — the editor no longer knows what `name` is. Put the
annotation back. This contrast is what makes hints feel worth typing.

**7. Prove the runtime doesn't care.** Back to the terminal:

```pycon
>>> def add(a: int, b: int) -> int:
...     return a + b
...
>>> add(2, 3)
5
>>> add("two", "three")
'twothree'
>>> add.__annotations__
{'a': <class 'int'>, 'b': <class 'int'>, 'return': <class 'int'>}
```

Expected: `add("two", "three")` returns `'twothree'` with **no error whatsoever** — `+` on two strings
concatenates, and Python obliges. Then `__annotations__` shows every hint **is** recorded on the
function object — read, stored, and **ignored at runtime**. Editor strict, runtime relaxed. That
contrast is the closing beat of the session.

> **3.14 note, only if a sharp student asks.** Since **PEP 649/749** (new in 3.14), annotations are
> **lazily evaluated** — `def f(x: NotDefinedYet)` now *defines fine* and only raises `NameError` when
> `f.__annotations__` is accessed, and forward references work without quotes. Don't volunteer this to
> a beginner cohort; it's Part 2 material. Just don't be caught out if someone probes.

### End state learners should see

Scope made visible: a name resolving **L → E → G → B** and stopping at the first hit; `UnboundLocalError`
triggered, explained, and then *re-triggered* by removing `nonlocal`; a closure counting `1, 2, 3`
across calls with a second, independent counter starting again at `1`; and a `global` toggle. Then the
concrete payoff of type hints: a wrong call underlined **before running**, a rich autocomplete list on
a typed parameter that degrades without it — and the same wrong call executing happily in the
interpreter with its annotations sitting untouched on `__annotations__`.

### Reset between runs

```bash
# REPL state vanishes on exit:
exit()

# typed.py must keep its WRONG call for the squiggle to appear. Restore if edited live:
cat > typed.py <<'EOF'
def greet(name: str, greeting: str = "Hi") -> str:
    """Return a friendly greeting."""
    return f"{greeting}, {name}!"


greet(123, "Hello")
EOF
```

If you deleted the `: str` annotation in step 6 and forgot to undo it, `Ctrl/Cmd + Z` in VS Code, or
restore from the block above. Leave `typeCheckingMode` at `basic` — you want it on for the next run.

### Live-failure fallback

- **⚠️ No red squiggle in VS Code (the most likely failure):** in order —
  1. Confirm `python.analysis.typeCheckingMode` is `basic` in **workspace** settings
     (`Ctrl/Cmd + Shift + P` → *Preferences: Open Workspace Settings (JSON)*).
  2. `Developer: Reload Window`.
  3. Check the Python interpreter (status bar) is 3.14 — not a stale venv.
  4. Check no stray `pyrightconfig.json` in the folder is overriding the setting (`rm` it).
  5. Still nothing? **Fall back to the CLI**, which proves the identical point with the same engine:
     ```bash
     uvx pyright --pythonversion 3.14 typed.py
     ```
     Expected: `error: Argument of type "Literal[123]" cannot be assigned to parameter "name" of type "str"...` and `1 error`.
     (Note the pyright **CLI** defaults to `standard`, so it flags this with no config at all.)
  6. Last resort: a **pre-captured screenshot** of the squiggle + hover tooltip
     (`assets/d10-pylance.png`) — take this during your rehearsal, not on the day.
- **Pylance is slow to start / "Loading…" in the status bar:** give it 10–15 seconds on first open;
  talk through the annotation syntax on Slide 31 while it warms up. Opening `typed.py` during
  pre-flight avoids this entirely.
- **The tooltip wording differs slightly:** Pylance's message text tracks its release (current:
  **2026.2.108**). The *shape* — "Argument of type X cannot be assigned to parameter Y of type Z" — is
  stable. Read what's on screen; don't read this doc aloud.
- **`UnboundLocalError` doesn't fire in step 2:** you almost certainly wrote `print(count)` instead of
  `count = count + 1`. Reading is fine — it's the **assignment** that makes it local. Retype it exactly.
- **The counter returns `1, 1, 1`:** you're calling `make_counter()()` each time (a fresh closure per
  call) instead of binding `c = make_counter()` once and calling `c()`. Bind it first.
- **Whole demo derails:** `scope.py` (pre-flight) has every scope construct ready —
  `python3.14 -i scope.py`, then `outer()`, `c = make_counter()`, `c()`, `enable_debug()`.

---

## Post-demo state to leave on screen

Leave **VS Code showing `typed.py`** with the red squiggle under `123` and the hover tooltip visible,
with the terminal beside it showing `add("two", "three")` returning `'twothree'`. That single split
screen *is* the argument of the session — the editor objecting and the runtime shrugging, side by
side — and it's the perfect visual to hold while you move into Q&A and the Lab 5 brief, since the
lab's first step is switching that same setting on.
