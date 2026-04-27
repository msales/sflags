# Code Quality

## Test Coverage
- `parser_test.go` - tests for struct parsing logic
- `values_test.go` - tests for custom value types
- `values_generated_test.go` - auto-generated tests for all typed values
- `camelcase_test.go` - tests for CamelCase to flag-case conversion
- `converters_test.go` - tests for string conversion helpers
- Coverage script available at `scripts/cover_multi.sh`
- Makefile uses `-cover` flag on all test runs

## Code Patterns
- **Reflection-heavy** - core parsing relies on `reflect` package for struct introspection
- **Functional options** (`OptFunc`) for configurable parsing behavior
- **Code generation** - `go generate` produces typed value implementations from `values.json` template, reducing boilerplate
- **Interface composition** - `BoolFlag` and `RepeatableFlag` are optional interfaces checked via type assertion
- **Adapter pattern** - `gen/*` packages adapt the generic `[]*Flag` to framework-specific flag sets
- **Decorator pattern** - `validateValue` wraps any `Value` to add validation

## Error Handling
- Explicit nil/type checks before reflection operations
- Descriptive error messages from `ParseStruct` for invalid inputs
- Validation errors propagated through `validateValue.Set()` before delegating to underlying value

## Documentation
- Package-level GoDoc comment on `sflags` package
- All exported types, interfaces, and functions have GoDoc comments
- `README.md` at project root
- Example programs in `examples/` directory for each supported framework
- `values.json` serves as a schema for code generation
