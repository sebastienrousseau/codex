# Testing Guide

Comprehensive testing practices for the ecosystem.

## Testing Philosophy

- **Test behavior, not implementation** - Tests should verify what code does, not how
- **Keep tests fast** - Slow tests don't get run
- **Make tests deterministic** - No flaky tests
- **Test at the right level** - Unit tests for logic, integration tests for interactions

## Test Structure

### Arrange-Act-Assert (AAA)

```rust
#[test]
fn test_add_numbers() {
    // Arrange
    let a = 5;
    let b = 3;

    // Act
    let result = add(a, b);

    // Assert
    assert_eq!(result, 8);
}
```

### Given-When-Then (BDD Style)

```python
def test_user_login():
    # Given a valid user
    user = create_user(email="test@example.com", password="secure123")

    # When they attempt to login
    result = login(email="test@example.com", password="secure123")

    # Then login should succeed
    assert result.success is True
    assert result.user.email == "test@example.com"
```

## Rust Testing

### Unit Tests

```rust
// src/lib.rs
pub fn calculate_discount(price: f64, percentage: u8) -> f64 {
    price * (1.0 - percentage as f64 / 100.0)
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_calculate_discount_10_percent() {
        let result = calculate_discount(100.0, 10);
        assert!((result - 90.0).abs() < f64::EPSILON);
    }

    #[test]
    fn test_calculate_discount_zero() {
        let result = calculate_discount(100.0, 0);
        assert!((result - 100.0).abs() < f64::EPSILON);
    }

    #[test]
    fn test_calculate_discount_full() {
        let result = calculate_discount(100.0, 100);
        assert!((result - 0.0).abs() < f64::EPSILON);
    }
}
```

### Integration Tests

```rust
// tests/integration_test.rs
use my_crate::Config;

#[test]
fn test_config_loading() {
    let config = Config::from_file("tests/fixtures/config.toml")
        .expect("Should load config");

    assert_eq!(config.get::<String>("name"), Some("test".into()));
}
```

### Async Tests

```rust
#[tokio::test]
async fn test_async_operation() {
    let result = fetch_data("https://api.example.com").await;
    assert!(result.is_ok());
}
```

### Property-Based Testing

```rust
use proptest::prelude::*;

proptest! {
    #[test]
    fn test_parse_format_roundtrip(duration in 1u64..1000000) {
        let formatted = format_duration(Duration::from_secs(duration));
        let parsed = parse_duration(&formatted).unwrap();
        // Allow some precision loss
        prop_assert!((parsed.as_secs() as i64 - duration as i64).abs() < 60);
    }
}
```

### Running Tests

```bash
# All tests
cargo test

# Specific test
cargo test test_name

# With output
cargo test -- --nocapture

# Doc tests only
cargo test --doc

# Ignored tests
cargo test -- --ignored
```

## Python Testing

### pytest Basics

```python
# tests/test_calculator.py
import pytest
from calculator import add, divide

def test_add_positive_numbers():
    assert add(2, 3) == 5

def test_add_negative_numbers():
    assert add(-1, -1) == -2

def test_divide_by_zero():
    with pytest.raises(ZeroDivisionError):
        divide(1, 0)
```

### Fixtures

```python
import pytest

@pytest.fixture
def sample_user():
    return User(name="Test", email="test@example.com")

@pytest.fixture
def database():
    db = create_test_database()
    yield db
    db.cleanup()

def test_user_save(database, sample_user):
    database.save(sample_user)
    assert database.get_user(sample_user.id) == sample_user
```

### Parameterized Tests

```python
import pytest

@pytest.mark.parametrize("input,expected", [
    ("hello", "HELLO"),
    ("World", "WORLD"),
    ("", ""),
    ("123", "123"),
])
def test_uppercase(input, expected):
    assert input.upper() == expected
```

### Mocking

```python
from unittest.mock import Mock, patch

def test_api_call():
    with patch('module.requests.get') as mock_get:
        mock_get.return_value.json.return_value = {"data": "test"}

        result = fetch_api_data()

        assert result == {"data": "test"}
        mock_get.assert_called_once()
```

### Running Tests

```bash
# All tests
pytest

# Verbose
pytest -v

# Specific file
pytest tests/test_calculator.py

# Specific test
pytest tests/test_calculator.py::test_add

# With coverage
pytest --cov=src --cov-report=html

# Stop on first failure
pytest -x
```

## JavaScript/TypeScript Testing

### Vitest Basics

```typescript
// src/calculator.test.ts
import { describe, it, expect } from 'vitest';
import { add, divide } from './calculator';

describe('Calculator', () => {
  describe('add', () => {
    it('adds positive numbers', () => {
      expect(add(2, 3)).toBe(5);
    });

    it('adds negative numbers', () => {
      expect(add(-1, -1)).toBe(-2);
    });
  });

  describe('divide', () => {
    it('throws on division by zero', () => {
      expect(() => divide(1, 0)).toThrow('Division by zero');
    });
  });
});
```

### Async Testing

```typescript
import { describe, it, expect } from 'vitest';

describe('API', () => {
  it('fetches data', async () => {
    const data = await fetchData();
    expect(data).toHaveProperty('id');
  });
});
```

### Mocking

```typescript
import { vi, describe, it, expect } from 'vitest';

vi.mock('./api', () => ({
  fetchUser: vi.fn().mockResolvedValue({ id: 1, name: 'Test' }),
}));

describe('UserService', () => {
  it('gets user', async () => {
    const user = await getUser(1);
    expect(user.name).toBe('Test');
  });
});
```

### Running Tests

```bash
# All tests
pnpm test

# Watch mode
pnpm test --watch

# Coverage
pnpm test --coverage

# UI mode
pnpm test --ui
```

## Test Coverage

### Coverage Goals

| Type | Target |
|------|--------|
| Core libraries | 80%+ |
| CLI tools | 70%+ |
| Web projects | 60%+ |

### Measuring Coverage

```bash
# Rust
cargo tarpaulin --out Html

# Python
pytest --cov=src --cov-report=html

# JavaScript
pnpm test --coverage
```

## Test Data & Fixtures

### Fixture Files

```
tests/
├── fixtures/
│   ├── config.toml
│   ├── sample_input.json
│   └── expected_output.json
└── test_processor.py
```

### Factory Functions

```python
def make_user(**overrides):
    defaults = {
        "name": "Test User",
        "email": "test@example.com",
        "active": True,
    }
    return User(**{**defaults, **overrides})

def test_inactive_user():
    user = make_user(active=False)
    assert not user.can_login()
```

## CI Integration

Tests run automatically via pipelines:

```yaml
# Uses pipelines/rust-ci.yml or python-ci.yml
- uses: sebastienrousseau/pipelines/.github/workflows/rust-ci.yml@main
```

## Debugging Tests

### Rust
```bash
# Print output
cargo test -- --nocapture

# Run single test with backtrace
RUST_BACKTRACE=1 cargo test test_name
```

### Python
```bash
# Drop into debugger on failure
pytest --pdb

# Verbose output
pytest -vv
```

### JavaScript
```bash
# Debug mode
node --inspect-brk node_modules/.bin/vitest
```
