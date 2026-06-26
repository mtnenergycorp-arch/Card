# CLAUDE.md

Guidance for Claude Code (and other AI assistants) working in this repository.

> **Status: starter scaffold.** This repository is currently empty — no source
> code has been committed yet. This file documents what is known today (the git
> and branch workflow) and provides clearly-marked `TODO` sections to be filled
> in as the codebase grows. **When you add real code, replace the relevant
> `TODO` sections below** (re-running an `/init`-style analysis is a good way to
> regenerate them from the actual project).

## Repository overview

- **Name:** `card`
- **Owner:** `mtnenergycorp-arch`

<!-- TODO: Describe the project's purpose — what `card` is, who uses it, and
     the problem it solves. One or two paragraphs. -->

## Codebase structure

<!-- TODO: Populate once source code exists. Example:

    .
    ├── src/            # application source
    ├── tests/          # test suite
    ├── docs/           # documentation
    └── ...

     Note the entry point(s), where core logic lives, and any non-obvious
     organization. -->

_No source files yet._

## Development workflow

<!-- TODO: Fill in the real commands once tooling is set up. Document at least:

  - **Setup / install dependencies:** e.g. `npm install`, `pip install -e .`
  - **Build:** e.g. `npm run build`, `make`
  - **Run locally:** e.g. `npm start`, `python -m card`
  - **Test:** e.g. `npm test`, `pytest`
  - **Lint / format:** e.g. `npm run lint`, `ruff check`, `prettier --write`

     Prefer documenting how to run a *single* test, since that is the most
     common iteration loop. -->

_No build, run, test, or lint tooling configured yet._

## Conventions

<!-- TODO: Capture conventions a contributor must follow but couldn't guess:

  - **Language(s) / runtime version**
  - **Code style & formatting** (and whether it's enforced by a tool)
  - **Naming conventions**
  - **Testing conventions** (framework, where tests live, expectations for
    new code)
  - **Commit message style**, if any -->

_To be established._

## Git & branch workflow

These conventions apply now:

- **Develop on a feature branch.** The current working branch is
  `claude/claude-md-docs-nszxoz`. Create branches off the default branch for new
  work rather than committing directly to it.
- **Push with upstream tracking:** `git push -u origin <branch-name>`.
- **Never push to a different branch** than the one you were asked to work on
  without explicit permission.
- **Do not open a pull request** unless explicitly requested.
- Write clear, descriptive commit messages.

## Notes for AI assistants

- This file is a scaffold. Once real code is committed, **replace the `TODO`
  sections** with accurate, specific guidance derived from the actual codebase
  (structure, real build/test/lint commands, and observed conventions).
- Keep this file concise and current — prefer documenting the things a newcomer
  could not easily discover on their own over restating the obvious.
