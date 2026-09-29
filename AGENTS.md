# stdctl

## What this is

Single-file bash CLI that lints any repository against the project-standardization convention (source: the `project-standardization` skill in the skills repo). One subcommand: `check`. Rules: floor (G1-G7), structure + tier (S1-S5), skills-collection (K1-K2). Spec and history: `docs` artifacts live in the skills repo under `docs/artifacts/features/stdctl/`.

Tier: small

## Stack

Bash + coreutils only. One file: `bin/stdctl`. No dependencies, no build step.

## Git & workflow

- No commit/push without explicit user instruction, except while executing an approved spec+plan (commits at task boundaries).
- Conventional Commits 1.0.0, no scope. Branch naming Conventional Branch 1.1.0.
- Install: `ln -sf ~/projects/Tools/stdctl/bin/stdctl ~/.local/bin/stdctl` (single file, copy works too).