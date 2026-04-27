# Architecture

## Overview
sflags is a reflection-based library that parses Go struct definitions into CLI flags. It supports multiple CLI frameworks through generator adapters and provides customizable struct tags for flag names, environment variables, descriptions, and validation.

## Package Structure
| Package | Responsibility |
|---|---|
| `sflags` (root) | Core parsing logic: `ParseStruct`, `Flag` type, `Value` interface, custom types (`Counter`, `HexBytes`), functional options |
| `cmd/genvalues` | Code generator that produces typed `Value` implementations from `values.json` template |
| `gen/gcli` | Generator adapter for `urfave/cli` flags |
| `gen/gflag` | Generator adapter for standard `flag` package |
| `gen/gkingpin` | Generator adapter for `alecthomas/kingpin` |
| `gen/gpflag` | Generator adapter for `spf13/pflag` |
| `gen/govalidator` | Integration with `asaskevich/govalidator` for field validation |
| `validator` | Validation package wrapping govalidator |
| `examples/*` | Usage examples for each supported CLI framework |

## Key Interfaces
- **`Value`** - Core interface: `String() string`, `Set(string) error`, `Type() string`
- **`Getter`** - Extends `Value` with `Get() interface{}`
- **`BoolFlag`** - Optional interface for boolean flags: `IsBoolFlag() bool`
- **`RepeatableFlag`** - Optional interface for cumulative flags: `IsCumulative() bool`
- **`ValidateFunc`** - Function type for custom validation: `func(val string, field reflect.StructField, cfg interface{}) error`
- **`OptFunc`** - Functional option: `func(opt *opts)`

## Data Flow
1. User passes a pointer to a struct to `ParseStruct(cfg, ...OptFunc)`
2. `parseStruct` iterates struct fields via reflection
3. For each field, `parseFlagTag` extracts flag name/options from struct tags
4. `parseEnv` extracts environment variable mapping
5. `parseVal` resolves the field to a `Value` implementation (generated or custom)
6. Nested structs recurse with accumulated prefix
7. Returns `[]*Flag` slice consumed by framework-specific generators (`gen/*`)
8. Generators (`GenerateTo`, `Parse`) convert `[]*Flag` into framework-native flag sets

## External Dependencies
None. This is a pure library with no external service dependencies.
