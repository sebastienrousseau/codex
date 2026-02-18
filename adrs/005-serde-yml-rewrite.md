# ADR-005: serde_yml Safe Rewrite

## Status

Proposed

## Date

2026-02-18

## Context

The ecosystem currently depends on `serde_yml` and `libyml` crates for YAML parsing. These crates:

1. **Are archived** - No longer maintained
2. **Contain unsafe code** - Use libyaml C bindings via FFI
3. **Cause vulnerabilities** - 151 security alerts across 9 repos
4. **Block ecosystem health** - Major contributor to "Critical" status in Pulse

### Affected Repositories

- nucleusflow
- vrd
- metadata-gen
- mdx-gen
- libmake
- frontmatter-gen
- rlg
- Plus transitive dependencies

### Alternative Considered: Migrate to serde_yaml

While `serde_yaml` exists as an alternative, we prefer creating a new safe implementation to:
- Eliminate all unsafe code
- Maintain API compatibility where practical
- Have full control over the dependency

## Decision

Create a new `serde_yml` library with the following requirements:

### Technical Requirements

1. **Pure Rust** - No C bindings, no FFI
2. **Zero unsafe blocks** - Entire codebase safe Rust
3. **YAML 1.2 support** - Modern YAML specification
4. **Full serde integration** - Serialize/Deserialize traits
5. **API compatibility** - Where practical with existing serde_yml

### Quality Requirements

1. Comprehensive test suite (80%+ coverage)
2. Fuzzing for security
3. Benchmarks vs alternatives
4. Full documentation

### Implementation Approach

Consider using `yaml-rust2` as the parsing backend (pure Rust YAML parser) with a serde adapter layer.

## Consequences

### Positive

- Eliminates 151 vulnerabilities across ecosystem
- Removes unsafe code dependency
- Full control over YAML parsing
- Modern, maintained codebase

### Negative

- Significant development effort
- Potential API breaking changes
- Migration effort across 9 repos

### Neutral

- May have different performance characteristics than libyaml

## Implementation Notes

Notes saved at: `~/Downloads/serde_yml_rewrite_notes.txt`

## References

- [yaml-rust2](https://crates.io/crates/yaml-rust2) - Pure Rust YAML parser
- [serde_yaml](https://crates.io/crates/serde_yaml) - Alternative (uses unsafe-libyaml)
- Pulse health report showing 151 vulnerabilities
