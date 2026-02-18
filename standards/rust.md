# Rust Coding Standards

Standards and conventions for Rust projects in the ecosystem.

## Toolchain

- **Minimum Rust Version:** 1.75+ (or latest stable)
- **Edition:** 2021
- **Formatter:** rustfmt (default configuration)
- **Linter:** clippy with `-D warnings`

## Project Structure

```
project/
├── Cargo.toml
├── Cargo.lock          # Commit for binaries, ignore for libraries
├── src/
│   ├── lib.rs          # Library entry point
│   ├── main.rs         # Binary entry point (if applicable)
│   └── modules/
├── tests/              # Integration tests
├── benches/            # Benchmarks
├── examples/           # Example usage
├── README.md
├── LICENSE-MIT
├── LICENSE-APACHE
└── .github/
    └── workflows/
        └── ci.yml
```

## Naming Conventions

| Item | Convention | Example |
|------|------------|---------|
| Crates | snake_case | `my_crate` |
| Modules | snake_case | `my_module` |
| Types | PascalCase | `MyStruct` |
| Traits | PascalCase | `MyTrait` |
| Functions | snake_case | `my_function` |
| Constants | SCREAMING_SNAKE_CASE | `MAX_SIZE` |
| Static | SCREAMING_SNAKE_CASE | `GLOBAL_CONFIG` |

## Error Handling

### Use `thiserror` for Library Errors

```rust
use thiserror::Error;

#[derive(Error, Debug)]
pub enum MyError {
    #[error("Invalid input: {0}")]
    InvalidInput(String),

    #[error("IO error: {0}")]
    Io(#[from] std::io::Error),
}
```

### Use `anyhow` for Application Errors

```rust
use anyhow::{Context, Result};

fn main() -> Result<()> {
    let config = read_config()
        .context("Failed to read configuration")?;
    Ok(())
}
```

## Documentation

### Public API Documentation Required

```rust
/// Short summary of the function.
///
/// Longer description if needed.
///
/// # Arguments
///
/// * `input` - Description of input parameter
///
/// # Returns
///
/// Description of return value
///
/// # Errors
///
/// Returns error when...
///
/// # Examples
///
/// ```rust
/// let result = my_function("input");
/// assert_eq!(result, expected);
/// ```
pub fn my_function(input: &str) -> Result<Output> {
    // ...
}
```

## Testing

### Test Organization

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_basic_functionality() {
        // Arrange
        let input = "test";

        // Act
        let result = my_function(input);

        // Assert
        assert_eq!(result, expected);
    }
}
```

### Coverage Target

- Minimum: 80%
- Target: 90%+

## Dependencies

### Selection Criteria

1. **Active maintenance:** Check last commit date
2. **Security:** Review advisory database
3. **License compatibility:** MIT/Apache-2.0 preferred
4. **Minimal transitive dependencies**

### Common Dependencies

| Purpose | Crate |
|---------|-------|
| Error handling (lib) | `thiserror` |
| Error handling (app) | `anyhow` |
| Serialization | `serde` |
| Async runtime | `tokio` |
| HTTP client | `reqwest` |
| CLI | `clap` |
| Logging | `tracing` |

## CI/CD Requirements

Every Rust project must have:

```yaml
# .github/workflows/ci.yml
- cargo fmt --check
- cargo clippy -- -D warnings
- cargo test
- cargo audit
```

## Performance

### Benchmark Critical Paths

```rust
#[bench]
fn bench_critical_function(b: &mut Bencher) {
    b.iter(|| {
        critical_function()
    });
}
```

## Security

- Run `cargo audit` in CI
- Use `cargo deny` for license and dependency checks
- Avoid `unsafe` unless absolutely necessary
- Document all `unsafe` blocks with safety comments
