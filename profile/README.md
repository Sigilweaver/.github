# Sigilweaver

Building resilient infrastructure for data and science.

Sigilweaver primarily develops open-source scientific data tooling and standalone data pipelines. Our overarching goal is to ensure data remains portable, reproducible, and accessible across different ecosystems.

---

## Infrastructure & Tooling

| Project | Description |
|---------|-------------|
| [Loom](https://github.com/sigilweaver/loom) | Visual data pipeline tool built on Polars. Self-hosted, no SaaS required. Approaching its first public release. |

## Scientific Data & Bioinformatics

Open, high-performance tooling for specialized scientific workflows across genomics, mass spectrometry, and medical imaging.

### Mass Spectrometry

| Repository | Description |
|------------|-------------|
| [OpenMassSpec](https://github.com/Sigilweaver/OpenMassSpec) | Umbrella Rust and Python stack for mass spectrometry raw-file access. Single API over all vendor parsers, mzML 1.1.0 conversion, and Arrow output - no vendor SDKs or Windows-only DLLs. |
| [OpenTFRaw](https://github.com/Sigilweaver/OpenTFRaw) | Independent spec and reader for Thermo Fisher's RAW format, used by Xcalibur on Orbitrap and LTQ instruments. Full format documentation, mass spectra and chromatogram extraction. |
| [OpenTimsTDF](https://github.com/Sigilweaver/OpenTimsTDF) | Independent spec and reader for Bruker's TDF format - an SQLite database paired with a binary blob (`analysis.tdf_bin`), used by timsTOF instruments for time-of-flight acquisitions. |
| [OpenWRaw](https://github.com/Sigilweaver/OpenWRaw) | Independent spec and reader for Waters' RAW format. Handles the MassLynx directory structure to extract mass spectra, chromatograms, and metadata. |
| [OpenARaw](https://github.com/Sigilweaver/OpenARaw) | Independent spec and reader for Agilent's MassHunter `.d` format, clean-room reverse-engineered. Covers Q-TOF (profile and centroid) and QQQ (MRM) acquisitions. |
| [OpenSXRaw](https://github.com/Sigilweaver/OpenSXRaw) | Independent spec and reader for SCIEX's legacy `.wiff`/`.wiff.scan` format, clean-room reverse-engineered. Covers the TripleTOF and QTRAP instrument families. |
| [SpecLance](https://github.com/Sigilweaver/SpecLance) | Columnar, memory-mapped mass spectrometry store built on Lance. Ingests from vendor formats (via OpenMassSpec) or mzML - faster reads, cheaper seeks, multi-run directories, and direct export back to mzML. |

### Genomics

| Project | Description |
|---------|-------------|
| [Phreddy](https://github.com/Sigilweaver/Phreddy) | From-scratch, columnar, memory-mappable genomics pipeline in Rust. Transcodes paired-end FASTQ into a custom columnar format then runs every downstream stage (map, sort, call) as a bounded-memory streaming computation -- single binary, single machine, no cluster. Research preview. |
| [GenoLance](https://github.com/Sigilweaver/GenoLance) | Fast, columnar multi-sample variant store built on LanceDB. Ingests VCF/BCF files (and annotation sources like ClinVar) into per-field Arrow columns - query by region or gene, annotate against ClinVar, and compare across samples with no Spark, no cloud, no GenomicsDB. |

### Medical Imaging

| Repository | Description |
|------------|-------------|
| [OpenKSpace](https://github.com/Sigilweaver/OpenKSpace) | Rust library and CLI for Cartesian MRI k-space reconstruction from ISMRMRD `.h5` files. Noise pre-whitening, navigator phase correction, 2D/3D IFFT with root-sum-of-squares coil combination, PNG output. |
| [DICOM-Atlas](https://github.com/Sigilweaver/DICOM-Atlas) | Open registry of public and private DICOM tags compiled from vendor conformance statements plus PS3.6. / [pydicom](https://github.com/pydicom/pydicom) / [GDCM](https://gdcm.sourceforge.net) |

## Open Specifications & Interoperability

Developing reference implementations and public test corpora for binary formats. This work focuses on enabling cross-platform interoperability, toolchain integration, and long-term data preservation.

### QVD - Qlik

| Repository | Description |
|------------|-------------|
| [OpenQVD](https://github.com/Sigilweaver/OpenQVD) | Native Rust reader/writer for Qlik QVD files. Full spec, Python bindings (PyArrow, Polars, Pandas), CLI. |
| [QVD-Sources](https://github.com/Sigilweaver/QVD-Sources) | Curated index of `.qvd` files sourced from public GitHub repos. |

### YXDB - Alteryx

| Repository | Description |
|------------|-------------|
| [SigilYX](https://github.com/Sigilweaver/SigilYX) | Rust reader/writer for Alteryx YXDB files. Full spec, Python bindings (PyArrow, Polars, Pandas), lazy-compatible. |
| [OpenYXDB](https://github.com/Sigilweaver/OpenYXDB) | Maintained fork of Alteryx's open-source C++ YXDB library. Cross-platform, Python bindings via nanobind, PyArrow/Pandas/Polars integration. |
| [YXDB-Sources](https://github.com/Sigilweaver/YXDB-Sources) | Curated index of `.yxdb` files sourced from public GitHub repos. |

### Hyper - Tableau
| Repository | Description |
|------------|-------------|
| [Hyper-Sources](https://github.com/Sigilweaver/Hyper-Sources) | Curated index of `.hyper` files sourced from public GitHub repos. |

### Other
| Repository | Description |
|------------|-------------|
| [OpenQBW](https://github.com/Sigilweaver/OpenQBW) | Specification and parser for QuickBooks `.qbw` files. |
| [OpenSQLAnywhere](https://github.com/Sigilweaver/OpenSQLAnywhere) | Specification and parser for the SAP SQL Anywhere page-store format |

