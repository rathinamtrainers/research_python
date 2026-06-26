# Demo Runbook — Session 3 · Strings & Text

**Trainer-facing.** This is the exact, reproducible, on-screen runbook for the two live demos in
Week 3: **D5 — slicing & method chaining** and **D6 — f-string formatting + t-string contrast**.
Every command, its expected output, the end state learners should see, a reset between runs, and a
live-failure fallback are below. All commands verified against **Python 3.14.6** (which ships
**t-strings / PEP 750** and the improved REPL) as of **2026-06-25**.

> Both demos run almost entirely in the **3.14 REPL**. The only version-sensitive piece is the
> t-string in D6 — it requires Python **3.14 or newer**; confirm your REPL banner says `3.14.x`
> before you start, or the `t"..."` line will raise a `SyntaxError`.

---

## Pre-flight checklist (do this before the session starts)

Run through this 10–15 minutes before you go live. Tick every box.

**Accounts / keys.** None required. Everything in Week 3 is free, offline, and local — no logins,
no API keys, no network needed once Python is installed.

**Tools installed on the demo machine (verify versions):**

```bash
python3.14 --version      # -> Python 3.14.6  (MUST be 3.14.x for t-strings)
```

- Expected: `python3.14 --version` prints `Python 3.14.6` (or a later 3.14.x).
- The **t-string demo (D6) needs Python 3.14+**. If the banner shows 3.13 or earlier, the `t"..."`
  line will be a `SyntaxError`. Fix this before going live: use `uv run python` (uv supplies 3.14 via
  `.python-version`), or `py -3.14` on Windows, or `uv python install 3.14`.

**Confirm the t-string works *now*, before the session** (this is the one risky line — pre-test it):

```bash
python3.14 -c 'name="Eggs"; print(type(t"Item: {name}"))'
```

- Expected: `<class 'string.templatelib.Template'>`.
- If this prints that line, D6 will work live. If it errors, your Python is not 3.14 — switch
  interpreters before you start.

**Sample data / repo state.**

- No files needed for D5 (pure REPL). For D6 you stay in the REPL too — no project setup required.
- Have the **messy grocery string** and the **report values** (below) ready to type or paste, so you
  don't fumble them live.
- Optional: open the `taskapp` folder from Week 1 so students see the running project is still there;
  today's work is in the REPL, but `report.py` (the lab) will live beside it.

**Window / zoom layout.**

- Terminal font **≥ 18pt**, high-contrast theme; terminal window large, roughly two-thirds screen.
- A **colour-capable terminal** so the 3.14 REPL's syntax highlighting shows — it makes strings,
  numbers, and the `b'...'` / `Template(...)` output much easier to read on screen.
- Hide notifications / Do-Not-Disturb on.

**Recording.**

- **Start the Teams recording before you speak the first word of Slide 11** (the D5 hand-off) —
  ideally start it at the very top of the session. Students rebuild both demos from this recording.

---

## Demo D5 — slicing & method chaining

**Backs:** the str-literals / indexing / slicing / methods / immutability concepts (Slides 5–14;
speaker-notes Segment 3). **Scenario:** clean and reshape a messy line of text, then prove the
original was never changed.

**Preconditions:** a 3.14 REPL open (`python3.14`, or `uv run python`, or `py -3.14`).

### Steps

**1. Open the REPL.**

```bash
python3.14
```

Expected: a banner like `Python 3.14.6 ... on <platform>` followed by the `>>>` prompt, with syntax
highlighting on by default.

> Windows: `py -3.14`. Inside a uv project: `uv run python`.

**2. Create the messy string.**

```pycon
>>> raw = "  Buy Milk, Eggs, Bread  "
>>> raw
'  Buy Milk, Eggs, Bread  '
```

Expected: the value echoes back **with** its leading/trailing spaces. Point out the spaces and the
mixed casing — that's the mess we'll clean.

**3. Chain strip + lower.**

```pycon
>>> raw.strip().lower()
'buy milk, eggs, bread'
```

Expected: spaces trimmed *and* text lowercased, in one line. Narrate: `strip()` returned a new
string, and `lower()` acted on that — two methods, one after another.

**4. Split into clean items (chaining inside a comprehension).**

```pycon
>>> items = [w.strip() for w in raw.strip().split(",")]
>>> items
['Buy Milk', 'Eggs', 'Bread']
```

Expected: a list of three tidy items, each with its surrounding spaces removed. (`split(",")` breaks
on the commas; the per-item `strip()` cleans the leftover spaces.)

> If the comprehension feels too advanced for the cohort this early, do it the long way instead:
> `parts = raw.strip().split(",")` then `items = [p.strip() for p in parts]`, or even a plain `for`
> loop appending to a list. The lesson is split/clean/rebuild, not the comprehension.

**5. Index and slice a single word.**

```pycon
>>> word = "Bread"
>>> word[0]
'B'
>>> word[-1]
'd'
>>> word[:3]
'Bre'
>>> word[::-1]
'daerB'
```

Expected: first character, last character, first three characters, and the reversed string. Narrate
that indexing starts at 0, `-1` is the last, the slice stop is excluded, and `::-1` reverses.

**6. Rebuild a clean string with join.**

```pycon
>>> ", ".join(items)
'Buy Milk, Eggs, Bread'
```

Expected: the list glued back into one clean string with `", "` between items. `join` is the inverse
of `split`.

**7. Prove immutability — the original is unchanged.**

```pycon
>>> raw
'  Buy Milk, Eggs, Bread  '
```

Expected: `raw` **still has its original spaces and casing**. Every method above returned a *new*
string; none changed `raw`.

**8. (Optional) show the immutability error explicitly.**

