# Demo Runbook — Session 1 · Getting Started & the Python Model

**Trainer-facing.** This is the exact, reproducible, on-screen runbook for the two live demos in
Week 1: **D1 — REPL tour & first script** and **D2 — `uv init` project + Ruff**. Every command,
its expected output, the end state learners should see, a reset between runs, and a live-failure
fallback are below. All commands verified against **Python 3.14.6**, **uv** (Astral), and **Ruff**
(Astral) as of **2026-06-22**.

> The demo machine drives the whole course. The `taskapp` you create in D2 is the seed of the
> capstone — keep the folder you create today; you re-open it every week.

---

## Pre-flight checklist (do this before the session starts)

Run through this 10–15 minutes before you go live. Tick every box.

**Accounts / keys.** None required. Everything in Week 1 is free, offline, local — no logins, no
API keys.

**Tools installed on the demo machine (verify versions):**

```bash
python3.14 --version      # -> Python 3.14.6
uv --version              # -> uv 0.x.x  (any recent uv)
uvx ruff --version        # -> ruff 0.x.x  (fetched on first run; pre-warm it now)
code --version            # VS Code present (used briefly to show the project)
```

- Expected: `python3.14 --version` prints `Python 3.14.6` (or a later 3.14.x).
- If `uvx ruff --version` has to download Ruff, **let it download now**, before going live, so the
  first in-session `uvx ruff` call is instant.
- If `python3.14` is not on PATH, use `uv run python` instead (uv supplies 3.14 via the project's
  `.python-version`), or run `uv python install 3.14` ahead of time.

**Sample data / repo state.**

- Create an **empty working folder** for today and `cd` into it, e.g. `~/pymastery-demos/week01`.
- Make sure there is **no** leftover `hello.py` or `taskapp/` from rehearsal in that folder (see
  *Reset* in each demo). A clean folder makes D2 dramatic.
- Have the **messy-code snippet** (below, in D2 step 5) on your clipboard or in a scratch file ready
  to paste.

**Window / zoom layout.**

- Terminal font **≥ 18pt**, high-contrast theme; terminal window large, roughly two-thirds screen.
- A file explorer or VS Code window ready to show the `taskapp` folder contents.
- Hide notifications / Do-Not-Disturb on.

**Recording.**

- **Start the Teams recording before you speak the first word of Slide 13** (the D1 hand-off) —
  ideally start it at the very top of the session. Students rebuild both demos from this recording.

---

## Demo D1 — REPL tour & first script

**Backs:** the "REPL vs script" concept and the bytecode/mental-model concept (Slides 12–17;
speaker-notes Segment 3). **Scenario:** meet Python by talking to it, then save your first script.

**Preconditions:** clean working folder; `python3.14` available (or use `uv run python`).

### Steps

**1. Launch the 3.14 REPL.**

```bash
python3.14
```

Expected: a banner like `Python 3.14.6 ... on <platform>` followed by the `>>>` prompt. In a
colour-capable terminal the prompt and your input render with **syntax highlighting on by default**.

> Windows equivalent: `py -3.14`. Inside a uv project you can also use `uv run python`.

**2. Evaluate arithmetic and text.**

```pycon
>>> 2 + 2
4
>>> "Python".upper()
'PYTHON'
>>> print("Hello from the REPL")
Hello from the REPL
```

Expected: each line answers instantly. Point out the colours as you type the string.

**3. Show import autocompletion (the crowd-pleaser).**

Type `import ma` and **press Tab** (do not type the rest):

```pycon
>>> import ma     # <Tab>
```

Expected: the REPL completes it to `import math`. Press Enter to run it. (Autocompletion in
`import` statements is **on by default** in the 3.14 REPL.)

> If Tab does not complete (some terminals/SSH sessions), narrate it and just type `import math`
> manually — the lesson still lands. See the fallback below.

**4. Use built-in help.**

```pycon
>>> help(len)
```

Expected: a short help page for `len` appears (`Help on built-in function len...`). Press `q` to
exit the pager if one opens.

**5. Leave the REPL.**

```pycon
>>> exit()
```

Expected: back to the shell prompt. (Mention `Ctrl-D` as the keyboard alternative on macOS/Linux;
`Ctrl-Z` then Enter on Windows.)

**6. Create the first script.** Create `hello.py` containing exactly:

```python
print("Hello, Python 3.14")
```

(Type it live in your editor, or `echo` it — but typing it live reinforces "scripts are saved code".)

**7. Run it by filename.**

```bash
python3.14 hello.py
```

Expected:

```text
Hello, Python 3.14
```

**8. Run it as a module with `-m`.**

```bash
python3.14 -m hello
```

Expected: **identical** output:

```text
Hello, Python 3.14
```

Narrate the difference: `hello.py` runs *this file by path*; `-m hello` runs *the module named
hello* found on the path (here, the current folder). Same result today; the `-m` form matters for
packages in Week 9.

### End state learners should see

The same greeting produced **two ways**, a REPL session that answered instantly and autocompleted an
import, and a clear felt difference between *exploring in the REPL* and *keeping code in a script*.

### Reset between runs

```bash
rm -f hello.py
```

