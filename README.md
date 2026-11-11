# RISC-V Zames (Attached Matrix Extension Simplified) Specification Proposal

This repository contains the draft specification for **Zames**: the RISC-V Attached Matrix Extension Simplified proposal.

The current draft is based on XuanTie Matrix Extension V0.6.0 and narrows the scope to a small matrix-multiply-oriented ISA subset for AI and ML workloads.

## Scope

The current draft focuses on:

- A single architectural matrix multiplication instruction: `mmacc`
- Tile configuration via `msettile{m|k|n}` and matrix-configuration programming via `msetcfg`
- Four tile registers (`tr0`-`tr3`), four accumulation registers (`acc0`-`acc3`), and eight associated `mcfg` registers
- Matrix load/store instructions organized by matrix class (`A` / `B` / `C`), direction (load / store), transpose mode, and whole-register mode
- The `mzero` utility instruction
- An optional `Zmasync` appendix / extension, documented separately from the Zames main-body instruction set
- An optional `Xxsaievu` external vector appendix, with independent vector registers, configuration, computation, and memory synchronization

The Zames main body intentionally omits the broader element-wise, conversion, movement, and packing features from the original XuanTie proposal.

## Repository Layout

```text
spec/               Main Zames specification sources (AsciiDoc)
doc/                Supplementary documents and build resources
doc/docs-resources/ RISC-V docs-resources submodule used for builds
riscv-isa-manual/   Local reference copy of the upstream RISC-V ISA manual
```

The main document entry point is `spec/header.adoc`.

## Reading the EVU Appendix

Start with `spec/xevu_appendix.adoc`. Like the Zmasync appendix, it follows
five sections: Overview, Programmer's Model, Instruction Format,
Instructions, and Instruction Encoding List.

| Section | Source | Content |
| --- | --- | --- |
| B.1 Overview | `spec/xevu_appendix.adoc` | Scope, notation, draft status, and completion model |
| B.2 Programmer's Model | `spec/xevu_program_model.adoc`, including `spec/xevu_memory_sync.adoc` | Vector registers, configuration ABI 2, ten CSRs, execution, memory and exception rules, and context management |
| B.3 Instruction Format | `spec/xevu_encoding.adoc` | Custom-0 formats, operand fields, and encoding legality |
| B.4 Instructions | `spec/xevu_instructions.adoc` | Arithmetic, RVV-mapped math/width conversions, packed INT8/FP8/FP4 conversions, completion, memory handoff, and a softmax-like example |
| B.5 Instruction Encoding List | `spec/xevu_instruction_list.adoc` | All 78 assigned operation selectors and reserved selectors |

The appendix is a concise ISA reference: common rules are stated once,
operation selectors are grouped by family, and backend algorithms and
implementation-review process details are omitted. Numerical limitations
and synchronization/error constraints remain explicit.

`Xxsaievu` is a project-local draft name, not a ratified extension. The appendix
retains the current EVU custom-0 encoding and independent state. Revision 0.3
defines scalar-GPR offsets for slideup/slidedown and removes the public WAIT
instruction (0x19 remains illegal); completion synchronization uses FENCE.
It retains the 15 RVV-mapped math/width encodings and six low-precision conversion
encodings present in the implementation sources, advertised by EVCAP bits
28/29 without changing configuration/context ABI 2. Packed conversions support
VRF-based quantization sequences; descriptor-based quantization and direct
matrix-data paths remain out of scope. Zames `mmacc`, TR/ACC, and the existing
Zmasync definitions are unchanged. Unresolved implementation/numerical
details are explicitly identified; documenting the
new source interface does not assert completed RTL simulation acceptance.

The normal PDF/HTML builds include this appendix in the same document as Zames
and Zmasync. No separate EVU build target is needed.

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
