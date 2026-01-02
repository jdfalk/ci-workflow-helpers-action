# file: CHANGELOG.md

# version: 1.0.1

# guid: e5f6a7b8-c9d0-4123-e4f5-a6b7c8d9e0f1

# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] - 2025-12-30

### Fixed

- Updated action icon from 'tool' to 'terminal' for better visual consistency

## [1.0.0] - 2025-12-29

### Added

- Initial release of CI Workflow Helpers Action
- Comprehensive multi-language CI helper commands
- Support for Go, Python, Rust, Frontend (Node.js), and Docker
- Coverage analysis and threshold enforcement
- CI matrix generation from repository configuration
- Super Linter configuration management
- PR automation wait functionality
- CI orchestration and status checking
- Embedded Python implementation in composite action
- 33 helper commands covering all major CI workflows

### Features

- **Go Commands**: setup, test, coverage checking, benchmarks
- **Python Commands**: install, lint (ruff), test (pytest)
- **Rust Commands**: format, clippy, coverage (llvm-cov), threshold enforcement
- **Frontend Commands**: install (npm/yarn/pnpm), run scripts
- **Docker Commands**: build, compose validation
- **CI Orchestration**: debug filters, execution determination, status checking
- **Configuration**: Super Linter config loading, matrix generation
- **Documentation**: link checking, structure validation (placeholders)

### Migration

- Replaces functionality from ghcommon/.github/workflows/scripts/ci_workflow.py
- Provides drop-in replacement for Python script commands
- Supports all original environment variable configurations