(Delete `hello.py` so the next rehearsal starts clean. Nothing else to undo — the REPL leaves no state.)

### Live-failure fallback

- **Tab autocompletion doesn't fire** (terminal quirk): say so out loud, type `import math` by hand,
  and move on — the highlighting and instant evaluation already demonstrate the new REPL.
- **`python3.14` not found mid-demo:** fall back to `uv run python` (uv has 3.14 ready), or `py -3.14`
  on Windows. If all else fails, you have a **pre-captured screenshot** of the REPL session and the
  two-way run (keep `assets/d1-fallback.png` ready) to talk over.
- **No colour in the REPL** (e.g. plain terminal): mention that highlighting needs an ANSI-capable
  terminal, and that `PYTHON_BASIC_REPL=1` would force the old plain REPL — then continue; colour is
  a nicety, not the point.

---

## Demo D2 — `uv init` project + Ruff

**Backs:** the "modern toolchain" concept (Slides 18–26; speaker-notes Segments 4–5).
**Scenario:** turn a loose script into a real, tidy, runnable project — the seed of the capstone.

**Preconditions:** clean working folder with **no** existing `taskapp/`; uv installed; Ruff
pre-warmed via `uvx ruff` in pre-flight.

### Steps

**1. Scaffold the project.**

```bash
uv init taskapp
```

Expected: uv reports it initialized a project, e.g. `Initialized project `taskapp` at .../taskapp`.
A new `taskapp/` folder now exists.

**2. Move into it and tour the files.**

```bash
cd taskapp
ls -a
```

Expected to see:

```text
.git  .gitignore  .python-version  README.md  main.py  pyproject.toml
```

Walk through each: `.python-version` pins the Python version; `README.md` describes the project;
`main.py` is a ready-to-run starter; `pyproject.toml` holds the project's name and metadata. (uv also
initializes a git repo by default — mention it exists; we don't use git in Week 1.)

**3. Show the starter `main.py`.**

```bash
cat main.py
```

Expected:

```python
def main():
    print("Hello from taskapp!")


if __name__ == "__main__":
    main()
```

**4. Run the project.**

```bash
uv run main.py
```

Expected (first run may briefly create the environment, then):

```text
Hello from taskapp!
```

Narrate: uv silently created an isolated environment and ran the project inside it — no manual setup.

**5. Paste deliberately messy code, then format it.** Open `main.py` and **replace** its contents
with this intentionally ugly version (have it ready on your clipboard):

```python
import math
def main( ):
    name='taskapp'
    print( "Hello from "+name+"!" )
if __name__=='__main__':
    main()
```

Then run the formatter:

```bash
uvx ruff format
```

Expected:

```text
1 file reformatted
```

Re-open `main.py` and show it's been tidied — consistent spacing, double quotes, blank lines restored.

**6. Run the linter and let it flag the unused import.** (The pasted code imports `math` but never
uses it.)

```bash
uvx ruff check
```

Expected: a diagnostic pointing at the unused import, e.g.:

```text
main.py:1:8: F401 [*] `math` imported but unused
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

**7. Auto-fix the lint issue.**

```bash
uvx ruff check --fix
```

Expected:

```text
Found 1 error (1 fixed, 0 remaining).
```

Run it once more to show a clean bill of health:

```bash
uvx ruff check
```

Expected:

```text
All checks passed!
```

**8. Confirm it still runs.**

```bash
uv run main.py
```

Expected:

```text
Hello from taskapp!
```

### End state learners should see

A clean, formatted, lint-clean `taskapp` project that runs via `uv run` and prints
`Hello from taskapp!` — the seed of the capstone — with Ruff having both reformatted the file
(`1 file reformatted`) and reported `All checks passed!`.

### Reset between runs

```bash
cd ..
rm -rf taskapp
```

(Deleting the folder fully resets D2; the next `uv init taskapp` starts fresh.)

### Live-failure fallback

- **`uvx ruff` tries to download mid-demo and stalls:** this is why we pre-warm in pre-flight. If it
  still stalls, switch to a network-free path: if Ruff is installed globally, call `ruff format` /
  `ruff check` directly; otherwise show the **pre-captured screenshots** (`assets/d2-format.png`,
  `assets/d2-check.png`) of the expected output.
- **`uv init` produces `hello.py` instead of `main.py`** (very old uv): that's pre-0.6.0 behaviour —
  just narrate the filename difference; the lesson is unchanged. Update uv after the session.
- **The lint diagnostic doesn't appear** (Ruff defaults shifted and `F401` is off): add the unused
  import is normally caught by default; if not, run `uvx ruff check --select F` to force the pyflakes
  rules, which include unused-import. Fall back to the screenshot if needed.
- **Whole demo derails:** there's a `checkpoint` folder on the demo machine containing a ready-made,
  correct `taskapp/` — `cd` into it, run `uv run main.py` and `uvx ruff check` to show the end state,
  and move on.

---

## Post-demo state to leave on screen

Leave the formatted, lint-clean `taskapp` open in the editor as you transition to Q&A, so the cohort
sees the finished artifact while you talk through install troubleshooting and the Lab 1 brief.
