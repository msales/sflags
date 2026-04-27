# Technology Stack

## Language & Runtime
Go 1.22.0

## Dependencies
| Dependency | Purpose |
|---|---|
| `github.com/spf13/cobra` | Cobra CLI framework integration |
| `github.com/spf13/pflag` | pflag flag set integration |
| `github.com/alecthomas/kingpin` | Kingpin CLI framework integration |
| `github.com/urfave/cli` | urfave/cli framework integration |
| `github.com/asaskevich/govalidator` | Struct field validation |
| `github.com/stretchr/testify` | Test assertions |
| `github.com/davecgh/go-spew` | Debug pretty printing |

## Build System
- **Makefile** with targets: `test`, `test_v`, `lint`, `check` (golangci-lint), `vet`, `fmt` (goimports), `race`, `generate`, `coverage`, `prepare` (all), `tools`
- `go generate` runs `cmd/genvalues/main.go` to produce `values_generated.go` and `values_generated_test.go`
- **Dockerfile** uses `msales/go-builder:1.22-base-1.0.1` for build verification only (library, not a service)
- Coverage script at `scripts/cover_multi.sh`

## Testing
- `testify/assert` for assertions
- Table-driven tests for parser, values, and converters
- Generated test files for typed value implementations
- Race detection via `make race`
- `golangci-lint` for static analysis

## CI/CD
- Makefile `travis` target suggests Travis CI usage: `info vet lint check test_v coverage`
- No `.github/workflows/` directory found
