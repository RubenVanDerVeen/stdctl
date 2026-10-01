# Project standards

Human-readable summary of the standards this repository follows. Aimed at contributors who do not use an AI coding agent or the `project-standardization` skill.

For the agent-facing operating notes (when to load which reference, bootstrap checklist, etc.), see the skill itself. **This file is the human contract; the skill is the agent contract.**

---

## Stack

Two layers: formal ISO/IEC/IEEE norms and industry conventions.

| Standard / convention   | Applied here? | Used for |
|-------------------------|---------------|----------|
| ISO/IEC/IEEE 26515:2018 | no            | Agile documentation process |
| ISO/IEC/IEEE 26514:2022 | no            | User-documentation structure |
| ISO/IEC/IEEE 29119-3:2021 | no          | Test plan / spec / report templates |
| ISO/IEC/IEEE 15289:2019 | no            | Lifecycle information items (source vs deliverable) |
| ISO 10007:2017          | no            | Configuration management |
| ISO 8601                | **yes**       | `YYYY-MM-DD` filename prefix for time-based records |
| IEEE article format     | no            | Research articles |
| Kebab-case ASCII paths  | **yes**       | All directory and filenames |
| English structural paths | **yes**      | Dir names in English; content may be Dutch |
| Conventional Commits 1.0.0 | **yes**    | Commit messages |
| Conventional Branch 1.1.0 | **yes**    | Git branch names |
| Keep a Changelog 1.1.0  | **yes**       | `CHANGELOG.md` format |
| SemVer 2.0.0            | **yes** (when shipped) | Version numbers for releases |
| Shellcheck (.shellcheckrc + pre-commit gate) | **yes** | Bash style for `bin/stdctl` |

---

## Naming rules: the floor

These apply to every file and directory.

### Kebab-case ASCII

Lowercase letters, digits, and hyphens only. No spaces, no underscores, no PascalCase, no non-ASCII characters.

```
OK    remote-controller/
OK    deployment-diagram.drawio
OK    esp32-c3-datasheet.pdf
BAD   Remote Controller/
BAD   Deployment_Diagram.drawio
BAD   ESP32_C3_Datasheet.pdf
```

Exceptions for conventional uppercase filenames: `README.md`, `AGENTS.md`, `CLAUDE.md` (one-line shim that `@import`s `AGENTS.md` for Claude Code, which does not read `AGENTS.md` natively), `CHANGELOG.md`, `STANDARDS.md`, `LICENSE`, `Makefile`, `Cargo.toml`, `package.json`. Everything else is kebab-case.

### English structural paths

Directory and file **names** in English. Document **content** may be Dutch where the deliverable requires it. Dutch acronyms that are the proper name of a deliverable (`pve`, `top`) are permitted.

### ISO 8601 date prefix

Time-based records start with the date:

```
OK    2026-05-09-standup.md
OK    2026-05-06-sprint-retrospective.md
BAD   Standup_2026-05-09.md
BAD   09-05-2026-standup.md
```

The date must come first so sort-by-filename produces chronological order.

---

## Commit messages: Conventional Commits 1.0.0

Format: `<type>(<scope>): <description>`.

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`.

```
OK    feat(remote-controller): add wireless pairing
OK    fix(motor): correct current calculation
OK    docs(readme): translate to English
BAD   Update stuff
BAD   wip
```

Scope is the **module or component**, not the discipline. `feat(remote-controller)` not `feat(electrical)`.

Enforcement: a tracked `commit-msg` hook (`.githooks/commit-msg`) rejects non-conforming subjects. Activate once per clone: `git config core.hooksPath .githooks` (installed by the `project-standardization` bootstrap, step 9).

---

## Branches: Conventional Branch 1.1.0

Format: `<type>/<description>`. Specification: <https://conventionalbranch.org/>.

- Lowercase letters, digits, hyphens. Dots only for release versions (`release/v1.2.0`). No underscores, no spaces, no consecutive, leading, or trailing separators.
- Trunk branches (`main`, `master`, `develop`) carry no prefix.
- Spec prefixes: `feature/` (`feat/`), `bugfix/` (`fix/`), `hotfix/`, `release/`, `chore/`. Extra types mirroring Conventional Commits (`docs/`, `refactor/`, `test/`, `ci/`) are a sanctioned team extension: document any custom type here so tooling and teammates know it.

```
OK    feat/add-login-page
OK    fix/header-bug
BAD   Feature/Add-Login    (uppercase)
BAD   my-branch            (no type prefix)
```

---

## Changelog: Keep a Changelog 1.1.0

`CHANGELOG.md` at repo root. Grouped by version (semver) or milestone (sprint). Sections: `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`.

See `CHANGELOG.md` for the current state.

## Versioning: SemVer 2.0.0

Applies when the project ships versioned releases (Tauri apps, CLIs, libraries, installers). Skip for sub-projects versioned through a parent.

- **Canonical source + sync targets**: declared in `AGENTS.md` -> `### Versioning`. All sync targets are bumped in the same commit as the canonical source.
- **Policy**: SemVer 2.0.0 strict. During 0.x, `0.X+1.0` MAY break; `0.X.Y+1` is backwards-compatible only. After 1.0, standard semver (major / minor / patch = break / feature / fix).
- **Bump trigger**: `[Unreleased]` in `CHANGELOG.md` accumulates changes during development; cutting a version is a deliberate act (rename heading + bump sources + optional tag).

