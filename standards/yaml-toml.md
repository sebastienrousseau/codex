# YAML & TOML Standards

Standards for configuration files in the ecosystem.

## YAML Standards

### File Extensions

- Use `.yaml` (not `.yml`)
- Exception: Docker Compose uses `.yml` by convention

### Formatting

```yaml
# 2 space indentation
# No tabs
# Spaces around colons in mappings

# Good
server:
  host: localhost
  port: 8080

# Avoid
server:
    host:localhost
    port:8080
```

### Strings

```yaml
# Plain strings (preferred when possible)
name: my-project

# Quoted strings (when special characters present)
description: "Project: A great tool"
path: '/usr/local/bin'

# Multiline strings
readme: |
  This is a multiline string.
  Line breaks are preserved.

  Blank lines too.

# Folded strings (newlines become spaces)
description: >
  This is a long description
  that will be folded into
  a single line.
```

### Lists

```yaml
# Block style (preferred)
languages:
  - rust
  - python
  - javascript

# Inline style (for short lists)
tags: [cli, tools, automation]
```

### Mappings

```yaml
# Block style
database:
  host: localhost
  port: 5432
  name: mydb

# Inline style (for simple mappings)
point: {x: 10, y: 20}
```

### Comments

```yaml
# Section comment
server:
  # Field comment
  host: localhost  # Inline comment (use sparingly)
  port: 8080
```

### Anchors and Aliases

```yaml
# Define anchor
defaults: &defaults
  timeout: 30
  retries: 3

# Use alias
development:
  <<: *defaults
  debug: true

production:
  <<: *defaults
  debug: false
```

### GitHub Actions Specific

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run tests
        run: cargo test
```

## TOML Standards

### File Structure

```toml
# Package/project metadata first
[package]
name = "my-project"
version = "1.0.0"

# Dependencies next
[dependencies]
serde = "1.0"

# Dev dependencies
[dev-dependencies]
proptest = "1.0"

# Features last
[features]
default = ["std"]
std = []
```

### Strings

```toml
# Basic strings
name = "my-project"

# Literal strings (no escaping)
path = 'C:\Users\name'

# Multiline basic strings
description = """
This is a long description
that spans multiple lines.
"""

# Multiline literal strings
pattern = '''
\d+\.\d+\.\d+
'''
```

### Numbers

```toml
# Integers
port = 8080
hex = 0xFF
octal = 0o755
binary = 0b1010

# Floats
pi = 3.14159
scientific = 1.0e6

# Underscores for readability
large_number = 1_000_000
```

### Booleans

```toml
enabled = true
debug = false
```

### Arrays

```toml
# Inline (for short arrays)
tags = ["cli", "tools"]

# Multiline
authors = [
    "Name One <one@example.com>",
    "Name Two <two@example.com>",
]

# Array of tables
[[bin]]
name = "tool"
path = "src/bin/tool.rs"

[[bin]]
name = "other"
path = "src/bin/other.rs"
```

### Tables

```toml
# Standard table
[server]
host = "localhost"
port = 8080

# Nested tables
[server.ssl]
enabled = true
cert = "/path/to/cert.pem"

# Inline tables (for simple cases)
point = { x = 10, y = 20 }
```

### Comments

```toml
# This is a comment

[section]  # Inline comments work too
key = "value"  # But use sparingly
```

### Cargo.toml Specifics

```toml
[package]
name = "my-crate"
version = "0.1.0"
edition = "2021"
authors = ["Name <email@example.com>"]
description = "A short description"
license = "MIT OR Apache-2.0"
repository = "https://github.com/user/repo"
documentation = "https://docs.rs/my-crate"
readme = "README.md"
keywords = ["keyword1", "keyword2"]
categories = ["category"]
rust-version = "1.70"

[dependencies]
# Version requirements
serde = "1.0"              # ^1.0.0
tokio = "1"                # ^1.0.0
exact = "=1.2.3"           # Exactly 1.2.3
range = ">=1.0, <2.0"      # Range

# Features
serde = { version = "1.0", features = ["derive"] }

# Optional dependencies
optional-dep = { version = "1.0", optional = true }

# Git dependencies
my-crate = { git = "https://github.com/user/repo" }
my-crate = { git = "https://github.com/user/repo", branch = "dev" }

# Path dependencies (workspace)
shared = { path = "../shared" }

[dev-dependencies]
criterion = "0.5"

[build-dependencies]
cc = "1.0"

[features]
default = ["std"]
std = []
full = ["feature1", "feature2"]

[profile.release]
lto = true
codegen-units = 1
```

### pyproject.toml Specifics

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "my-package"
version = "0.1.0"
description = "Package description"
readme = "README.md"
requires-python = ">=3.9"
license = "MIT"
authors = [
    { name = "Name", email = "email@example.com" },
]
classifiers = [
    "Development Status :: 4 - Beta",
    "Programming Language :: Python :: 3",
]
dependencies = [
    "httpx>=0.25.0",
    "pydantic>=2.0.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0",
    "ruff>=0.1.0",
]

[project.scripts]
my-cli = "my_package.cli:main"

[project.urls]
Homepage = "https://github.com/user/repo"
Documentation = "https://docs.example.com"

[tool.ruff]
line-length = 100
target-version = "py39"

[tool.pytest.ini_options]
testpaths = ["tests"]
```

## Validation

### YAML
```bash
# yamllint
yamllint .github/workflows/*.yaml

# Python
python -c "import yaml; yaml.safe_load(open('file.yaml'))"
```

### TOML
```bash
# taplo
taplo check Cargo.toml

# Python
python -c "import tomllib; tomllib.load(open('file.toml', 'rb'))"
```

## Common Mistakes

### YAML
```yaml
# Wrong - unquoted special characters
message: Hello: World    # Breaks on colon

# Right
message: "Hello: World"

# Wrong - tabs
server:
	port: 8080           # Tab character

# Right
server:
  port: 8080             # Spaces
```

### TOML
```toml
# Wrong - bare keys with special characters
my-key with spaces = "value"  # Invalid

# Right
"my-key with spaces" = "value"

# Wrong - multiline inline tables
point = {
    x = 10,
    y = 20
}  # Invalid in TOML

# Right
[point]
x = 10
y = 20
```
