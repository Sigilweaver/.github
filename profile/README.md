# Sigilweaver

A *sigil* is a symbol that carries meaning — an instruction, a pattern, a compressed idea. A *weaver* works with threads, turning raw material into structure. Put them together and you get a reasonably pretentious name for a programmer: someone who shifts symbols around to weave threads of logic into working software.

This organization is a personal open-source umbrella. The projects here are things I built because I wanted them to exist — tools I find useful, formats I wanted to understand, infrastructure I wanted to own.

---

## Projects

| Project | Description |
|---------|-------------|
| [Loom](https://github.com/sigilweaver/loom) | Visual data pipeline tool built on Polars. Self-hosted, no SaaS required. Approaching its first public release. (June?) |

### File formats

Reverse-engineering and documenting proprietary binary formats. Each format gets an open spec/parser (`Open*`) and a public file corpus (`*-Sources`).

#### QVD — Qlik

| Repository | Description |
|------------|-------------|
| [OpenQVD](https://github.com/Sigilweaver/OpenQVD) | Clean-room Rust reader/writer for Qlik QVD files. Full spec, Python bindings (PyArrow, Polars, Pandas), CLI. |
| [QVD-Sources](https://github.com/Sigilweaver/QVD-Sources) | Curated index of ~1,145 public `.qvd` files across 716 GitHub repos. |

#### YXDB — Alteryx

| Repository | Description |
|------------|-------------|
| [OpenYXDB](https://github.com/Sigilweaver/OpenYXDB) | Maintained fork of Alteryx's open-source C++ YXDB library. Cross-platform, Python bindings via nanobind, PyArrow/Pandas/Polars integration. |
| [YXDB-Sources](https://github.com/Sigilweaver/YXDB-Sources) | Curated index of 1,824 public `.yxdb` files (1,644 E1, 180 E2) across 156 GitHub repos. |

Landing pages for each project live at [sigilweaver.app](https://sigilweaver.app/). More projects are in active development.

---

## How Things Are Built Here

Strong opinions on tooling and project structure:

- **Isolation is non-negotiable.** Every project gets its own hermetic environment — [`pixi`](https://pixi.sh), [`uv`](https://docs.astral.sh/uv/), [`rustup`](https://rustup.rs), [`bun`](https://bun.sh). Fast to set up, zero ambient state.
- **Portability is a first-class requirement.** Linux, macOS, Windows, x64, ARM. If it only runs on one platform it isn't done.
- **Performance is a design constraint, not an afterthought.** Rust for the critical path, Python for ergonomics, the right data structures throughout.
- **Own the infrastructure.** Self-hosted, open-source, no required vendor accounts, no mandatory SaaS.

Open source because every tool in this stack is open source. That's the deal.
