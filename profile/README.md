# Sigilweaver

A *sigil* is a symbol that carries meaning — an instruction, a pattern, a compressed idea. A *weaver* works with threads, turning raw material into structure. Put them together and you get a reasonably pretentious name for a programmer: someone who shifts symbols around to weave threads of logic into working software.

This organization is a personal open-source umbrella. The projects here are things I built because I wanted them to exist — tools I find useful, formats I wanted to understand, infrastructure I wanted to own.

---

## Projects

| Project | Description |
|---------|-------------|
| [Loom](https://github.com/sigilweaver/loom) | Visual data pipeline tool built on Python. Self-hosted, no SaaS required. Approaching its first public release. |
| [OpenYXDB](https://github.com/sigilweaver/openyxdb) | Cross-platform C++/Python library for reading and writing Alteryx YXDB files. A maintained fork of Alteryx's official open-source implementation, with bug fixes, modern CMake, and PyArrow/Pandas/Polars integration. |
| [YXDB-Sources](https://github.com/sigilweaver/yxdb-sources) | Curated index of publicly available YXDB files from the open-source ecosystem. |

Landing pages for each project live at [sigilweaver.app](https://sigilweaver.app/). More projects are in active development.

---

## How Things Are Built Here

Strong opinions on tooling and project structure:

- **Isolation is non-negotiable.** Every project gets its own hermetic environment — [`pixi`](https://pixi.sh), [`uv`](https://docs.astral.sh/uv/), [`rustup`](https://rustup.rs), [`bun`](https://bun.sh). Fast to set up, zero ambient state.
- **Portability is a first-class requirement.** Linux, macOS, Windows, x64, ARM. If it only runs on one platform it isn't done.
- **Performance is a design constraint, not an afterthought.** Rust for the critical path, Python for ergonomics, the right data structures throughout.
- **Own the infrastructure.** Self-hosted, open-source, no required vendor accounts, no mandatory SaaS.

Open source because every tool in this stack is open source. That's the deal.
