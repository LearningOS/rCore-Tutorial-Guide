# AGENTS.md

This file is for coding agents working in `rCore-Tutorial-Guide`.
It documents how to build/verify the docs and how to keep changes consistent.

## 1) Repository Overview

- Project type: Sphinx documentation repository (Chinese content, ReST source).
- Main source directory: `source/`.
- Main Sphinx config: `source/conf.py`.
- Build entrypoint: `Makefile`.
- CI workflow: `.github/workflows/deploy.yml`.
- Target output: `build/html` (GitHub Pages deployment).

## 2) Environment and Toolchain

- Prefer `uv` for environment management.
- Pin Python to `3.8` for compatibility with old Sphinx stack.
- Use dependencies from `requirements.txt` exactly unless intentionally upgrading.

Recommended setup:

```bash
uv venv --python 3.8
source .venv/bin/activate
uv pip install -r requirements.txt
```

CI reference:

- GitHub Actions deploy workflow uses Python `3.8.16`.
- Keep local verification close to CI to reduce surprises.

## 3) Build / Lint / Test Commands

This repo does not have a traditional unit-test suite.
Validation is primarily done by building docs and checking warnings.

### 3.1 Full build

```bash
make html
```

or use `uv run` without activating the venv:

```bash
uv run sphinx-build -M html source build
```

### 3.2 Clean + rebuild

```bash
make clean && make html
```

Use after structural/toctree changes.

### 3.3 Lint/format status

This repository has no standalone lint/test pipeline for docs content.
Use doc build as the source of truth, and keep Python formatting aligned with `ruff.toml`.

### 3.4 Notes about expected warnings

- This repo currently emits three known warnings: "文档没有加入到任何目录树中".
- The affected files are:
  - `source/chapter1/5exercise.rst`
  - `source/chapter2/5exercise.rst`
  - `source/honorcode.rst`
- These files are intentionally retained; do not delete them just to silence warnings.
- When reporting build results, distinguish these known warnings from newly introduced warnings.

## 4) Commands to Avoid

- Do not run destructive Git commands (`reset --hard`, force pushes) unless explicitly requested.
- Do not introduce unrelated dependency upgrades in doc-only changes.
- Do not switch Python version casually; this repo intentionally tracks an older Sphinx stack.

## 5) Code Style and Editing Rules

### 5.1 General

- Keep edits minimal and scoped to the requested task.
- Preserve existing document structure and heading hierarchy.
- Prefer ASCII unless the file already uses non-ASCII (this repo contains Chinese text, so Chinese is expected in prose).

### 5.2 ReST (`*.rst`)

- Use consistent section underlines and indentation.
- Keep code blocks with explicit language (`.. code-block:: bash`, etc.).
- Maintain current Chinese prose style for explanations.
- Use English inside command/code blocks.
- Prefer concise actionable instructions over conceptual digressions.
- When adding optional/legacy flows, label clearly with `.. note::` or `.. attention::`.

### 5.3 Python config (`source/conf.py`)

- File is configuration, not app logic: prioritize readability and low diff noise.
- Keep import style as existing (`from ... import ...`).
- Keep naming consistent with Sphinx conventions (`html_*`, `templates_path`, etc.).
- Avoid unnecessary refactors in lexer section unless required.

### 5.4 Formatting / Ruff

- Repository includes `ruff.toml`.
- Current relevant settings:
  - `line-length = 200`
  - `format.quote-style = 'single'`
- When editing Python files, keep style aligned with this config to reduce diff churn.

### 5.5 CSS (`source/_static/*.css`)

- Follow existing simple style: 4-space indentation, readable property grouping.
- Add styles narrowly (scoped selectors), avoid global overrides unless necessary.
- Preserve theme compatibility with Furo.

### 5.6 Shell snippets / scripts

- Commands should be copy-paste ready.
- Prefer `uv` commands for Python env/dependency operations.
- Use safe defaults and explicit paths where ambiguity exists.

## 6) Naming Conventions

- ReST filenames: lowercase, hyphen-separated (existing convention).
- CSS classes: kebab-case or existing upstream class names.
- Python names in `conf.py`: follow Sphinx/config conventions; avoid custom abstractions.

## 7) Error Handling and Verification Expectations

- For docs changes, "error handling" means build-time robustness:
  - no broken directives,
  - no malformed code blocks,
  - no invalid references where avoidable.
- Before finalizing, run at least one build command.
- Do not remove intentionally retained files to suppress known warnings.

## 8) Dependency Policy

- Keep `requirements.txt` minimal and intentional.
- If upgrading deps, explain compatibility rationale in PR notes.
- Align with CI Python version when changing dependency pins.

## 9) GitHub / CI Notes

- Deploy workflow builds docs on pushes to `main` and `dev`.
- Output published from `build/html`.
- For warnings from intentionally retained legacy pages, keep files as-is and mention the warning context in PR notes.

## 10) Rule Files Check

Checked for additional agent-rule files:

- `.cursor/rules/`: not present.
- `.cursorrules`: not present.
- `.github/copilot-instructions.md`: not present.

If these files are added later, merge their constraints into this document.

## 11) Practical Agent Workflow

1. Read target `.rst` and nearby sections for tone/structure.
2. Edit only needed files.
3. Run full `make html` during iteration (or `uv run sphinx-build -M html source build`).
4. If warnings appear, verify whether they are expected legacy warnings before changing files.
5. Summarize changed files and any remaining warnings.
