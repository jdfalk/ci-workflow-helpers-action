# file: .github/copilot-instructions.md
# version: 1.0.0
# guid: f6a7b8c9-d0e1-4234-f5a6-b7c8d9e0f1a2

# AI Agent Instructions (Standard)

- Use VS Code tasks for non-git operations; prefer MCP GitHub tools for git operations.
- Commits must use conventional commits (type(scope): description).
- Include versioned headers in docs/configs and bump versions on changes.
- This is a composite GitHub Action that embeds Python logic directly in action.yml.
- All changes to commands should be made in the embedded Python script within action.yml.
- Follow GitHub Actions composite action best practices.
- Provide concise plans and progress updates.

## Repository Context

This repository provides a composite GitHub Action that consolidates CI workflow helper commands for multi-language projects. It replaces the functionality from ghcommon's ci_workflow.py script.

**Key Files:**
- [action.yml](action.yml) - Main composite action with embedded Python script
- [README.md](README.md) - Comprehensive documentation and examples
- [CHANGELOG.md](CHANGELOG.md) - Version history

**Supported Languages:**
- Go (setup, test, coverage, benchmarks)
- Python (install, lint, test)
- Rust (format, clippy, coverage, threshold)
- Frontend (install, run scripts)
- Docker (build, compose)

**Design Pattern:**
- Composite action using `shell: python` to embed the entire Python script
- Single file action.yml contains all logic (no separate src/ directory needed)
- All environment variables passed as action inputs
- Outputs exposed via GITHUB_OUTPUT