Full bump decision table and release-cut recipe: `references/versioning.md` in the `project-standardization` skill. See `CHANGELOG.md` for the release history.

---

## Repository layout

```
project-root/
+- AGENTS.md                  # agent context file (auto-loaded)
+- README.md                  # user-facing
+- CHANGELOG.md               # release notes (Keep a Changelog 1.1.0)
+- STANDARDS.md               # this file
+- CLAUDE.md                  # one-line shim that imports AGENTS.md
+- bin/stdctl                 # the only source file: bash CLI
+- docs/artifacts/            # per-feature specs/plans/reviews (locally pre-created)
+- .githooks/                 # tracked commit-msg + pre-commit hooks
+- .editorconfig
+- .shellcheckrc
```

Single-source-file project. No `src/`, no `lib/`, no per-rule modules. New rules live as additional functions inside the same file.

---

## Source vs deliverable

Not applicable: this project has no Typst or other source-to-PDF pipeline. The CLI is a single bash script, distributed as-is.

---

## Specs, plans, reviews

Primary artifact home is the skills repo: `docs/artifacts/features/stdctl/` (spec + history).

Local `docs/artifacts/{features,reviews,choices}/` are pre-created deliberately (user override of "never pre-create empty"). Reviews log flat under `docs/artifacts/reviews/`.

- `docs/artifacts/features/<feature>/YYYY-MM-DD-<topic>-design.md`: design spec.
- `docs/artifacts/features/<feature>/YYYY-MM-DD-<topic>-plan.md`: implementation plan.
- `docs/artifacts/features/<feature>/YYYY-MM-DD-<topic>-outline.md`: decomposition outline (multi-plan topics).
- `docs/artifacts/features/<feature>/YYYY-MM-DD-<topic>-manifest.md`: dispatch manifest (multi-plan topics).
- `docs/artifacts/features/<feature>/YYYY-MM-DD-<topic>-report.md`: execution report for a completed plan.
- `docs/artifacts/reviews/YYYY-MM-DD-<topic>-review.md`: audits and reviews.

When delegating to superpowers (`brainstorming`, `writing-plans`) or GSD, name the canonical path (`docs/artifacts/features/<feature>/...`) instead of the framework default (`docs/superpowers/...`, `.planning/...`); both frameworks accept the override. A `docs/superpowers/`, `.planning/`, or other framework-native directory should never land in this repo. If one does, `git rm` it.

Each is append-only history. If a spec changes mid-implementation, edit in place + add an `## Amendments` section. Do not create a second spec for the same topic.

---

## Forbidden patterns

- No `temp/`, no `old/`, no `archive/` directories. Git history is the archive.
- No secrets in tracked files. `.env`, tokens, passwords stay out of git.
- No PascalCase or spaces in any filename. Rename on import.
- No splitting the CLI into multiple files. Single-file constraint is binding.

---

## References

- Full standards-stack rationale: research paper <https://portfolio.rvdv-lab.nl/research.html?id=project-standaardenpakket-voor-het-idp-project> (local copy at `docs/research/<paper>.pdf` when present).
- Conventional Commits 1.0.0: <https://www.conventionalcommits.org/en/v1.0.0/>
- Conventional Branch 1.1.0: <https://conventionalbranch.org/>
- Keep a Changelog 1.1.0: <https://keepachangelog.com/en/1.1.0/>
- SemVer 2.0.0: <https://semver.org/spec/v2.0.0.html>
- ISO 8601 date format: <https://www.iso.org/iso-8601-date-and-time-format.html>
- `AGENTS.md` convention: <https://agents.md>