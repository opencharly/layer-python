# python

A CPython 3.13 interpreter for OpenCharly images, provided as a pixi-managed
conda-forge environment.

The `python` candy ships a `pixi.toml` pinning `python >=3.13,<3.14` from
conda-forge. A pixi builder stage materializes the environment at
`~/.pixi/envs/default`, so the interpreter lands at a fixed, PATH-contributed
path.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `python` |
| Interpreter | `~/.pixi/envs/default/bin/python` |
| Version | CPython `>=3.13,<3.14` (conda-forge) |
| Requires | [`layer-pixi`](https://github.com/opencharly/layer-pixi) |
| Install files | `charly.yml`, `pixi.toml`, `pixi.lock` |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-python-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-python:v2026.243.0409'
```

Then, inside the built image (or on a dev host):

```bash
~/.pixi/envs/default/bin/python --version    # Python 3.13.x
~/.pixi/envs/default/bin/python -c "import sys, json, sqlite3; print(sys.version_info.major, sys.version_info.minor)"
```

The candy's `plan:` asserts the interpreter exists at the fixed path, reports a
CPython 3.13 version, and executes a standard-library snippet including the
`sqlite3` C extension.

## Package management

pixi is the only Python package manager here — `pip install` and `conda install`
are not the supported path. Add dependencies to `pixi.toml` and regenerate
`pixi.lock`.

## Layout

- `charly.yml` — the `python:` candy entity (the `require:` on `layer-pixi` and
  the `check:` assertions) and the embedded `python-skill:` skill entity.
- `pixi.toml` / `pixi.lock` — the conda-forge environment pinning CPython 3.13.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-languages:python`
- Dependency: `/charly-languages:pixi`
- ML variant: `/charly-languages:python-ml`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
