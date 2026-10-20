# RISC-V Zames (Attached Matrix Extension Simplified) Specification Proposal

This repository contains the draft specification for **Zames**: the RISC-V Attached Matrix Extension Simplified proposal.

The current draft is based on XuanTie Matrix Extension V0.6.0 and defines a matrix-multiply and EW/Reduce ISA for AI and ML workloads, with an XSAI EW/Reduce draft extension to the simplified instruction set.

## Scope

The current draft focuses on:

- A single architectural matrix multiplication instruction: `mmacc`
- Tile configuration via `msettile{m|k|n}` and matrix-configuration programming/conversion via `msetcfg` and `msetcfg.cvt`
- Four tile registers (`tr0`-`tr3`), four accumulation registers (`acc0`-`acc3`) with INT32/FP32 internal elements, and eight associated `mcfg` registers
- Thirty-two independent local-vector registers (`lv0`-`lv31`), each with 128 raw 32-bit elements
- Accumulator element-wise arithmetic, local-vector/GPR broadcasts, and matrix-to-local-vector ADD/MAX reductions
- Matrix load/store instructions organized by matrix class (`A` / `B` / `C`), direction (load / store), transpose mode, and whole-register mode
- The `mzero` utility instruction
- An optional `Zmasync` appendix / extension, documented separately from the Zames main-body instruction set

The EW/Reduce draft defines the instructions and conversions described in the main specification. It does not add pure local-vector arithmetic, direct transfers to CPU RVV registers, or the broader movement and packing instruction families from the original XuanTie proposal. The existing `mmacc` capability mechanism and its supported combinations remain in force; defining EW/Reduce does not add new multiply combinations.

The initial compiler interface is assembly/disassembly and C inline assembly under `+zames`, with explicit physical register names and clobbers. Intrinsics, automatic allocation of matrix/local-vector registers, new C types, and a new ABI are outside this delivery.

## Repository Layout

```text
spec/               Main Zames specification sources (AsciiDoc)
doc/                Supplementary documents and build resources
doc/docs-resources/ RISC-V docs-resources submodule used for builds
riscv-isa-manual/   Local reference copy of the upstream RISC-V ISA manual
```

The main document entry point is `spec/header.adoc`.

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
