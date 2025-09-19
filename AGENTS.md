# Repository Guidelines

## Project Structure & Module Organization
- `redbot/` holds core runtime; `redbot/cogs/` contains built-in cogs and shared helpers; keep new logic under existing cog folder rather than adding new top-level packages.
- `tests/` mirrors the runtime layout (`tests/core`, `tests/cogs`) for unit tests; place fixtures in `tests/conftest.py`.
- `docs/` contains Sphinx sources; update relevant `.rst` when changing user-facing behavior.

## Build, Test, and Development Commands
- `make newenv` builds a fresh virtualenv in `.venv` and installs editable Red plus dev tooling.
- `make syncenv` refreshes dev dependencies when requirements change.
- `tox` runs the full gate (pytest on supported Python versions, docs build, style diff).
- `tox -e docs` checks documentation warnings; `tox -e style` ensures Black formatting before pushing.
- During iteration run `pytest tests/core/test_scheduler.py -k scenario` for focused suites.

## Coding Style & Naming Conventions
- Python code is auto-formatted with Black (line length 99, Python 3.8 target). Run `make reformat` before committing.
- Prefer explicit async-aware APIs from `redbot.core` and keep imports sorted logically.
- Name modules and cogs with lowercase underscores (`cogs/myfeature/__init__.py`); class names remain PascalCase, command names match invocation strings.
- Add type hints where practical; `py.typed` signals consumers rely on them.

## Testing Guidelines
- Tests use `pytest` plus the `redbot.pytest` plugin; structure new tests next to the feature (`tests/cogs/<cog>/test_*.py`).
- Write regression coverage for each bugfix; mark async tests with `pytest.mark.asyncio` only when the plugin cannot auto-detect.
- Use `tox -e py311` or `pytest --maxfail=1 --disable-warnings` locally before submitting.

## Commit & Pull Request Guidelines
- Follow the existing history pattern: concise, imperative subjects with optional scope tags (e.g. `[Audio] Fix track reconnect`); reference issues via `(#1234)` when applicable.
- Squash draft commits before review; keep PRs focused and describe behavior changes, migration steps, and test evidence in the PR body.
- Include screenshots or logs for UX-affecting changes and note any configuration updates required for self-hosters.

## Security & Configuration Tips
- Never commit tokens or guild-specific data; load secrets via environment variables or config files outside the repo.
- Report vulnerabilities through the process in `SECURITY.md` and avoid discussing them in public issues until triaged.
