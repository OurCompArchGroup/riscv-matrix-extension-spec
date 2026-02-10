# RISC-V Matrix Extension Specification Proposal V0.6.0 (Simplified)

## Introduction
This is a simplified matrix extension proposal for AI applications under RISC-V architecture, based on XuanTie Matrix Extension V0.6.0. This version focuses on the core matrix multiplication functionality with minimal instruction set.

**Key Simplifications:**
* **Single matrix multiplication instruction**: Only int8 × int8 → int32 signed matrix multiplication
* **Minimal load/store**: A/B matrices support only 8-bit load, C matrix supports 8/16/32/64-bit load/store
* **Removed features**: Element-wise operations, type conversions, data movement, broadcast/pack/slide operations
* **Core registers**: 4 tile registers (tr0-tr3) and 4 accumulation registers (acc0-acc3)
* **Asynchronous store extension (Zmasync)**: Token-based synchronization for overlapping computation with memory operations

The extension is still under construction, and this is a preview specification project.

## Key Directories
```
|--spec/                    ## The RISC-V Matrix Extension specification
|--doc/                     ## Documentation resources
    |--docs-resources/      ## RISC-V documentation build resources (submodule)
```

## Removed Components
The following components from the original XuanTie v0.6.0 have been removed:
- Neural network library (shl)
- Model deployment toolkit (hhb)
- Emulator (qemu)
- GNU toolchain (xuantie-gnu-toolchain)
- Demo applications
    |--GEMM                 ## A GEMM evaluation demo using intrinsic
```

## Quick Start
### Prepare and clone repos.
```
## Quick Start
### Prepare and clone repos.
```
git clone https://github.com/XUANTIE-RV/riscv-matrix-extension-spec.git
cd riscv-matrix-extension-spec
git submodule update --init
```

## Matrix Extension Documents
This project is built using AsciiDoctor (Ruby). The repository has been setup to build the PDF on checkin using GitHub actions.

For more information on AsciiDoctor, specification guidelines, or building locally, see the
[RISC-V Documentation Developer Guide](https://github.com/riscv/docs-dev-guide).

RISC-V Matrix Extension Specification is kept in ./spec.
The recommended method for building the PDF files is to use the Docker Image, as described in the [RISC-V Docs Base Container Image repository](https://github.com/riscv/riscv-docs-base-container-image).

### Building the Specification

**Using Docker (Recommended):**
```bash
cd spec
make build-container
```

**Using Local AsciiDoctor:**
```bash
# Install dependencies
gem install asciidoctor-pdf asciidoctor-bibtex asciidoctor-diagram asciidoctor-mathematical

cd spec
make build-no-container
```

**Using GitHub Actions:**
Push changes to the repository, and the PDF will be automatically built and available as an artifact.

## Specification Summary

This simplified matrix extension includes:

- **Single instruction**: `mmacc.w.b` (int8 × int8 → int32 signed matrix multiplication)
- **Load instructions**: 8-bit for A/B matrices, 8/16/32/64-bit for C matrix
- **Store instructions**: 8/16/32/64-bit for C matrix only
- **Configuration**: `msettile{m|k|n}`, `minit`
- **Utility**: `mzero1r` (clear single register)
- **Zmasync extension**: Token-based asynchronous store synchronization

For detailed instruction descriptions, see the specification PDF in the `spec/` directory.
