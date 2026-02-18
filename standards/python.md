# Python Coding Standards

Standards and conventions for Python projects in the ecosystem.

## Toolchain

- **Python Version:** 3.9+ (prefer 3.11+)
- **Package Manager:** Poetry or uv
- **Formatter:** Ruff (or Black)
- **Linter:** Ruff
- **Type Checker:** mypy (strict mode)

## Project Structure

```
project/
├── pyproject.toml      # Project configuration (Poetry/Hatch)
├── src/
│   └── package_name/
│       ├── __init__.py
│       ├── main.py
│       └── modules/
├── tests/
│   ├── __init__.py
│   ├── conftest.py
│   └── test_*.py
├── docs/
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
| Packages | snake_case | `my_package` |
| Modules | snake_case | `my_module.py` |
| Classes | PascalCase | `MyClass` |
| Functions | snake_case | `my_function` |
| Constants | SCREAMING_SNAKE_CASE | `MAX_SIZE` |
| Variables | snake_case | `my_variable` |
| Private | _prefix | `_internal_method` |

## Type Hints

### Required for All Public APIs

```python
from typing import Optional, List, Dict

def process_items(
    items: List[str],
    config: Optional[Dict[str, str]] = None,
) -> List[str]:
    """Process a list of items."""
    ...
```

### Use Modern Type Syntax (3.9+)

```python
# Good (3.9+)
def get_items() -> list[str]:
    ...

# Avoid (older style)
from typing import List
def get_items() -> List[str]:
    ...
```

## Documentation

### Docstring Style: Google

```python
def fetch_data(url: str, timeout: int = 30) -> dict:
    """Fetch data from a URL.

    Retrieves JSON data from the specified URL with
    configurable timeout.

    Args:
        url: The URL to fetch data from.
        timeout: Request timeout in seconds. Defaults to 30.

    Returns:
        A dictionary containing the parsed JSON response.

    Raises:
        ConnectionError: If the URL is unreachable.
        ValueError: If the response is not valid JSON.

    Example:
        >>> data = fetch_data("https://api.example.com/data")
        >>> print(data["status"])
        'ok'
    """
    ...
```

## Error Handling

### Custom Exceptions

```python
class PackageError(Exception):
    """Base exception for package errors."""
    pass

class ConfigurationError(PackageError):
    """Raised when configuration is invalid."""
    pass

class NetworkError(PackageError):
    """Raised when network operations fail."""
    pass
```

### Context Managers for Resources

```python
from contextlib import contextmanager

@contextmanager
def managed_resource():
    resource = acquire_resource()
    try:
        yield resource
    finally:
        release_resource(resource)
```

## Testing

### Use pytest

```python
import pytest

class TestMyFunction:
    """Tests for my_function."""

    def test_basic_input(self):
        """Test with basic input."""
        result = my_function("input")
        assert result == "expected"

    def test_edge_case(self):
        """Test edge case handling."""
        with pytest.raises(ValueError):
            my_function("")

    @pytest.mark.parametrize("input,expected", [
        ("a", "A"),
        ("b", "B"),
    ])
    def test_multiple_cases(self, input, expected):
        """Test multiple input/output pairs."""
        assert my_function(input) == expected
```

### Coverage Target

- Minimum: 80%
- Target: 95%+ (pain001 standard)

## Dependencies

### pyproject.toml Configuration

```toml
[project]
name = "my-package"
version = "0.1.0"
requires-python = ">=3.9"
dependencies = [
    "httpx>=0.24",
    "pydantic>=2.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0",
    "pytest-cov>=4.0",
    "ruff>=0.1",
    "mypy>=1.0",
]
```

### Common Dependencies

| Purpose | Package |
|---------|---------|
| HTTP Client | `httpx` |
| Data Validation | `pydantic` |
| CLI | `typer` or `click` |
| Testing | `pytest` |
| Async | `asyncio` (stdlib) |

## CI/CD Requirements

```yaml
# .github/workflows/ci.yml
- ruff check .
- ruff format --check .
- mypy src/
- pytest --cov=src --cov-report=xml
```

## Security

- Use `bandit` for security scanning
- Never commit secrets or credentials
- Use environment variables for sensitive config
- Validate all external input
- Keep dependencies updated

## Async Guidelines

```python
import asyncio
from typing import AsyncIterator

async def fetch_all(urls: list[str]) -> list[dict]:
    """Fetch multiple URLs concurrently."""
    async with httpx.AsyncClient() as client:
        tasks = [client.get(url) for url in urls]
        responses = await asyncio.gather(*tasks)
        return [r.json() for r in responses]
```
