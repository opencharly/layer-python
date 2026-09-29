# AGENTS.md — layer-python

Standalone candy repo for the `python` layer — a CPython 3.13 interpreter as a
pixi-managed conda-forge environment. The candy lives in `charly.yml` at the repo
root: the `require:` on `layer-pixi`, the `check:` assertions, and the embedded
`skill:` entity projected into the marketplace corpus as `/charly-languages:python`.

Canonical files:

- `charly.yml` — the `python:` candy entity and the `python-skill:` skill entity.
- `pixi.toml` / `pixi.lock` — the conda-forge environment pinning CPython 3.13.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-languages:python` — the owning skill. The pixi-managed CPython 3.13
  environment, its fixed interpreter path, and why pixi is the only package
  manager. Load before editing or troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, package sections, service declarations).
  Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- `charly box validate` at the repo root checks the manifest parses and
  validates.
- The candy's `plan:` `check:` steps are the functional evidence: the
  interpreter at `~/.pixi/envs/default/bin/python`, a CPython 3.13 version, and a
  real standard-library execution including the `sqlite3` C extension.
- Regenerate `pixi.lock` whenever `pixi.toml` changes — the build installs with
  `pixi install --frozen` and fails loudly on a stale lock.

## Modify this repo

- Edit the `python:` candy entity AND the `python-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a version
  or path change not mirrored in the skill leaves the corpus stale.
- Add Python dependencies to `pixi.toml` (not a `run:` pip step) and regenerate
  the lock; new behaviour claims belong in the `plan:` as an observable `check:`
  step, and in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
