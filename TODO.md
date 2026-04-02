# TODO

- TODO: `doc/Makefile` still builds an `intrinsic` manual, but `doc/intrinsic/` is missing. Decide whether to restore the directory or remove the stale target.
- TODO: `spec/nvidia_block_scaling.adoc`, `spec/report_nvidia_block_scaling.adoc`, and `spec/report_amd_block_scaling.adoc` are reference and analysis documents, not spec chapters. Decide whether they belong under `doc/`, a future `research/` directory, or another references area instead of the `spec/` root.
- TODO: `xiangshan-ai-programming-guide.adoc` is a standalone supplementary document and is not connected to any current build target under `doc/` or `spec/`. Decide whether to move it under `doc/` and give it a build path, or keep it explicitly as an unbuilt draft/reference.
- TODO: `riscv-isa-manual/` exists as an untracked local reference copy of the upstream ISA manual and is not part of the current build assembly. Decide whether it should become a tracked submodule/vendor directory, remain intentionally local and be added to `.gitignore`, or be removed from the repository workspace.
