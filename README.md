# file: README.md

# version: 1.0.0

# guid: b2c3d4e5-f6a7-4890-b1c2-d3e4f5a6b7c8

# CI Workflow Helpers Action

Comprehensive CI workflow helper commands for multi-language projects. This
composite GitHub Action provides a collection of utility commands for testing,
building, coverage checking, and CI orchestration across Go, Python, Rust,
Frontend/Node.js, Docker, and documentation projects.

## Features

- **Multi-language Support**: Go, Python, Rust, Frontend (Node.js), Docker
- **Coverage Analysis**: Automated coverage checking and reporting
- **Matrix Generation**: Generate test matrices for multi-version CI
- **Super Linter Integration**: Load and manage Super Linter configurations
- **PR Automation**: Wait for PR automation workflows to complete
- **CI Orchestration**: Determine execution strategies, check CI status

## Usage

### Basic Example

```yaml
- name: Run Go Setup
  uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: go-setup

- name: Run Go Tests with Coverage
  uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: go-test
    coverage-threshold: '80'
```

### With Repository Configuration

```yaml
- name: Load Repository Config
  id: config
  uses: jdfalk/load-config-action@v1

- name: Generate CI Matrices
  id: matrices
  uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: generate-matrices
    repository-config: ${{ steps.config.outputs.config }}
```

## Available Commands

### CI Orchestration

| Command                  | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| `debug-filter`           | Debug file change detection filters                          |
| `determine-execution`    | Determine which CI steps should run based on file changes    |
| `wait-for-pr-automation` | Wait for PR automation workflow to complete                  |
| `generate-ci-summary`    | Generate comprehensive CI pipeline summary                   |
| `check-ci-status`        | Check overall CI pipeline status and fail if any jobs failed |

### Configuration

| Command                    | Description                                         |
| -------------------------- | --------------------------------------------------- |
| `load-super-linter-config` | Load Super Linter configuration based on event type |
| `write-validation-summary` | Write validation summary to step summary            |
| `generate-matrices`        | Generate CI matrices for all languages              |

### Go Commands

| Command             | Description                                |
| ------------------- | ------------------------------------------ |
| `go-setup`          | Download dependencies and build Go project |
| `go-test`           | Run Go tests with coverage                 |
| `check-go-coverage` | Check Go coverage against threshold        |
| `run-benchmarks`    | Run Go benchmarks                          |

### Python Commands

| Command            | Description                               |
| ------------------ | ----------------------------------------- |
| `python-install`   | Install Python dependencies               |
| `python-run-tests` | Run Python tests with pytest and coverage |
| `python-lint`      | Run ruff format and lint checks           |

### Rust Commands

| Command                      | Description                           |
| ---------------------------- | ------------------------------------- |
| `ensure-cargo-llvm-cov`      | Install cargo-llvm-cov if not present |
| `rust-format`                | Check Rust formatting with rustfmt    |
| `rust-clippy`                | Run cargo clippy linting              |
| `generate-rust-lcov`         | Generate lcov coverage report         |
| `generate-rust-html`         | Generate HTML coverage report         |
| `compute-rust-coverage`      | Compute coverage percentage from lcov |
| `enforce-coverage-threshold` | Enforce coverage threshold for Rust   |

### Frontend Commands

| Command            | Description                                   |
| ------------------ | --------------------------------------------- |
| `frontend-install` | Install frontend dependencies (npm/yarn/pnpm) |
| `frontend-run`     | Run a specific npm script                     |

### Docker Commands

| Command               | Description                           |
| --------------------- | ------------------------------------- |
| `docker-build`        | Build Docker image                    |
| `docker-test-compose` | Validate docker-compose configuration |

### Documentation Commands

| Command                   | Description                                    |
| ------------------------- | ---------------------------------------------- |
| `docs-check-links`        | Check documentation links (placeholder)        |
| `docs-validate-structure` | Validate documentation structure (placeholder) |

## Inputs

### Required Inputs

| Input     | Description        | Required | Default |
| --------- | ------------------ | -------- | ------- |
| `command` | Command to execute | Yes      | -       |

### Common Inputs

| Input                | Description                   | Default         |
| -------------------- | ----------------------------- | --------------- |
| `repository-config`  | Repository configuration JSON | `{}`            |
| `coverage-threshold` | Coverage threshold percentage | `0`             |
| `coverage-file`      | Coverage output file path     | `coverage.out`  |
| `coverage-html`      | HTML coverage report path     | `coverage.html` |

### Frontend-Specific Inputs

| Input                      | Description                    | Default             |
| -------------------------- | ------------------------------ | ------------------- |
| `frontend-working-dir`     | Working directory for frontend | `.`                 |
| `frontend-script`          | NPM script to run              | ``                  |
| `frontend-success-message` | Success message                | `Command succeeded` |
| `frontend-failure-message` | Failure message                | `Command failed`    |

### Rust-Specific Inputs

| Input                        | Description            | Default     |
| ---------------------------- | ---------------------- | ----------- |
| `lcov-output`                | Rust lcov output path  | `lcov.info` |
| `html-output-dir`            | Rust HTML coverage dir | `htmlcov`   |
| `clippy-all-features`        | Enable all features    | `false`     |
| `clippy-features`            | Specific features      | ``          |
| `clippy-no-default-features` | Disable defaults       | `false`     |
| `clippy-extra-args`          | Extra clippy args      | ``          |

### Docker-Specific Inputs

| Input             | Description        | Default      |
| ----------------- | ------------------ | ------------ |
| `dockerfile-path` | Path to Dockerfile | `Dockerfile` |
| `docker-image`    | Docker image name  | `test-image` |

