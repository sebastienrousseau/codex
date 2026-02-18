# ADR 002: Testing Strategy

## Status

Accepted

## Context

The ecosystem contains projects in multiple languages (Rust, Python, JavaScript, Shell). We need a consistent testing strategy that ensures quality across all projects while respecting language-specific idioms and tools.

## Decision

We adopt a layered testing approach with language-specific tooling.

### Testing Layers

1. **Unit Tests** - Test individual functions and modules in isolation
2. **Integration Tests** - Test interactions between components
3. **End-to-End Tests** - Test complete workflows (where applicable)
4. **Property-Based Tests** - For complex algorithms (optional but encouraged)

### Language-Specific Tooling

#### Rust
- **Framework**: Built-in `#[test]` with `cargo test`
- **Coverage**: `cargo-tarpaulin` or `llvm-cov`
- **Property Testing**: `proptest` or `quickcheck`
- **Benchmarks**: `criterion`

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_function_name() {
        // Arrange
        let input = "test";

        // Act
        let result = function_under_test(input);

        // Assert
        assert_eq!(result, expected);
    }
}
```

#### Python
- **Framework**: `pytest`
- **Coverage**: `pytest-cov`
- **Mocking**: `pytest-mock` or `unittest.mock`
- **Property Testing**: `hypothesis`

```python
def test_function_name():
    # Arrange
    input_data = "test"

    # Act
    result = function_under_test(input_data)

    # Assert
    assert result == expected
```

#### JavaScript/TypeScript
- **Framework**: `vitest` (preferred) or `jest`
- **Coverage**: Built-in vitest coverage
- **Mocking**: Built-in vitest mocking

```typescript
import { describe, it, expect } from 'vitest';

describe('functionName', () => {
  it('should return expected result', () => {
    // Arrange
    const input = 'test';

    // Act
    const result = functionUnderTest(input);

    // Assert
    expect(result).toBe(expected);
  });
});
```

#### Shell
- **Framework**: `bats-core`
- **Assertions**: `bats-assert`
- **File Testing**: `bats-file`

```bash
@test "script should succeed with valid input" {
  run ./script.sh valid_input
  [ "$status" -eq 0 ]
}
```

### Coverage Requirements

| Project Type | Minimum Coverage |
|--------------|------------------|
| Core Libraries | 80% |
| CLI Tools | 70% |
| Web Projects | 60% |
| Scripts | 50% |

### Test Organization

```
project/
├── src/
│   └── lib.rs          # Source code
├── tests/
│   ├── unit/           # Unit tests (optional subfolder)
│   ├── integration/    # Integration tests
│   └── fixtures/       # Test data
└── benches/            # Benchmarks (Rust)
```

## Consequences

### Positive
- Consistent quality bar across projects
- Clear expectations for contributors
- Easier CI/CD configuration
- Better documentation through tests

### Negative
- Initial setup overhead for new projects
- Coverage requirements may slow development
- Different tools per language increases cognitive load

### Mitigations
- Use `devkit` templates to automate test setup
- Use `pipelines` for consistent CI configuration
- Document common patterns in this codex
