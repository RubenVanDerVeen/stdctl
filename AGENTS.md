# stdctl: Agent Notes

Context for AI coding agents that follow the `agents.md` convention (opencode, Codex, Cursor, Aider, GitHub Copilot, Hermes, etc.) working on this repo. Claude Code requires the `CLAUDE.md` shim at the repo root that imports this file. Loaded automatically each session. Start here.

## What this is

Single-file bash CLI that lints any repository against the project-standardization convention (source: the `project-standardization` skill in the skills repo). One subcommand: `check`. Rules: floor (G1-G7), structure + tier (S1-S5), skills-collection (K1-K2). Tier: small.

## Stack

- **Language**: Bash + coreutils only.
- **Source**: single file `bin/stdctl`.
- **Dependencies**: none. No build step.

## Critical conventions

### File structure

- Single-file constraint: `bin/stdctl` is the only source file. Do not split into modules, do not introduce `lib/`, `src/`, or per-rule files. New rules live inside the same file as additional `g*` / `s*` / `k*` functions wired into the dispatcher.
- `bin/stdctl` must be shellcheck-clean (`.shellcheckrc` + the `.githooks/pre-commit` P8 gate enforce this; bypass with `--no-verify` only).
- Indentation: tabs. `.editorconfig` enforces. No mixed indent anywhere.

### Rule additions

- Each rule ID belongs to exactly one of the three rule families (G/S/K). Adding one without updating the spec in the skills repo and the rule list in `bin/stdctl` (usage banner + dispatcher) is a bug.
- Rule IDs are monotonic and never renumbered (G8, S6, K3, ...). They are public, cited by skill docs.

## Adding features, modules, or components

Catalogs to keep in sync when a rule family, rule, or related artefact is added:

- `bin/stdctl`: single-file implementation + rule-list in the usage banner.
- Skills-repo spec under `docs/artifacts/features/stdctl/` (rules G1-G7, S1-S5, K1-K2 + future additions).

Red flags (any one = stop and fix before committing):

- A rule family or rule ID exists in `bin/stdctl` but is missing from the skills-repo docs.
- The rule list in `bin/stdctl`'s usage banner disagrees with the skills-repo spec.
- A rule ID was renumbered (existing IDs are stable contracts).

## Git & workflow

- Repo: https://github.com/RubenVanDerVeen/stdctl
- **No commit/push without explicit user instruction.** Default: every commit waits for the user.
- **Carve-out: spec/plan-driven development.** When the user has approved both a spec and a plan under `docs/artifacts/features/stdctl/`, the agent commits at task boundaries the plan specifies.
- **Default to a feature branch.** `feat/<scope>`, `fix/<scope>`, `chore/<scope>` per Conventional Branch 1.1.0. Small fixes (typos, single-line tweaks, dep bumps, docs-only edits) may land directly on the default branch.
- Commit messages: Conventional Commits 1.0.0 (`<type>(<scope>): <description>`).
- Commits are enforced by a tracked `commit-msg` hook (`.githooks/commit-msg`); activate per clone with `git config core.hooksPath .githooks`. Bypass: `git commit --no-verify`.
- **Bundle related changes into a single commit.** One logical change = one commit.

### Versioning

- **Canonical source:** git tag (`git tag -a vX.Y.Z`). No version file in the tree.
- **Sync target:** `CHANGELOG.md` (every shipped heading mirrors a tag).
- **Policy:** [SemVer 2.0.0](https://semver.org/spec/v2.0.0.html). Decision table + release-cut recipe: `references/versioning.md` in the `project-standardization` skill. During 0.x, `0.X+1.0` MAY break; `0.X.Y+1` is backwards-compatible only.
- **Trigger:** plan execution appends to `[Unreleased]` in `CHANGELOG.md`. At plan close-out the documenter ship-bumps when the branch carries feat/fix commits and this section declares a canonical source. Tagging stays deliberate, user-invoked.
- **Last release:** _none yet_.

## Artifacts

Primary artifact home is the skills repo: `docs/artifacts/features/stdctl/` (spec + history).

Local `docs/artifacts/{features,reviews,choices}/` are pre-created deliberately (user override of "never pre-create empty"). Reviews log flat under `docs/artifacts/reviews/`. Filenames: `YYYY-MM-DD-<topic>-<type>.md` per template convention.

## Build environment

None. `bin/stdctl` is a plain bash script. Install: `ln -sf ~/projects/Tools/stdctl/bin/stdctl ~/.local/bin/stdctl` (single-file, copy works too).