### Super Linter Inputs

| Input         | Description      | Default               |
| ------------- | ---------------- | --------------------- |
| `pr-env-file` | PR mode env file | `super-linter-pr.env` |
| `ci-env-file` | CI mode env file | `super-linter-ci.env` |

### PR Automation Inputs

| Input           | Description            | Default         |
| --------------- | ---------------------- | --------------- |
| `target-sha`    | Target SHA for waiting | ``              |
| `workflow-name` | Workflow to wait for   | `PR Automation` |
| `max-attempts`  | Max wait attempts      | `60`            |
| `sleep-seconds` | Seconds between checks | `10`            |

### Fallback Version Inputs

| Input                         | Description             | Default  |
| ----------------------------- | ----------------------- | -------- |
| `fallback-go-version`         | Fallback Go version     | `1.24`   |
| `fallback-python-version`     | Fallback Python version | `3.13`   |
| `fallback-rust-version`       | Fallback Rust version   | `stable` |
| `fallback-node-version`       | Fallback Node version   | `22`     |
| `fallback-coverage-threshold` | Fallback coverage       | `80`     |

## Outputs

| Output                 | Description                       |
| ---------------------- | --------------------------------- |
| `skip_ci`              | Whether CI should be skipped      |
| `should_lint`          | Whether linting should run        |
| `should_test_go`       | Whether Go tests should run       |
| `should_test_frontend` | Whether Frontend tests should run |
| `should_test_python`   | Whether Python tests should run   |
| `should_test_rust`     | Whether Rust tests should run     |
| `should_test_docker`   | Whether Docker tests should run   |
| `config-file`          | Super Linter config file loaded   |
| `go-matrix`            | Generated Go CI matrix            |
| `python-matrix`        | Generated Python CI matrix        |
| `rust-matrix`          | Generated Rust CI matrix          |
| `frontend-matrix`      | Generated Frontend CI matrix      |
| `coverage-threshold`   | Coverage threshold from config    |
| `percent`              | Computed coverage percentage      |

## Examples

### Complete Go CI Workflow

```yaml
jobs:
  go-ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-go@v5
        with:
          go-version: '1.24'

      - name: Setup Go Project
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: go-setup

      - name: Run Go Tests
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: go-test
          coverage-threshold: '80'

      - name: Run Benchmarks
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: run-benchmarks
```

### Python CI with Linting

```yaml
jobs:
  python-ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: '3.13'

      - name: Install Dependencies
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: python-install

      - name: Lint Python Code
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: python-lint

      - name: Run Tests
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: python-run-tests
```

### Rust CI with Coverage

```yaml
jobs:
  rust-ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: dtolnay/rust-toolchain@stable

      - name: Install Coverage Tool
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: ensure-cargo-llvm-cov

      - name: Check Formatting
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: rust-format

      - name: Run Clippy
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: rust-clippy
          clippy-extra-args: '-- -D warnings'

      - name: Generate Coverage
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: generate-rust-lcov

      - name: Compute Coverage
        id: coverage
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: compute-rust-coverage

      - name: Enforce Threshold
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: enforce-coverage-threshold
          coverage-threshold: '80'
        env:
          COVERAGE_PERCENT: ${{ steps.coverage.outputs.percent }}
```

### Frontend CI

```yaml
jobs:
  frontend-ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: '22'

      - name: Install Dependencies
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: frontend-install
          frontend-working-dir: './client'

      - name: Build
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: frontend-run
          frontend-script: 'build'
          frontend-working-dir: './client'

      - name: Test
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: frontend-run
          frontend-script: 'test'
          frontend-working-dir: './client'
```

### Matrix Generation

```yaml
jobs:
  setup:
    runs-on: ubuntu-latest
    outputs:
      go-matrix: ${{ steps.matrices.outputs.go-matrix }}
      python-matrix: ${{ steps.matrices.outputs.python-matrix }}
    steps:
      - uses: actions/checkout@v4

      - name: Load Config
        id: config
        uses: jdfalk/load-config-action@v1

      - name: Generate Matrices
        id: matrices
        uses: jdfalk/ci-workflow-helpers-action@v1
        with:
          command: generate-matrices
          repository-config: ${{ steps.config.outputs.config }}

  go-ci:
    needs: setup
    strategy:
      matrix: ${{ fromJson(needs.setup.outputs.go-matrix) }}
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with:
          go-version: ${{ matrix.go-version }}
      # ... rest of CI steps
```

## Design Philosophy

This action consolidates CI workflow helper logic into a reusable, testable, and
maintainable composite action. It replaces scattered Python scripts with a
centralized tool that can be versioned and referenced across multiple
repositories.

**Key Benefits:**

- **Single Source of Truth**: One action replaces multiple script files
- **Versioned Releases**: Use `@v1`, `@v1.2.3`, or `@main` tags
- **Consistent Behavior**: Same logic across all repositories
- **Easy Updates**: Update once, propagate everywhere
- **Self-Contained**: All logic embedded in action.yml

## Migration from ci_workflow.py

If you're migrating from the original `ci_workflow.py` script:

**Before:**

```yaml
- name: Run Tests
  run: python .github/workflows/scripts/ci_workflow.py go-test
  env:
    COVERAGE_THRESHOLD: '80'
```

**After:**

```yaml
- name: Run Tests
  uses: jdfalk/ci-workflow-helpers-action@v1
  with:
    command: go-test
    coverage-threshold: '80'
```

## License

MIT

## Contributing

Contributions welcome! Please open issues or pull requests on GitHub.

## Author

Created by [jdfalk](https://github.com/jdfalk) as part of the ghcommon CI
infrastructure suite.
