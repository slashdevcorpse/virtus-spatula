# virtus-spatula

This repo is the Virtus fork of the upstream project. Virtus uses it as a Python wheel in the isolated scraper
virtualenv of the Open States bills ETL (B2). It replaces the copy `vendor/spatula` (`1.0.0+virtus.1`) in `virtus`.

- Upstream: https://codeberg.org/jpt/spatula (licence MIT). The GitHub repo `openstates/spatula` is an old mirror.
  Its `main` (`6b48d857`, version 0.9.1) is an ancestor of the upstream release commit and has no 1.0.0.
- Base: upstream commit `eb5e1701d338b3b0332c3b9d92395824c26db1e0` (`release 1.0.0`, 2025-10-31). The PyPI source
  archive `spatula-1.0.0.tar.gz` (SHA-256 `2cec1a736a168e8e0876714be37183160e356a33715a9ddd87837c1303b37f3f`) holds
  the same bytes as this commit for `src/spatula/*`, `LICENSE`, `README.md` and `pyproject.toml`.

## Branches

- `upstream-main` holds the upstream history: upstream `main` as it was when this fork got its history.
- `main` must be equal to upstream `main`. Only a fast-forward from upstream moves it. This repo is a GitHub fork of
  the old mirror, so `main` is the mirror's `6b48d857`. That commit is an ancestor of `upstream-main`, so a
  fast-forward of `main` to `upstream-main` fixes it with no re-create.
- `virtus` holds our patches as separate commits on top of an upstream commit.

## Patches

One line for each commit on `virtus` that is not in upstream:

1. `Spatula 1.0.0+virtus.1: allow lxml 6 and IPython 8.10 to 9`: the version `1.0.0+virtus.1`, `lxml>4.6,<7` (upstream
   `<6`; the scraper process uses lxml 6.1.3) and the `shell` extra `ipython>=8.10.0,<10.0.0` (upstream `<8`). The
   package code is unchanged.
2. `Release workflow: wheel, sdist and SHA256SUMS for v*+virtus.* tags`: `.github/workflows/virtus-release.yml`, and
   the build backend pin `hatchling==1.33.0`.
3. `VIRTUS.md`: this file.

## Release

1. Increase `<n>` in the `pyproject.toml` version `<upstream version>+virtus.<n>`.
2. Tag the `virtus` commit `v<upstream version>+virtus.<n>` and push the tag.
3. The workflow `virtus-release` builds the wheel and the sdist with `uv build` and publishes them with one
   `SHA256SUMS` as the release assets of the tag.
4. In `virtus`, run `tools/pins/bump.py virtus-spatula <tag>`.

## Sync with upstream

1. Fast-forward `main` and `upstream-main` to upstream `main`:
   `git fetch upstream && git push origin upstream/main:main upstream/main:upstream-main`.
2. Merge the upstream release commit into `virtus` (never rebase `virtus`). Solve the conflicts in the patch commits.
3. Update "Base" in this file. If the upstream version changes, set the `pyproject.toml` version to
   `<new upstream version>+virtus.1`.
4. Do a release.
