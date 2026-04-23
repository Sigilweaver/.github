# Sigilweaver

A *sigil* is a symbol that carries meaning — an instruction, a pattern, a compressed idea. A *weaver* works with threads, turning raw material into structure. Put them together and you get a reasonably pretentious name for a programmer: someone who shifts symbols around to weave threads of logic into working software.

This organization is a personal open-source umbrella. The projects here are things I built because I wanted them to exist — tools I find useful, formats I wanted to understand, infrastructure I wanted to own.

Some of it is for things the world needs but nobody will build commercially: open specifications for closed formats, tools for scientific workflows, infrastructure that researchers can actually run themselves.

---

## Projects

| Project | Description |
|---------|-------------|
| [Loom](https://github.com/sigilweaver/loom) | Visual data pipeline tool built on Polars. Self-hosted, no SaaS required. Approaching its first public release. (June?) |

### File formats

Reverse-engineering and documenting proprietary binary formats. Each format gets an open spec/parser (`Open*`) and a public file corpus (`*-Sources`).

> Have a proprietary binary format you'd like reverse-engineered and documented? See the [methodology and rules](https://github.com/Sigilweaver/.github/blob/main/METHODOLOGY.md) or reach out at [requests@sigilweaver.app](mailto:requests@sigilweaver.app).

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

### Bioinformatics

> These projects are coming soon.

#### Genomics

| Project | Description |
|---------|-------------|
| [BioLance](https://github.com/Sigilweaver/BioLance) | Fast, columnar multi-sample variant store built on LanceDB. Ingests VCF/BCF files (and annotation sources like ClinVar) into per-field Arrow columns — query by region or gene, annotate against ClinVar, and compare across samples with no Spark, no cloud, no GenomicsDB. |

#### Proteomics

Mass spectrometry file formats, following the same reverse-engineering approach as QVD and YXDB.

##### TFRaw — Thermo Fisher Scientific

| Repository | Description |
|------------|-------------|
| [OpenTFRaw](https://github.com/Sigilweaver/OpenTFRaw) | Clean-room spec and reader for Thermo Fisher's proprietary RAW format, used by Xcalibur on Orbitrap and LTQ instruments. Full format documentation, mass spectra and chromatogram extraction. |
| [TFRaw-Sources](https://github.com/Sigilweaver/TFRaw-Sources) | Curated index of public `.raw` files from mass spectrometry data repositories. |

##### TDF — Bruker timsTOF

| Repository | Description |
|------------|-------------|
| [OpenTDF](https://github.com/Sigilweaver/OpenTDF) | Clean-room spec and reader for Bruker's TDF format — an SQLite database paired with a binary blob (`analysis.tdf_bin`), used by timsTOF instruments for time-of-flight acquisitions. |
| [TDF-Sources](https://github.com/Sigilweaver/TDF-Sources) | Curated index of public Bruker `.d` analysis directories from mass spectrometry data repositories. |

#### Medical Imaging

| Repository | Description |
|------------|-------------|
| [OpenKSpace](https://github.com/Sigilweaver/OpenKSpace) | Rust library and CLI for Cartesian MRI k-space reconstruction from ISMRMRD `.h5` files. Noise pre-whitening, navigator phase correction, 2D/3D IFFT with root-sum-of-squares coil combination, PNG output. |

---

## How Things Are Built Here

Strong opinions on tooling and project structure:

- **Isolation is non-negotiable.** Every project gets its own hermetic environment — [`pixi`](https://pixi.sh), [`uv`](https://docs.astral.sh/uv/), [`rustup`](https://rustup.rs), [`bun`](https://bun.sh). Fast to set up, zero ambient state.
- **Portability is a first-class requirement.** Linux, macOS, Windows, x64, ARM. If it only runs on one platform it isn't done.
- **Performance is a design constraint, not an afterthought.** Rust for the critical path, Python for ergonomics, the right data structures throughout.
- **Own the infrastructure.** Self-hosted, open-source, no required vendor accounts, no mandatory SaaS.

Open source because every tool in this stack is open source. That's the deal.

