# Demo Runbook — Session 2 · Numbers, Variables & Expressions

**Trainer-facing.** This is the exact, reproducible, on-screen runbook for the two live demos in
Week 2: **D3 — REPL calculator & precedence** and **D4 — floating-point surprises & fixes**. Every
command, its expected output, the end state learners should see, a reset between runs, and a
live-failure fallback are below. All output verified against **Python 3.14.6** as of **2026-06-25**;
`math.isclose` (PEP 485, present since 3.5) and the binary-float behaviour are stable across all
3.14.x releases.

> Both demos run entirely in the **REPL** plus one tiny script for the tip calculator. Keep your
> `taskapp` folder from Week 1 handy — the Lab 2 script `calculator.py` is created beside it, and you
> can run the tip script from inside that project with `uv run`.

---

## Pre-flight checklist (do this before the session starts)

Run through this 10–15 minutes before you go live. Tick every box.

**Accounts / keys.** None required. Everything in Week 2 is free, offline, local — no logins, no API
keys.

**Tools installed on the demo machine (verify versions):**

```bash
python3.14 --version      # -> Python 3.14.6
uv --version              # -> uv 0.x.x  (any recent uv)
```

- Expected: `python3.14 --version` prints `Python 3.14.6` (or a later 3.14.x).
- If `python3.14` is not on PATH, use `uv run python` instead (uv supplies 3.14 via the project's
  `.python-version`), or `py -3.14` on Windows.

**Sample data / repo state.**

- Open a terminal in a clean working folder for today, e.g. `~/pymastery-demos/week02`. The REPL
  leaves no files behind, so no cleanup is needed before D3.
- For the tip step at the end of D4, have the small `tip.py` script (below) ready to type or paste.
- Have your Week 1 `taskapp/` folder reachable, in case you want to show running the script with
  `uv run` from inside the project.

**Window / zoom layout.**

- Terminal font **≥ 18pt**, high-contrast theme; terminal window large, roughly two-thirds screen.
- An editor window (VS Code) ready to type the tiny `tip.py` script live.
- Hide notifications / Do-Not-Disturb on.

**Recording.**

- **Start the Teams recording before you speak the first word of Slide 10** (the D3 hand-off) —
  ideally start it at the very top of the session. Students rebuild both demos from this recording.

---

## Demo D3 — REPL calculator & precedence

**Backs:** the numeric-types, operators, and precedence concepts (Slides 5–14; speaker-notes
Segments 2–3). **Scenario:** use Python as a calculator and watch precedence decide the answer.

**Preconditions:** a REPL open; `python3.14` available (or `uv run python` / `py -3.14`).

### Steps

**1. Launch the REPL.**

```bash
python3.14
```

Expected: a banner like `Python 3.14.6 ... on <platform>` and the `>>>` prompt, with syntax
highlighting on in a colour-capable terminal.

> Windows: `py -3.14`. Inside a uv project: `uv run python`.

**2. The three divisions and the power operator, side by side.**

```pycon
>>> 7 / 2
3.5
>>> 7 // 2
3
>>> 7 % 2
1
>>> 2 ** 10
1024
```

Expected exactly as shown. Narrate: `/` always returns a `float`; `//` floors to a whole number;
`%` is the remainder; `**` is power.

**3. Show that `/` always floats, even on an even divide.**

```pycon
>>> 4 / 2
2.0
>>> 6 // 2
3
```

Expected: `4 / 2` is `2.0` (a float), while `6 // 2` is `3` (an int). This is the point students
most often miss.

**4. The floor-division + modulo pair (whole groups + leftover).**

```pycon
>>> 17 // 5
3
>>> 17 % 5
2
```

Expected: three whole groups of five, with two left over. Mention `3 * 5 + 2 == 17`.

**5. Precedence: with and without parentheses.**

```pycon
>>> 2 + 3 * 4
14
>>> (2 + 3) * 4
20
```

Expected as shown. Ask the cohort to predict `2 + 3 * 4` before pressing Enter.

**6. Power groups right to left (the surprise).**

```pycon
>>> 2 ** 3 ** 2
512
```

Expected: `512`, because `**` is right-associative — Python computes `3 ** 2 == 9` first, then
`2 ** 9 == 512` (not `(2 ** 3) ** 2 == 64`).

**7. Mixing int and float widens to float.**

```pycon
>>> 2 + 3.0
5.0
>>> type(2 + 3.0)
<class 'float'>
```

Expected: `5.0`, and the type is `float`.

**8. Augmented and multiple assignment.**

```pycon
>>> x = 5
>>> x += 3
>>> x
8
>>> a, b = 1, 2
>>> a, b = b, a
>>> a, b
(2, 1)
```

Expected: `x` becomes `8`; the swap leaves `a == 2`, `b == 1` with no temporary variable.

### End state learners should see

A REPL session where each operator's result is unmistakable (`/` floats, `//` floors, `%` remainder,
`**` power and right-associative), where parentheses visibly change the answer, where mixing types
widens to `float`, and where augmented and multiple assignment (including the one-line swap) work.

