# Makefile for RISC-V Doc Template
#
# This work is licensed under the Creative Commons Attribution-ShareAlike 4.0
# International License. To view a copy of this license, visit
# http://creativecommons.org/licenses/by-sa/4.0/ or send a letter to
# Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.
#
# SPDX-License-Identifier: CC-BY-SA-4.0
#
# Description:
# 
# This Makefile is designed to automate the process of building and packaging 
# the Doc Template for RISC-V Extensions.

DATE ?= $(shell date +%Y-%m-%d)
REPO_ROOT := $(abspath ..)
DOCKER_RUN := docker run --rm -v ${REPO_ROOT}:/repo -w /repo/spec \
riscvintl/riscv-docs-base-container-image:latest

HEADER_SOURCE := header.adoc
PDF_RESULT := riscv_zames_specification_proposal.pdf
HTML_RESULT := riscv_zames_specification_proposal.html

ASCIIDOCTOR_PDF := asciidoctor-pdf
ASCIIDOCTOR_HTML := asciidoctor
OPTIONS := --trace \
           -a compress \
           -a mathematical-format=svg \
           -a revdate=${DATE} \
           -a pdf-fontsdir=docs-resources/fonts \
           -a pdf-theme=riscv-matrix.yml \
           --failure-level=ERROR
HTML_OPTIONS := --trace \
           -a mathematical-format=svg \
           -a revdate=${DATE} \
           --failure-level=ERROR
REQUIRES := --require=asciidoctor-bibtex \
            --require=asciidoctor-diagram \
            --require=asciidoctor-mathematical

.PHONY: all build clean build-container build-no-container build-html build-html-container build-html-no-container

all: build

build: 
	@echo "Checking if Docker is available..."
	@if command -v docker >/dev/null 2>&1 ; then \
		echo "Docker is available, building inside Docker container..."; \
		$(MAKE) build-container; \
	else \
		echo "Docker is not available, building without Docker..."; \
		$(MAKE) build-no-container; \
	fi

build-container:
	@echo "Starting build inside Docker container..."
	$(DOCKER_RUN) /bin/sh -c "$(ASCIIDOCTOR_PDF) $(OPTIONS) $(REQUIRES) --out-file=$(PDF_RESULT) $(HEADER_SOURCE)"
	@echo "Build completed successfully inside Docker container."

build-no-container:
	@echo "Starting build..."
	$(ASCIIDOCTOR_PDF) $(OPTIONS) $(REQUIRES) --out-file=$(PDF_RESULT) $(HEADER_SOURCE)
	@echo "Build completed successfully."

build-html:
	@echo "Checking if Docker is available..."
	@if command -v docker >/dev/null 2>&1 ; then \
		$(MAKE) build-html-container; \
	else \
		$(MAKE) build-html-no-container; \
	fi

build-html-container:
	@echo "Building HTML inside Docker container..."
	$(DOCKER_RUN) /bin/sh -c "$(ASCIIDOCTOR_HTML) $(HTML_OPTIONS) $(REQUIRES) --out-file=$(HTML_RESULT) $(HEADER_SOURCE)"
	@echo "HTML build completed."

build-html-no-container:
	@echo "Building HTML..."
	$(ASCIIDOCTOR_HTML) $(HTML_OPTIONS) $(REQUIRES) --out-file=$(HTML_RESULT) $(HEADER_SOURCE)
	@echo "HTML build completed."

clean:
	@echo "Cleaning up generated files..."
	rm -f $(PDF_RESULT) $(HTML_RESULT)
	@echo "Cleanup completed."