```pycon
>>> raw[0] = "X"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

Expected: a `TypeError` — a string cannot be changed in place. This is *why* methods return new
strings.

### End state learners should see

A messy string transformed by **chaining** into a clean list and back into a clean string, plus the
original string echoed back **unchanged**, and (optionally) the `TypeError` that proves strings are
immutable.

### Reset between runs

```pycon
>>> raw = "  Buy Milk, Eggs, Bread  "
```

(Just re-assign `raw`, or restart the REPL with Ctrl-D / `exit()` then `python3.14`. The REPL holds
no other state.)

### Live-failure fallback

- **The comprehension in step 4 trips you up or confuses the room:** fall back to the long form — a
  plain `for` loop appending stripped pieces to a list. Same result, simpler to read live.
- **No colour in the REPL:** mention highlighting needs an ANSI-capable terminal and that
  `PYTHON_BASIC_REPL=1` forces the plain REPL — then carry on; colour is a nicety, not the lesson.
- **Whole demo derails:** keep a **pre-captured screenshot** of the full chaining session
  (`assets/d5-chaining.png`) to talk over, and move on to D6.

---

## Demo D6 — f-string formatting + t-string contrast

**Backs:** the f-string / format-mini-language / t-string concepts (Slides 15–24; speaker-notes
Segment 4). **Scenario:** format a tidy, aligned report row, then contrast the same interpolation as
an f-string versus a t-string.

**Preconditions:** a **Python 3.14** REPL open (the t-string lines require 3.14+ — confirm the banner
or you'll hit a `SyntaxError`). Continue in the same REPL as D5, or open a fresh one.

### Steps

**1. Set up three report values.**

```pycon
>>> name, qty, price = "Eggs", 12, 3.5
```

Expected: no output; three variables now hold a name, quantity, and price.

**2. Build an aligned report row with the format mini-language.**

```pycon
>>> f"{name:<10}{qty:>4}{price:>8.2f}"
'Eggs        12    3.50'
```

Expected: the name left-aligned in 10 columns, the quantity right-aligned in 4, the price
right-aligned in 8 with two decimals (`3.50`). Read the spec aloud: `<10` left-pad to 10, `>4`
right-align in 4, `>8.2f` right-align in 8 with 2 decimal places.

**3. Show thousands grouping on a big number.**

```pycon
>>> f"{1234567.5:>12,.2f}"
'1,234,567.50'
```

Expected: thousands commas, two decimals, right-aligned in 12. The `,` is the grouping flag.

**4. Show the self-documenting `{value=}` debug trick.**

```pycon
>>> f"{qty=}"
'qty=12'
>>> f"{qty * 2 =}"
'qty * 2 =24'
```

Expected: the expression text *and* its value, printed together. Narrate: this is the fast way to
debug — no hand-written labels.

**5. Write the same interpolation as an f-string (baseline).**

```pycon
>>> f"Item: {name}"
'Item: Eggs'
```

Expected: an ordinary, finished `str`. Stress: the text is fully built right now.

**6. Now change one letter — make it a t-string.**

```pycon
>>> t"Item: {name}"
Template(strings=('Item: ', ''), interpolations=(Interpolation('Eggs', 'name', None, ''),))
```

Expected: **not** a string — a `Template` object that prints its `strings` (the static parts) and
`interpolations` (the values) separately. (The exact repr formatting may vary slightly by point
release; the key is that it's a `Template`, not `'Item: Eggs'`.)

**7. Reveal the type.**

```pycon
>>> type(t"Item: {name}")
<class 'string.templatelib.Template'>
```

Expected: `<class 'string.templatelib.Template'>`. This is the payoff line — f-strings make `str`,
t-strings make `Template`.

**8. (Optional) show that a Template keeps parts separate.**

```pycon
>>> tmpl = t"Item: {name}"
>>> tmpl.strings
('Item: ', '')
>>> tmpl.values
('Eggs',)
```

Expected: `strings` holds the literal text around the holes; `values` holds the interpolated value.
This is *why* a library can sanitise values before assembling the final text — the one-sentence
reason t-strings exist.

### End state learners should see

A single f-string producing an aligned, grouped, two-decimal report row; the `{value=}` debug trick;
and the **same** interpolation shown as an f-string (`'Item: Eggs'`, a `str`) versus a t-string (a
`string.templatelib.Template`) — so the cohort can state the difference in one sentence.

### Reset between runs

```pycon
>>> name, qty, price = "Eggs", 12, 3.5
```

(Re-assign the three values, or restart the REPL. No files or external state to clean.)

### Live-failure fallback

- **`t"..."` raises `SyntaxError` mid-demo:** your interpreter is not 3.14 — this is the one thing the
  pre-flight test catches. Switch to `uv run python` or `py -3.14` (both supply 3.14) and re-run. If
  you can't switch live, show the **pre-captured screenshot** (`assets/d6-tstring.png`) of the
  `type(...)` → `Template` result and narrate it; the concept still lands.
- **The `Template(...)` repr looks different from the slide:** that's fine — point at the class name
  in the `type()` output (step 7) as the source of truth; the repr layout is cosmetic.
- **Columns don't line up on screen** (proportional font): make sure the terminal uses a monospaced
  font; alignment only *looks* right in a fixed-width font. Bump the font size if needed.
- **Whole demo derails:** keep `assets/d6-format.png` (the aligned row + grouping) and
  `assets/d6-tstring.png` (the f-vs-t contrast) ready to talk over.

---

## Post-demo state to leave on screen

Leave the REPL showing the f-string-vs-t-string contrast — the aligned report row above and the
`type(t"...")` → `Template` line — visible as you move into the bytes/encodings concept and then Q&A,
so the cohort keeps the comparison in view while you wrap up.
