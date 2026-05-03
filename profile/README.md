# Sigilweaver

Building resilient infrastructure for data and science.

Sigilweaver primarily develops open-source scientific data tooling and standalone data pipelines. Our overarching goal is to ensure data remains portable, reproducible, and accessible across different ecosystems.

---

## Infrastructure & Tooling

| Project | Description |
|---------|-------------|
| [Loom](https://github.com/sigilweaver/loom) | Visual data pipeline tool built on Polars. Self-hosted, no SaaS required. Approaching its first public release. |

## Open Specifications & Interoperability

Developing reference implementations and public test corpora for binary formats. This work focuses on enabling cross-platform interoperability, toolchain integration, and long-term data preservation.

### QVD — Qlik

| Repository | Description |
|------------|-------------|
| [OpenQVD](https://github.com/Sigilweaver/OpenQVD) | Native Rust reader/writer for Qlik QVD files. Full spec, Python bindings (PyArrow, Polars, Pandas), CLI. |
| [QVD-Sources](https://github.com/Sigilweaver/QVD-Sources) | Curated index of `.qvd` files sourced from public GitHub repos. |

### YXDB — Alteryx

| Repository | Description |
|------------|-------------|
| [OpenYXDB](https://github.com/Sigilweaver/OpenYXDB) | Maintained fork of Alteryx's open-source C++ YXDB library. Cross-platform, Python bindings via nanobind, PyArrow/Pandas/Polars integration. |
| [YXDB-Sources](https://github.com/Sigilweaver/YXDB-Sources) | Curated index of `.yxdb` files sourced from public GitHub repos. |

### Hyper — Tableau
| Repository | Description |
|------------|-------------|
| [Hyper-Sources](https://github.com/Sigilweaver/Hyper-Sources) | Curated index of `.hyper` files sourced from public GitHub repos. |


## Scientific Data & Bioinformatics

Open, high-performance tooling for specialized scientific workflows across genomics, proteomics, and medical imaging.

### Genomics

| Project | Description |
|---------|-------------|
| [BioLance](https://github.com/Sigilweaver/BioLance) | Fast, columnar multi-sample variant store built on LanceDB. Ingests VCF/BCF files (and annotation sources like ClinVar) into per-field Arrow columns — query by region or gene, annotate against ClinVar, and compare across samples with no Spark, no cloud, no GenomicsDB. |

### Proteomics

| Repository | Description |
|------------|-------------|
| [OpenTFRaw](https://github.com/Sigilweaver/OpenTFRaw) | Independent spec and reader for Thermo Fisher's proprietary RAW format, used by Xcalibur on Orbitrap and LTQ instruments. Full format documentation, mass spectra and chromatogram extraction. |
| [OpenTDF](https://github.com/Sigilweaver/OpenTDF) | Independent spec and reader for Bruker's TDF format — an SQLite database paired with a binary blob (`analysis.tdf_bin`), used by timsTOF instruments for time-of-flight acquisitions. |

### Medical Imaging

| Repository | Description |
|------------|-------------|
| [OpenKSpace](https://github.com/Sigilweaver/OpenKSpace) | Rust library and CLI for Cartesian MRI k-space reconstruction from ISMRMRD `.h5` files. Noise pre-whitening, navigator phase correction, 2D/3D IFFT with root-sum-of-squares coil combination, PNG output. |
| [DICOM-Atlas](https://github.com/Sigilweaver/DICOM-Atlas) | Open registry of public and private DICOM tags compiled from vendor conformance statements plus PS3.6. / [pydicom](https://github.com/pydicom/pydicom) / [GDCM](https://gdcm.sourceforge.net) |
