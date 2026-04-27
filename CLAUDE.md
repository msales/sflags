@~/CLAUDE.md

## Project Description

**Language:** Go 1.22  
**Module:** `github.com/msales/sflags`  
**Purpose:** Struct-to-flags parsing library. Generates CLI flags from Go struct definitions using reflection, with support for multiple CLI frameworks (cobra, pflag, kingpin, urfave/cli, standard flag). Supports struct tags for flag names, environment variable mappings, descriptions, validation, and nested structs.  
**Key dependencies:**
- `github.com/spf13/cobra` - Cobra CLI framework integration
- `github.com/spf13/pflag` - pflag integration
- `github.com/alecthomas/kingpin` - Kingpin CLI integration
- `github.com/urfave/cli` - urfave/cli integration
- `github.com/asaskevich/govalidator` - Validation support
- `github.com/stretchr/testify` - Testing assertions

## Directory Structure

```
cmd/genvalues/      Code generator for typed flag values (go generate)
examples/           Example integrations per CLI framework
  anonymous/        Anonymous struct embedding example
  cobra/            Cobra integration example
  flag/             Standard flag package example
  kingpin/          Kingpin integration example
  pflag/            pflag integration example
  simple_flag/      Simple flag example
  urfave_cli/       urfave/cli example
  validator/        Validation example
gen/                Generator adapters per framework
  gcli/             urfave/cli flag generator
  gflag/            Standard flag generator
  gkingpin/         Kingpin flag generator
  gpflag/           pflag flag generator
  govalidator/      govalidator integration
scripts/            Coverage scripts
validator/          Validation package
```

## Build, Test & Lint

```bash
# Build
go build ./...

# Run tests
go test -cover ./...

# Run tests with verbose output
go test -cover -v ./...

# Vet
go vet ./...

# Lint
golangci-lint run

# Format
goimports -w .

# Generate typed values
go generate

# Run all checks
make prepare

# Race detection
make race
```

The Makefile provides targets: `test`, `test_v`, `lint`, `check`, `vet`, `fmt`, `race`, `generate`, `coverage`, `prepare` (runs all).

## Code Style

### Imports

Two groups separated by blank lines:
1. Standard library
2. Third-party packages

No internal packages; this is a flat library.

### Naming Conventions

- **Packages:** short, lowercase (`sflags`, `gen`, `gpflag`, `gcli`)
- **Interfaces:** `Value`, `Getter`, `BoolFlag`, `RepeatableFlag` - describe the capability
- **Types:** PascalCase for exported (`Flag`, `Counter`, `HexBytes`), camelCase for unexported (`opts`, `validateValue`)
- **Functional options:** `OptFunc` pattern for configuration (`DescTag()`, `Prefix()`, `EnvPrefix()`, `FlagDivider()`)

### Error Handling

- Errors returned directly from `ParseStruct` with descriptive messages
- Validation errors propagated through `validateValue.Set()`
- Nil checks on reflect values before processing

### Testing

- `testify/assert` for assertions
- Extensive test coverage for parsers, values, and converters
- Generated test files (`values_generated_test.go`) for typed values
- Per-file test naming: `*_test.go` alongside source files
- Tests for each generator in `gen/` subdirectories

## Branching & Git

- **Never** work directly on `main` or `master`. Always create a new branch off the latest `main`/`master`.
- Before creating a branch, **always pull** the latest changes from the remote (`git pull origin master`).
- If the user provides a task identifier in the format `TRK-XXXX`, use it as the branch name (e.g., `TRK-1234`). Otherwise, **ask the user** for a task ID or branch name before proceeding.
- **Never push** to `main` or `master` directly.
- Commit changes after each meaningful iteration. Use your judgment to decide when progress should be saved — prefer smaller, atomic commits over large monolithic ones.
- Write clear, concise commit messages. Use conventional commit format when appropriate (e.g., `feat:`, `fix:`, `refactor:`, `test:`).
