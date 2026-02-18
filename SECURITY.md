# Security Policy

## Reporting a Vulnerability

We take security vulnerabilities seriously. If you discover a security issue, please report it responsibly.

### How to Report

**Do NOT open a public GitHub issue for security vulnerabilities.**

Instead, please report security vulnerabilities by emailing:

**security@sebastienrousseau.com**

Include the following information:
- Description of the vulnerability
- Steps to reproduce
- Affected versions
- Potential impact
- Any suggested fixes (optional)

### What to Expect

1. **Acknowledgment**: We will acknowledge receipt within 48 hours
2. **Assessment**: We will assess the vulnerability within 7 days
3. **Updates**: We will provide status updates every 7 days
4. **Resolution**: We aim to resolve critical issues within 30 days
5. **Disclosure**: We will coordinate public disclosure with you

### Scope

This security policy applies to all repositories in the sebastienrousseau ecosystem.

#### In Scope

- Code vulnerabilities
- Dependency vulnerabilities
- Authentication/authorization issues
- Data exposure risks
- Injection vulnerabilities
- Cryptographic issues

#### Out of Scope

- Social engineering attacks
- Physical security
- Denial of service attacks
- Issues in dependencies we don't control
- Issues requiring physical access

## Security Practices

### Dependencies

- All dependencies are audited regularly
- Dependabot alerts are enabled on all repositories
- Security patches are applied promptly

### Code Review

- All changes require review before merging
- Security-sensitive changes require additional review
- Automated security scanning in CI

### Secrets Management

- No secrets in code repositories
- Environment variables for configuration
- Secrets scanning enabled

## Supported Versions

| Version | Supported          |
|---------|--------------------|
| Latest  | :white_check_mark: |
| < 1 year old | :white_check_mark: |
| > 1 year old | :x: |

## Security Updates

Security updates are released as:

- **Critical**: Immediate patch release
- **High**: Within 7 days
- **Medium**: Next scheduled release
- **Low**: Best effort

## Acknowledgments

We appreciate security researchers who responsibly disclose vulnerabilities. Contributors will be acknowledged in release notes (unless they prefer to remain anonymous).

## Resources

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE/SANS Top 25](https://cwe.mitre.org/top25/)
- [Rust Security Guidelines](https://rustsec.org/)
- [Python Security Best Practices](https://python.org/dev/security/)
