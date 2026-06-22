# Topic Source — Python (basics to expert)

> No source documentation folder was supplied for this run. Per the `training_01` workflow,
> the authoritative aspect list was assembled directly from the **current official Python
> documentation** (web-verified) and recorded here so the brochure scope is traceable.
> **Last verified: 2026-06-11.**

## Current Python version (web-verified)

- **Python 3.14.6** — released 2026-06-10; current stable branch is **3.14** (3.14.0 final: Oct 2025).
- Documentation root: <https://docs.python.org/3/> (tracks 3.14.x).
- Version status: <https://devguide.python.org/versions/>.

## Authoritative documentation surfaces used as the aspect list

| # | Document | URL | What it scopes |
|---|----------|-----|----------------|
| 1 | The Python Tutorial | https://docs.python.org/3/tutorial/index.html | Fundamentals → intermediate teaching order |
| 2 | The Python Language Reference | https://docs.python.org/3/reference/index.html | Data model, execution model, grammar, imports |
| 3 | The Python Standard Library | https://docs.python.org/3/library/index.html | Built-ins, stdlib modules, every library aspect |
| 4 | What's New in Python 3.14 | https://docs.python.org/3/whatsnew/3.14.html | Current-version feature surface (deltas) |
| 5 | typing — Type hints | https://docs.python.org/3/library/typing.html | Static typing system |
| 6 | asyncio | https://docs.python.org/3/library/asyncio.html | Async concurrency |
| 7 | concurrency (threading / multiprocessing / concurrent.futures) | https://docs.python.org/3/library/concurrency.html | Parallelism & concurrency |
| 8 | Extending and Embedding the Python Interpreter | https://docs.python.org/3/extending/index.html | C extensions |
| 9 | Python/C API Reference Manual | https://docs.python.org/3/c-api/index.html | CPython C API |
| 10 | Python Packaging User Guide | https://packaging.python.org/ | Packaging, pyproject.toml, distribution |
| 11 | The Python Profilers | https://docs.python.org/3/library/profile.html | cProfile / profiling |
| 12 | unittest / doctest | https://docs.python.org/3/library/unittest.html | Stdlib testing |

## Python 3.14-era features that must be reflected in scope (web-verified)

- **Free-threaded CPython (no-GIL)** now an officially supported build — **PEP 779**.
- **Template strings (t-strings)** — **PEP 750** (`Template` objects; safe interpolation).
- **Deferred / lazy evaluation of annotations** — **PEP 649 / PEP 749**; use `annotationlib.get_annotations()`.
- **Experimental JIT compiler** (copy-and-patch) — opt-in.
- **`except` / `except*` without parentheses** for multiple exception types (when no `as`).
- **`InterpreterPoolExecutor`** (multiple-interpreters, PEP 734) in `concurrent.futures`.
- **`forkserver`** now default start method for `ProcessPoolExecutor` on Unix (non-macOS).
- **Exception groups & `except*`** — PEP 654 (continues from 3.11).
- Improved interactive REPL, colorized tracebacks, `python -m asyncio ps PID` task inspection.

## Third-party tooling referenced (web-verified current, 2026-06)

- **uv** (Astral) — fast packaging/venv/project manager; widely adopted for 3.14 workflows.
- **Ruff** — linter + formatter (Astral).
- **pytest** — de-facto testing framework.
- **mypy** / **pyright (Pylance)** — static type checkers.
- **cffi**, **ctypes**, **Cython**, **pybind11** — C interop.
- **PyPy** — alternative JIT interpreter.
