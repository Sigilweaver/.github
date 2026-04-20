# Format reverse-engineering — methodology and rules

This document explains how format work is done here, what legal framework it operates under, and what criteria a format needs to meet for me to take it on.

---

## What this work is

Proprietary binary formats are everywhere. When a vendor controls the only parser, users are locked in: they cannot migrate, cannot build tooling, cannot publish research pipelines that others can replicate without a license. This work aims to change that for specific formats by:

1. Building a public corpus of sample files from openly available sources.
2. Writing a clean-room specification based purely on observation of those files.
3. Implementing an open-source reader (and writer where it makes sense) that anyone can use without a vendor dependency.

---

## What "clean-room" means here

Every spec and parser is developed by direct observation of publicly available binary files. No vendor source code, no licensed SDKs, no disassembly of proprietary executables, no material obtained under NDA, no files generated under EULA. The spec is written in plain language; the implementation is written from scratch against that spec.

This is consistent with the approach upheld in *Sega v. Accolade*, *Sony v. Connectix*, and the broader line of US and EU fair-use / interoperability caselaw. The goal is interoperability, not reproduction.

---

## Corpus requirements

**Before work on any format begins, a publicly available corpus of sample files must already exist.** This means files that are:

- Freely downloadable without authentication, payment, or agreement to restrictive terms.
- Indexed by public search engines or published in academic/research data repositories (GitHub, PRIDE Archive, Zenodo, figshare, etc.).

**What I will not accept:**

- Files sent directly by users who obtained them under a EULA, click-through license, or subscription agreement — even if the user believes redistribution is permitted. I cannot verify the chain of title.
- Files extracted from vendor installations or SDKs.
- Files covered by an NDA or confidentiality agreement of any kind.
- Anything obtained through scraping behind a login wall.

The corpus-building pipelines (`*-Sources` repos) document exactly where each file came from and how it was found.

---

## Criteria

A format should serve at least one of these goals before it's worth working on:

- **Scientific community** — research data trapped in a proprietary format that prevents reproducible pipelines. Mass spectrometry, genomics, imaging, and similar fields are full of these.
- **Open-source ecosystem** — a format in wide enough use that the lack of a free parser forces open-source tools to depend on a proprietary runtime or CLI.
- **Vendor lock-in mitigation** — users have their own data in a format they cannot read without paying for a specific product that may no longer be available or affordable.

Formats that exist purely for DRM, anti-cheat, or access control are out of scope — those are adversarial designs, not accidental lock-in.

---

## How the work proceeds

1. **Corpus** — find and index publicly available files using the `*-Sources` pipeline. The corpus feeds both spec development and validation.
2. **Observation** — load files in a hex editor, write tooling to dump structures, identify patterns. The `re/` directory in each format repo captures this work.
3. **Specification** — write a plain-English spec that describes the binary layout precisely enough that someone could implement a parser independently. The spec is a first-class artifact, not a comment in the code.
4. **Parser** — implement a reader (and writer where it makes sense) against the spec. Rust for performance-sensitive work; Python for ergonomics and accessibility.
5. **Validation** — run the parser against the full public corpus and report pass rates. A release is not made until the pass rate is high enough to be genuinely useful.
6. **Bindings** — Python bindings with PyArrow/Polars/Pandas integration where the format is tabular.

---

## License choices

| Component | License | Rationale |
|-----------|---------|-----------|
| Parsers / specs | AGPL-3.0 | Copyleft keeps derivative parsers open. |
| Corpus pipelines | MIT | These are utility scripts, no reason to be restrictive. |
| Forks of existing code | Inherited | License inherited from upstream. |

---

## Requesting a format

Reach out at [requests@sigilweaver.app](mailto:requests@sigilweaver.app) with:

- What the format is and what software produces it.
- Why it matters (which of the three criteria above does it meet?).
- Where the public corpus lives (URLs to data repositories, GitHub search results, etc.).

I can't commit to a timeline or guarantee I'll take every request, but I read everything.

A note on tone: I don't care where you are technically. You don't need to know what a bit field is or have read the caselaw to reach out — you just need to know that your data is stuck somewhere and you'd like it not to be. I'm generally patient and happy to engage. The open-source world has enough gatekeeping already.
