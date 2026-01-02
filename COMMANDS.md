# file: COMMANDS.md

# version: 1.0.0

# guid: b8c9d0e1-f2a3-4456-b7c8-d9e0f1a2b3c4

# Command Quick Reference

Quick reference for all available CI Workflow Helper commands.

## Syntax

```yaml
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: <command-name>
    # ... additional inputs
```

## Command Categories

### 🔧 CI Orchestration

```yaml
# Debug file change detection
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: debug-filter

# Determine which CI steps should run
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: determine-execution

# Wait for PR automation to complete
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: wait-for-pr-automation
    target-sha: ${{ github.event.pull_request.head.sha }}
    workflow-name: 'PR Automation'

# Generate CI pipeline summary
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: generate-ci-summary

# Check overall CI status
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: check-ci-status
```

### ⚙️ Configuration

```yaml
# Load Super Linter config
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: load-super-linter-config
    pr-env-file: super-linter-pr.env
    ci-env-file: super-linter-ci.env

# Write validation summary
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: write-validation-summary

# Generate CI matrices
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: generate-matrices
    repository-config: ${{ steps.config.outputs.config }}
```

### 🔷 Go Commands

```yaml
# Setup Go project
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: go-setup

# Run Go tests with coverage
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: go-test
    coverage-threshold: '80'

# Check existing coverage
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: check-go-coverage
    coverage-file: coverage.out
    coverage-threshold: '80'

# Run benchmarks
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: run-benchmarks
```

### 🐍 Python Commands

```yaml
# Install Python dependencies
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: python-install

# Run Python linting (ruff)
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: python-lint

# Run Python tests
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: python-run-tests
```

### 🦀 Rust Commands

```yaml
# Install cargo-llvm-cov
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: ensure-cargo-llvm-cov

# Check Rust formatting
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: rust-format

# Run cargo clippy
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: rust-clippy
    clippy-all-features: 'true'
    clippy-extra-args: '-- -D warnings'

# Generate lcov coverage
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: generate-rust-lcov
    lcov-output: lcov.info

# Generate HTML coverage
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: generate-rust-html
    html-output-dir: htmlcov

# Compute coverage percentage
- uses: jdfalk/ci-workflow-helpers-action@v1
  id: coverage
  with:
    command: compute-rust-coverage

# Enforce coverage threshold
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: enforce-coverage-threshold
    coverage-threshold: '80'
  env:
    COVERAGE_PERCENT: ${{ steps.coverage.outputs.percent }}
```

### 📦 Frontend Commands

```yaml
# Install frontend dependencies
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: frontend-install
    frontend-working-dir: './client'

# Run npm script
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: frontend-run
    frontend-script: 'build'
    frontend-working-dir: './client'
    frontend-success-message: 'Build succeeded!'
    frontend-failure-message: 'Build failed'
```

### 🐳 Docker Commands

```yaml
# Build Docker image
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: docker-build
    dockerfile-path: Dockerfile
    docker-image: my-app:latest

# Test docker-compose config
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: docker-test-compose
```

### 📚 Documentation Commands

```yaml
# Check documentation links (placeholder)
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: docs-check-links

# Validate documentation structure (placeholder)
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: docs-validate-structure
```

## Common Patterns

### Full Go CI Pipeline

```yaml
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: go-setup

- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: go-test
    coverage-threshold: '80'

- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: run-benchmarks
```

### Full Python CI Pipeline

```yaml
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: python-install

- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: python-lint

- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: python-run-tests
```

### Full Rust CI Pipeline with Coverage

```yaml
- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: ensure-cargo-llvm-cov

- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: rust-format

- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: rust-clippy

- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: generate-rust-lcov

- uses: jdfalk/ci-workflow-helpers-action@v1
  id: coverage
  with:
    command: compute-rust-coverage

- uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: enforce-coverage-threshold
    coverage-threshold: '80'
  env:
    COVERAGE_PERCENT: ${{ steps.coverage.outputs.percent }}
```

### Matrix Generation and Usage

```yaml
jobs:
  setup:
    runs-on: ubuntu-latest
    outputs:
      go-matrix: ${{ steps.matrices.outputs.go-matrix }}
    steps:
      - uses: actions/checkout@v4
      - uses: jdfalk/load-config-action@v1
        id: config
      - uses: jdfalk/ci-workflow-helpers-action@v1
        id: matrices
        with:
          command: generate-matrices
          repository-config: ${{ steps.config.outputs.config }}

  test:
    needs: setup
    strategy:
      matrix: ${{ fromJson(needs.setup.outputs.go-matrix) }}
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/setup-go@v5
        with:
          go-version: ${{ matrix.go-version }}
      # ... test steps
```

## Environment Variables

Most commands respect these environment variables when set:

- `REPOSITORY_CONFIG` - JSON configuration from load-config-action
- `CI_GO_FILES`, `CI_PYTHON_FILES`, etc. - File change detection flags
- `GITHUB_OUTPUT` - GitHub Actions output file (set automatically)
- `GITHUB_STEP_SUMMARY` - GitHub Actions summary file (set automatically)
- `GITHUB_ENV` - GitHub Actions environment file (set automatically)

## Exit Codes

- `0` - Success
- `1` - General failure
- Command-specific exit codes for coverage threshold violations, test failures,
  etc.
