# stdctl

A single-file bash linter for repositories that follow the [project-standardization](https://github.com/RubenVanDerVeen/skills) convention. Run `stdctl check` inside any git repo and it tells you whether the repo still matches the convention: file layout, agent context files, changelog, hooks, commit history, and (if present) skill collections.

The convention itself, including the authoritative rule spec, lives in the [skills repo](https://github.com/RubenVanDerVeen/skills) under `docs/artifacts/features/stdctl/`.

## Install

Requirements: bash and coreutils. Nothing else, no build step.

```sh
# from a local clone
ln -sf ~/projects/Tools/stdctl/bin/stdctl ~/.local/bin/stdctl

# or straight from the repo
curl -fsSL https://raw.githubusercontent.com/RubenVanDerVeen/stdctl/main/bin/stdctl \
  -o ~/.local/bin/stdctl && chmod +x ~/.local/bin/stdctl
```

## Usage

```sh
cd your-repo
stdctl check            # full report
stdctl check --quiet    # failures only
```

Exit codes:

| Code | Meaning |
|------|---------|
| 0 | compliant, no FAILs |
| 1 | one or more FAILs, or the repo is not standardized |
| 2 | usage error, or not a git repo |

## What it checks

**Floor rules (G1-G7)** apply to every repo:

| Rule | Check |
|------|-------|
| G1 | no em-dashes in tracked markdown |
| G2 | no `temp/`, `old/`, `archive/`, `.planning/`, or `docs/superpowers/` paths |
| G3 | tracked paths are kebab-case (conventional files like `README.md` exempt) |
| G4 | artifact filenames start with an ISO 8601 date (`YYYY-MM-DD-...`) |
| G5 | branch names follow Conventional Branch `<type>/<description>` |
| G6 | shared hooks are activated (`core.hooksPath .githooks` + `.gitattributes` eol rule) |
| G7 | commit subjects follow Conventional Commits (legacy history warns, not fails) |

**Structure rules (S1-S5)** check the agent scaffolding against the declared tier (small, medium, or large):

| Rule | Check |
|------|-------|
| S1 | `AGENTS.md` declares a tier and has a Git section |
| S2 | `CLAUDE.md` shim exists and imports `AGENTS.md` |
| S3 | `.agents/` topic split matches the tier |
| S4 | `CHANGELOG.md` follows Keep a Changelog (required by tier) |
| S5 | `docs/artifacts/` has the `features/` + `reviews/` layout |

**Skills-collection rules (K1-K2)** fire only when the repo contains skills:

| Rule | Check |
|------|-------|
| K1 | `SKILL.md` frontmatter is valid: kebab-case name, `Use when...` description, 1 KB cap |
| K2 | skill body has an `## Overview` heading and no `## Skill` heading |

Some checks warn instead of fail where the convention allows drift (legacy commit history, missing `docs/artifacts/choices/` before any decision record exists). Adding rules is a coordinated change: rule IDs are monotonic (G8, S6, K3, ...) and each new ID must land in both `bin/stdctl` and the spec in the skills repo.

## Status

Pre-release. No versions tagged yet; see [CHANGELOG.md](CHANGELOG.md) for ongoing changes.

## AI assistance

- **AI involvement:** AI-driven
  <!-- human-written | AI-assisted | AI-driven | fully vibecoded -->
- **Method:** AI work is organized and professionally executed via a personal
  skill system: brainstorm > spec > plan > subagent execution > review.
  See the [skills repo](https://github.com/RubenVanDerVeen/skills) and
  [how the workflow is organized](https://github.com/RubenVanDerVeen/skills/blob/main/docs/workflows/workflow.md).
