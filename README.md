# Sigilweaver/.github

Organization defaults for Sigilweaver repositories: issue and PR templates,
`SUPPORT.md`, `CODE_OF_CONDUCT.md`, the organization profile
(`profile/README.md`), and the reusable GitHub Actions workflows described
below.

## Reusable workflows

Shared CI and release workflows for the Rust + PyO3 reader repos
(OpenTFRaw, OpenWRaw, OpenTimsTDF, OpenARaw, OpenSXRaw, OpenSZRaw,
OpenMassSpecCore, OpenMassSpec, SpecLance). They follow
[`ops/CI_STANDARDS.md`](https://github.com/Sigilweaver/ops/blob/main/CI_STANDARDS.md):
fmt and clippy once on Linux, tests on every OS a wheel ships for, PyPI
through trusted publishing only.

| Workflow | Purpose |
| --- | --- |
| [`rust-ci.yml`](.github/workflows/rust-ci.yml) | fmt + clippy, cargo test per OS, MSRV check, cargo doc, optional test corpus |
| [`maturin-ci.yml`](.github/workflows/maturin-ci.yml) | abi3 wheel build + pytest per OS and Python version |
| [`audit.yml`](.github/workflows/audit.yml) | cargo audit with an ignore list |
| [`release.yml`](.github/workflows/release.yml) | tag check, CI, wheel + sdist build, crates.io publish |
| [`release-finalize.yml`](.github/workflows/release-finalize.yml) | GitHub Release from CHANGELOG, docs branch update |

Callers reference them at `@main`:

```yaml
uses: Sigilweaver/.github/.github/workflows/rust-ci.yml@main
```

`release.yml` calls `rust-ci.yml` at `@main` as well.

A complete set of caller workflows for OpenSZRaw is in
[`examples/openszraw/.github/workflows/`](examples/openszraw/.github/workflows/).

Arguments inputs (`test-args`, `clippy-args`, `pytest-args`, and so on) are
split on whitespace.

### rust-ci.yml

Jobs:

- `lint` (ubuntu-latest): `cargo fmt --all -- --check`, then
  `cargo clippy <clippy-args> -- -D warnings`.
- `test` (each OS in `os`, bash on every OS, `RUSTFLAGS=-D warnings`):
  `cargo test <test-args>`, with the corpus steps below when enabled.
- `msrv` (ubuntu-latest): reads `rust-version` from the root `Cargo.toml`
  (`[workspace.package]` first, then `[package]`), installs that toolchain,
  runs `cargo check <msrv-args>`. Fails if no `rust-version` is set.
- `doc` (ubuntu-latest): `cargo doc <doc-args>` with
  `RUSTDOCFLAGS=-D warnings`.

| Input | Type | Default | Description |
| --- | --- | --- | --- |
| `os` | string (JSON array) | `["ubuntu-latest", "macos-latest", "windows-latest"]` | Runners for the `test` job. |
| `test-args` | string | `--workspace --all-targets` | Arguments to `cargo test`. |
| `clippy-args` | string | `--workspace --all-targets` | Arguments to `cargo clippy` before `--`. |
| `msrv` | boolean | `true` | Run the `msrv` job. |
| `msrv-args` | string | `--workspace` | Arguments to `cargo check` on the MSRV toolchain. |
| `doc` | boolean | `true` | Run the `doc` job. |
| `doc-args` | string | `--workspace --no-deps` | Arguments to `cargo doc`. |
| `corpus-fetch-command` | string | empty | Bash script that downloads the corpus. Empty disables all corpus steps. |
| `corpus-sha256` | string | empty | `sha256sum` manifest, one `<sha256>  <file>` per line, paths relative to the corpus directory. Required with `corpus-fetch-command`. |
| `corpus-path` | string | `corpus` | Corpus directory, relative to the repository root. |
| `corpus-cache-key` | string | empty | Extra text mixed into the cache key. |
| `corpus-env-var` | string | empty | Environment variable set to the absolute corpus directory for the test steps. |
| `corpus-linux-only` | boolean | `false` | Fetch the corpus on Linux runners only; other OSes run the tests without it. |
| `corpus-test-args` | string | empty | See below. |

An empty string for `os` or for any `*-args` input means the default shown.

Corpus handling, when `corpus-fetch-command` is set:

1. `actions/cache` restores `corpus-path`. The key is
   `corpus-<runner OS>-<hash of corpus-cache-key and corpus-sha256>`, so
   editing the manifest starts a fresh cache.
2. On a cache miss, `corpus-fetch-command` runs under
   `bash -euo pipefail` with `corpus-path` as its working directory.
3. Every file in `corpus-sha256` is checked with `sha256sum -c` (or
   `shasum -a 256 -c` on macOS), on cache hits too. A missing file or a
   mismatch fails the job.
4. `CORPUS_DIR` always holds the absolute corpus directory; if
   `corpus-env-var` is set, that variable gets the same value.
5. If `corpus-test-args` is empty, `cargo test <test-args>` runs with
   `REQUIRE_CORPUS=1`. If it is set, `cargo test <test-args>` runs without
   `REQUIRE_CORPUS`, then `cargo test <corpus-test-args>` runs with
   `REQUIRE_CORPUS=1`. Use this when only some corpus tests have fixtures
   in the CI corpus.

### maturin-ci.yml

One job per OS x Python version: installs maturin, runs
`maturin build --release --out dist [--features <maturin-features>]
[--manifest-path <manifest-path>] <maturin-args>`, fails unless every wheel
in `dist/` is tagged `abi3`, installs the wheel plus
`pytest-requirements`, applies `test-env`, and runs
`python -m pytest <pytest-args>`. The corpus inputs work as in
`rust-ci.yml`; when the corpus is fetched, pytest runs with
`REQUIRE_CORPUS=1`.

| Input | Type | Default | Description |
| --- | --- | --- | --- |
| `os` | string (JSON array) | `["ubuntu-latest", "macos-latest", "windows-latest"]` | Runners. |
| `python-versions` | string (JSON array) | `["3.9", "3.13"]` | Python versions the wheel is built with and tested on. |
| `manifest-path` | string | empty | Cargo.toml of the PyO3 crate. Empty uses `[tool.maturin] manifest-path` from `pyproject.toml`. |
| `maturin-version` | string | `>=1.5,<2` | pip version specifier for maturin. |
| `maturin-features` | string | `pyo3/abi3-py38` | Comma-separated Cargo features passed with `--features`. |
| `maturin-args` | string | empty | Extra `maturin build` arguments. |
| `pytest-requirements` | string | `pytest` | Space-separated pip requirements installed with the wheel. |
| `pytest-args` | string | `tests` | Arguments to `python -m pytest`. |
| `test-env` | string | empty | `KEY=VALUE` lines added to the pytest environment. `{corpus}` in a value becomes the absolute corpus directory. |
| `corpus-fetch-command`, `corpus-sha256`, `corpus-path`, `corpus-cache-key`, `corpus-env-var`, `corpus-linux-only` | | | Same as `rust-ci.yml`. |

### audit.yml

Runs `cargo audit` on ubuntu-latest. The caller owns the triggers.

| Input | Type | Default | Description |
| --- | --- | --- | --- |
| `ignore` | string | empty | Advisory IDs to ignore, separated by whitespace or newlines. Text after `#` on a line is a comment, so each ID can carry its reason. |
| `args` | string | empty | Extra `cargo audit` arguments. |

### release.yml and release-finalize.yml

A release is one caller workflow on `push: tags: ["v*"]` with three jobs:

1. `release` calls `release.yml`:
   - `verify` fails unless the run is on a tag, the tag is `v<version>`
     where `<version>` is `[workspace.package] version` (or
     `[package] version`, or the version of `version-package`) from
     `Cargo.toml`, and the changelog has a non-empty `## [<version>]`
     section.
   - `ci` runs `rust-ci.yml` with the `ci-*` inputs.
   - `build-wheels` builds one abi3 wheel per `wheel-targets` entry with
     `PyO3/maturin-action` (`manylinux: auto`) and fails on a non-abi3
     wheel. `build-sdist` builds the sdist. Both upload artifacts named
     `release-dist-*`.
   - `publish-crates` runs after `ci`, `build-wheels` and `build-sdist`
     succeed. For each package in `crates`, in order, it skips the
     version if crates.io already has it and otherwise runs
     `cargo publish -p <name>`. It authenticates with crates.io trusted
     publishing (`rust-lang/crates-io-auth-action`). If that exchange
     fails and the `CARGO_REGISTRY_TOKEN` secret is passed, it uses the
     token and logs a warning; with no token, the failed exchange fails
     the job. With `crates-io-trusted-publishing: false` it uses only the
     token.
2. `pypi`, a job in the caller itself, downloads the
   `dist-artifact-pattern` artifacts and runs
   `pypa/gh-action-pypi-publish`. PyPI trusted publishing does not work
   from inside a reusable workflow, so this step cannot live in
   `release.yml`.
3. `finalize` calls `release-finalize.yml` after `pypi`:
   - `github-release` creates the GitHub Release for the tag (title = tag,
     body = the changelog section for the version, pre-release when the
     version contains `-`). On a re-run it updates the notes of the
     existing release.
   - `docs` runs
     `git push -f origin refs/tags/<tag>^{commit}:refs/heads/<docs-branch>`,
     moving the docs branch to the tagged commit for the Cloudflare docs
     deploy.

No publish step uses `continue-on-error`. The only step that does is the
crates.io token exchange, and only when a `CARGO_REGISTRY_TOKEN` fallback
is present.

`release.yml` inputs:

| Input | Type | Default | Description |
| --- | --- | --- | --- |
| `crates` | string | empty | Packages to publish to crates.io, in order, separated by whitespace or newlines. Empty skips crates.io. |
| `version-package` | string | empty | Package whose version the tag must match. Empty uses `[workspace.package] version`, then `[package] version`. |
| `changelog` | string | `CHANGELOG.md` | Changelog that must have a section for the version. |
| `crates-io-trusted-publishing` | boolean | `true` | Try crates.io trusted publishing before `CARGO_REGISTRY_TOKEN`. |
| `crates-io-environment` | string | `crates-io` | GitHub environment of the `publish-crates` job. Must not be empty. |
| `python` | boolean | `true` | Build wheels and an sdist. |
| `python-manifest-path` | string | empty | Cargo.toml of the PyO3 crate. Empty uses `pyproject.toml`. |
| `maturin-features` | string | `pyo3/abi3-py38` | Cargo features for the wheel builds. |
| `maturin-args` | string | empty | Extra `maturin build` arguments for the wheels. |
| `wheel-targets` | string (JSON array) | Linux x86_64 + aarch64, macOS x86_64 + aarch64, Windows x86_64 | Wheel matrix, e.g. `[{"os": "ubuntu-latest", "target": "x86_64"}]`. |
| `ci-os`, `ci-test-args`, `ci-clippy-args`, `ci-msrv`, `ci-msrv-args`, `ci-doc`, `ci-doc-args`, `ci-corpus-fetch-command`, `ci-corpus-sha256`, `ci-corpus-path`, `ci-corpus-cache-key`, `ci-corpus-env-var`, `ci-corpus-linux-only`, `ci-corpus-test-args` | | | Passed to `rust-ci.yml` as the input of the same name without `ci-`. Use the same values as the repo's CI caller. |

| Secret | Required | Description |
| --- | --- | --- |
| `CARGO_REGISTRY_TOKEN` | no | crates.io token used when trusted publishing is disabled or its exchange fails. |

| Output | Description |
| --- | --- |
| `version` | Released version, the tag without `v`. |
| `dist-artifact-pattern` | `release-dist-*`, for `actions/download-artifact` `pattern`. |

`release-finalize.yml` inputs:

| Input | Type | Default | Description |
| --- | --- | --- | --- |
| `changelog` | string | `CHANGELOG.md` | Changelog the release notes come from. |
| `docs-branch` | string | `docs-live` | Branch moved to the tag commit. Empty skips the `docs` job. |

Release caller (permissions are per job because a reusable workflow gets
at most what its calling job grants):

```yaml
name: Publish
on:
  push:
    tags: ["v*"]
concurrency:
  group: publish-${{ github.ref }}
  cancel-in-progress: false
permissions:
  contents: read
jobs:
  release:
    uses: Sigilweaver/.github/.github/workflows/release.yml@main
    permissions:
      contents: read
      id-token: write
    secrets:
      CARGO_REGISTRY_TOKEN: ${{ secrets.CARGO_REGISTRY_TOKEN }}
    with:
      crates: my-crate
      python-manifest-path: crates/my-crate-py/Cargo.toml
  pypi:
    needs: release
    runs-on: ubuntu-latest
    environment:
      name: pypi
      url: https://pypi.org/p/my-crate
    permissions:
      id-token: write
    steps:
      - uses: actions/download-artifact@v8
        with:
          pattern: ${{ needs.release.outputs.dist-artifact-pattern }}
          path: dist
          merge-multiple: true
      - uses: pypa/gh-action-pypi-publish@release/v1
        with:
          packages-dir: dist/
          skip-existing: true
  finalize:
    needs: [release, pypi]
    uses: Sigilweaver/.github/.github/workflows/release-finalize.yml@main
    permissions:
      contents: write
```

Repos without Python bindings set `python: false`, drop the `pypi` job,
and use `needs: release` on `finalize`.

Registry setup:

- PyPI and crates.io both match trusted publishers against the caller's
  workflow file (for example `publish.yml`), not against `release.yml`.
  Register the caller's filename, and the environment (`pypi` for PyPI,
  `crates-io-environment` for crates.io) if the publisher names one.
- crates.io needs one trusted publisher per published crate.
- The `docs` job force-pushes with `GITHUB_TOKEN`; a branch protection rule
  on the docs branch that blocks force pushes from GitHub Actions makes it
  fail.
