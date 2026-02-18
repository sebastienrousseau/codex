# Security Guide

Best practices for security across the ecosystem.

## Security Principles

1. **Defense in depth** - Multiple layers of protection
2. **Least privilege** - Minimal permissions required
3. **Fail securely** - Errors should not expose sensitive data
4. **Trust nothing** - Validate all inputs

## Secure Coding Practices

### Input Validation

Always validate and sanitize inputs:

```rust
// Rust
fn process_input(input: &str) -> Result<Output, Error> {
    // Validate length
    if input.len() > MAX_LENGTH {
        return Err(Error::InputTooLong);
    }

    // Validate format
    if !is_valid_format(input) {
        return Err(Error::InvalidFormat);
    }

    // Process validated input
    Ok(process(input))
}
```

```python
# Python
def process_input(input_data: str) -> Output:
    # Validate length
    if len(input_data) > MAX_LENGTH:
        raise ValueError("Input too long")

    # Validate format
    if not is_valid_format(input_data):
        raise ValueError("Invalid format")

    return process(input_data)
```

### Secrets Management

**Never commit secrets:**
- API keys
- Passwords
- Private keys
- Tokens
- Connection strings

**Use environment variables:**
```bash
# Load from environment
export API_KEY="secret-key"
```

```rust
// Rust
let api_key = std::env::var("API_KEY")
    .expect("API_KEY must be set");
```

```python
# Python
import os
api_key = os.environ["API_KEY"]
```

**Use secret scanners:**
- Pre-commit hooks (devkit)
- GitHub secret scanning
- `gitleaks` or `truffleHog`

### Error Handling

Don't expose internal details:

```rust
// Bad - exposes internal path
Err(format!("Failed to read /etc/secret/config.json: {}", e))

// Good - generic message
Err(Error::ConfigurationError)
```

```python
# Bad
raise Exception(f"Database error: {connection_string}")

# Good
logger.error(f"Database error: {e}")  # Log internally
raise DatabaseError("Failed to connect")  # User-facing
```

### SQL Injection Prevention

Always use parameterized queries:

```python
# Bad
cursor.execute(f"SELECT * FROM users WHERE id = {user_id}")

# Good
cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))
```

### Path Traversal Prevention

Validate file paths:

```rust
use std::path::Path;

fn read_file(filename: &str) -> Result<String, Error> {
    let path = Path::new(filename);

    // Check for path traversal
    if path.components().any(|c| c == std::path::Component::ParentDir) {
        return Err(Error::InvalidPath);
    }

    // Ensure within allowed directory
    let full_path = allowed_dir.join(path);
    if !full_path.starts_with(&allowed_dir) {
        return Err(Error::InvalidPath);
    }

    std::fs::read_to_string(full_path)
        .map_err(|_| Error::FileNotFound)
}
```

## Dependency Security

### Automated Scanning

Use CI pipelines for security scanning:

```yaml
# .github/workflows/security.yml
- uses: sebastienrousseau/pipelines/.github/workflows/security.yml@main
```

### Manual Auditing

```bash
# Rust
cargo audit

# Python
pip-audit
safety check

# JavaScript
npm audit
pnpm audit
```

### Responding to Vulnerabilities

1. **Assess severity** - CVSS score, exploitability
2. **Check exposure** - Is vulnerable code path used?
3. **Update dependency** - If patch available
4. **Mitigate** - If no patch, add workarounds
5. **Document** - Record decision in ADR if needed

## Authentication & Authorization

### Password Handling

Never store plaintext passwords:

```rust
// Use argon2 for password hashing
use argon2::{self, Config};

fn hash_password(password: &str) -> String {
    let salt = generate_random_salt();
    let config = Config::default();
    argon2::hash_encoded(password.as_bytes(), &salt, &config)
        .expect("Failed to hash password")
}

fn verify_password(password: &str, hash: &str) -> bool {
    argon2::verify_encoded(hash, password.as_bytes())
        .unwrap_or(false)
}
```

### Token Management

- Use short-lived tokens
- Implement token rotation
- Store tokens securely (not in localStorage for web)

## HTTPS & TLS

- Always use HTTPS in production
- Use TLS 1.2+ only
- Validate certificates

```rust
// Rust with reqwest
let client = reqwest::Client::builder()
    .min_tls_version(reqwest::tls::Version::TLS_1_2)
    .build()?;
```

## Security Headers (Web)

```
Content-Security-Policy: default-src 'self'
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

## Reporting Vulnerabilities

See [SECURITY.md](../SECURITY.md) for reporting process.

## Security Checklist

Before release:

- [ ] No hardcoded secrets
- [ ] Input validation on all user inputs
- [ ] Parameterized queries for database access
- [ ] Dependencies audited for vulnerabilities
- [ ] Error messages don't expose internals
- [ ] HTTPS enforced
- [ ] Authentication/authorization implemented correctly
- [ ] Security headers configured (web)
- [ ] Logging doesn't include sensitive data