### Reset between runs

```pycon
>>> exit()
```

Nothing on disk to undo — the REPL leaves no state. Relaunch `python3.14` for a fresh session.

### Live-failure fallback

- **`python3.14` not found mid-demo:** fall back to `uv run python`, or `py -3.14` on Windows. If all
  else fails, talk over a **pre-captured screenshot** of the REPL session (`assets/d3-fallback.png`).
- **No colour in the REPL** (plain terminal): mention highlighting needs an ANSI-capable terminal,
  then continue — colour is a nicety, not the point.
- **A result looks off because of an earlier rebind:** type `exit()` and relaunch for a clean REPL,
  then re-run the line.

---

## Demo D4 — floating-point surprises & fixes

**Backs:** the floating-point, variables/`None`, and input-casting concepts (Slides 15–23;
speaker-notes Segments 4–5). **Scenario:** the famous `0.1 + 0.2`, then the three defenses, then a
real tip calculation from user input.

**Preconditions:** a REPL open; an editor ready to type `tip.py`.

### Steps

**1. The surprise.**

```pycon
>>> 0.1 + 0.2
0.30000000000000004
>>> 0.1 + 0.2 == 0.3
False
```

Expected exactly as shown: the sum prints `0.30000000000000004`, and the `==` comparison is `False`.
Explain in one plain paragraph: decimals are stored as binary fractions and most don't fit exactly,
so tiny errors accumulate. Stress this is not a Python flaw — nearly every language does the same.

**2. Defense 1 — `round` for a clean value.**

```pycon
>>> round(0.1 + 0.2, 2)
0.3
```

Expected: `0.3`. The `2` means "round to two decimal places."

**3. Defense 2 — `math.isclose` to compare.**

```pycon
>>> import math
>>> math.isclose(0.1 + 0.2, 0.3)
True
```

Expected: `True`. Narrate the golden rule: never compare floats with `==`; use `math.isclose`.

**4. Defense 3 — format for display.**

```pycon
>>> f"{0.1 + 0.2:.2f}"
'0.30'
```

Expected: the string `'0.30'`. Note this is a preview of next week's f-strings — display only.

**5. `None` and `is None` (variables segment tie-in).**

```pycon
>>> result = None
>>> result is None
True
```

Expected: `True`. Mention you test for `None` with `is None`, not `== None`.

**6. Show the input-is-text trap, then the cast fix.**

```pycon
>>> age = "42"          # this is what input() would hand you
>>> age + 1
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
>>> int(age) + 1
43
```

Expected: the `TypeError` on `age + 1`, then `43` after casting. (Using a literal `"42"` here avoids
waiting on live `input()` typing; mention that `input()` returns exactly this kind of string.)

**7. The tip calculator — read, cast, compute, round.** In the editor, create `tip.py`:

```python
bill = float(input("Bill amount? "))
percent = float(input("Tip percent? "))
tip = bill * percent / 100
total = bill + tip
print(f"Tip: {round(tip, 2)}")
print(f"Total: {round(total, 2)}")
```

Run it:

```bash
python3.14 tip.py
```

Type `50` then `15` at the two prompts. Expected:

```text
Bill amount? 50
Tip percent? 15
Tip: 7.5
Total: 57.5
```

> You can also run it from inside the Week 1 project: drop `tip.py` in `taskapp/` and run
> `uv run tip.py`.

Narrate: `input()` returned text, we cast both with `float()`, computed the tip, and **rounded** the
results so no float noise reaches the user.

### End state learners should see

A clear demonstration that `0.1 + 0.2` is `0.30000000000000004`; the three defenses working
(`round`, `math.isclose`, format); the `input()`-returns-text trap and its `float()`/`int()` cast
fix; and a working tip script that reads two numbers and prints cleanly-rounded results.

### Reset between runs

```bash
rm -f tip.py
```

(Delete `tip.py` so the next rehearsal starts clean; `exit()` the REPL to clear any rebound names.)

### Live-failure fallback

- **`0.1 + 0.2` prints `0.3` (not the long form):** this only happens on an unusual non-CPython
  build; on the demo machine's CPython 3.14.6 it prints `0.30000000000000004`. If it ever doesn't,
  show `(0.1 + 0.2) - 0.3` (which prints a tiny nonzero number like `5.551115123125783e-17`) to make
  the imprecision visible, then continue.
- **`input()` hangs or you fat-finger a prompt:** Ctrl-C to abort and re-run `python3.14 tip.py`; or
  fall back to the literal-string demonstration in step 6, which needs no typed input.
- **Whole demo derails:** there's a `checkpoint` folder on the demo machine with a ready-made,
  correct `tip.py` and a screenshot of the `0.1 + 0.2` REPL session (`assets/d4-floats.png`) — run
  the script or talk over the screenshot and move on.

---

## Post-demo state to leave on screen

Leave the `tip.py` run visible (the `Tip:` / `Total:` lines) as you transition to Q&A, so the cohort
sees a complete, correctly-rounded calculator while you talk through the arithmetic gotchas and the
Lab 2 brief.
