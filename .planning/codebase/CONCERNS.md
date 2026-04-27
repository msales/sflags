# Concerns & Risks

## Technical Debt
- `Makefile` references deprecated tools (`golint`, `depscheck`, `golocc`) and uses `GO111MODULE=off` in the `tools` target
- `golangci-lint` invocation in `check` target uses `--no-config` and references deprecated linters (`megacheck`, `interfacer`, `gas`)
- `alecthomas/kingpin` dependency is archived/unmaintained (v2.2.6)
- `urfave/cli` v1 is used; v2/v3 is the current major version
- Some variable names use non-standard spelling (`splitted` instead of `split`, `fNameSplitted`)

## Missing Coverage
- No tests for `gen/` adapter packages visible at root level (may exist in subdirectories)
- No tests for `cmd/genvalues` code generator
- No benchmarks for reflection-heavy parsing operations
- `validator/` package test coverage unclear
- Map value parsing has limited test coverage

## Security
- Reflection is used extensively; malformed struct tags could cause unexpected behavior but not security issues
- No secrets or credentials in the codebase
- Dockerfile uses `GITHUB_TOKEN` build arg for private repo access

## Performance
- Reflection-based parsing has inherent overhead; typically called once at startup so not a concern
- No caching of parsed flags; each `ParseStruct` call re-parses the entire struct
- Generated value types avoid reflection at runtime for `Set`/`String`/`Type` operations
- `anyOf` helper uses linear scan; acceptable given small `reflect.Kind` slices
