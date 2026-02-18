# Codex

**The Architecture Documentation Hub for the Sebastien Rousseau Ecosystem**

Codex serves as the single source of truth for architectural decisions, coding standards, integration patterns, and cross-project documentation across the entire ecosystem.

## Overview

This repository centralizes documentation for the following ecosystem projects:

| Repository | Purpose | Language |
|------------|---------|----------|
| [commons](https://github.com/sebastienrousseau/commons) | Shared utilities library | Rust |
| [pipelines](https://github.com/sebastienrousseau/pipelines) | CI/CD templates | YAML/Shell |
| [devkit](https://github.com/sebastienrousseau/devkit) | Developer tooling | Shell |
| [pulse](https://github.com/sebastienrousseau/pulse) | Ecosystem monitoring | Python |

## Documentation Structure

```
codex/
├── adrs/                    # Architecture Decision Records
│   ├── 000-template.md      # ADR template
│   ├── 001-codex-structure.md
│   └── ...
├── standards/               # Coding Standards
│   ├── rust.md              # Rust coding guidelines
│   ├── python.md            # Python coding guidelines
│   ├── javascript.md        # JavaScript coding guidelines
│   └── shell.md             # Shell scripting guidelines
├── guides/                  # Integration Guides
│   ├── getting-started.md   # Ecosystem onboarding
│   ├── cross-project.md     # Cross-project integration
│   ├── cicd-usage.md        # Using pipelines templates
│   └── monitoring.md        # Setting up pulse monitoring
├── diagrams/                # Architecture Diagrams
│   └── ecosystem-overview.md
├── CONTRIBUTING.md          # Contribution guidelines
└── README.md                # This file
```

## Quick Links

### Architecture Decision Records
- [ADR Template](adrs/000-template.md) - How to write ADRs
- [ADR-001: Codex Structure](adrs/001-codex-structure.md) - Why this structure

### Coding Standards
- [Rust Standards](standards/rust.md) - Rust coding conventions
- [Python Standards](standards/python.md) - Python coding conventions
- [JavaScript Standards](standards/javascript.md) - JavaScript coding conventions
- [Shell Standards](standards/shell.md) - Shell scripting conventions

### Integration Guides
- [Getting Started](guides/getting-started.md) - Ecosystem onboarding
- [Cross-Project Integration](guides/cross-project.md) - How projects work together
- [CI/CD Usage](guides/cicd-usage.md) - Using pipelines templates
- [Monitoring Setup](guides/monitoring.md) - Setting up pulse

## Core Principles

### 1. Consistency
All ecosystem projects follow the same conventions, making it easy to move between codebases.

### 2. Quality First
Every project implements comprehensive testing, linting, and security scanning.

### 3. Documentation Driven
Major decisions are documented in ADRs before implementation.

### 4. Automation
CI/CD pipelines enforce standards automatically.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on:
- Submitting documentation updates
- Proposing new ADRs
- Updating coding standards

## License

Dual-licensed under [MIT](LICENSE-MIT) and [Apache-2.0](LICENSE-APACHE).
