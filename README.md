# RISC-V Zames (Attached Matrix Extension Simplified) Specification Proposal

This repository contains the draft specification for **Zames**: the RISC-V Attached Matrix Extension Simplified proposal.

The current draft is based on XuanTie Matrix Extension V0.6.0 and narrows the scope to a small matrix-multiply-oriented ISA subset for AI and ML workloads.

## Scope

The current draft focuses on:

- A single architectural matrix multiplication instruction: `mmacc`
- Tile configuration via `msettile{m|k|n}` and type configuration via `msettype`
- Four tile registers (`tr0`-`tr3`), four accumulation registers (`acc0`-`acc3`), and eight associated `mtype` registers
- Matrix load/store instructions organized by matrix class (`A` / `B` / `C`), direction (load / store), transpose mode, and whole-register mode
- The `mzero1r` utility instruction
- An optional `Zmasync` appendix / extension, documented separately from the Zames main-body instruction set

The draft intentionally omits the broader element-wise, conversion, movement, and packing features from the original XuanTie proposal.

## Repository Layout

```text
spec/               Main Zames specification sources (AsciiDoc)
doc/                Supplementary documents and build resources
doc/docs-resources/ RISC-V docs-resources submodule used for builds
riscv-isa-manual/   Local reference copy of the upstream RISC-V ISA manual
```

The main document entry point is `spec/header.adoc`.

Files under `spec/proposal*.adoc` are retained as historical design notes only. They are not included in the main Zames specification build and are not part of the CI document probe/build path.

## Quick Start

```bash
git clone https://github.com/XUANTIE-RV/riscv-matrix-extension-spec.git
cd riscv-matrix-extension-spec
git submodule update --init
```

## Building The Specification

The recommended path is to build from `spec/`.

**Automatic mode:**

```bash
cd spec
make
```

This uses Docker when available and falls back to a local AsciiDoctor toolchain otherwise.

**Force Docker build:**

```bash
cd spec
make build-container
```

**Local build without Docker:**

```bash
gem install asciidoctor-pdf asciidoctor-bibtex asciidoctor-diagram asciidoctor-mathematical

cd spec
make build-no-container
```

**Build HTML:**

```bash
cd spec
make build-html
```

Primary outputs:

- `spec/riscv_zames_specification_proposal.pdf`
- `spec/riscv_zames_specification_proposal.html`

## Supplementary Documents

The `doc/` directory contains supplementary material such as:

- ABI documentation under `doc/abi/`
- SHL documentation under `doc/shl/`
- Slides and supporting references under `doc/slides/`

## CI

GitHub Actions builds the specification artifacts for changes under `spec/` and can also publish the generated HTML/PDF outputs.

## References

For more information on the RISC-V documentation toolchain and authoring conventions:

- [RISC-V Documentation Developer Guide](https://github.com/riscv/docs-dev-guide)
- [RISC-V Docs Base Container Image](https://github.com/riscv/riscv-docs-base-container-image)
