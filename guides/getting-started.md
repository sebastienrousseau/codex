# Getting Started with the Ecosystem

This guide helps you get started as a contributor to the Sebastien Rousseau ecosystem.

## Prerequisites

Ensure you have the following installed:

### Required Tools

| Tool | Version | Installation |
|------|---------|--------------|
| Git | 2.40+ | [git-scm.com](https://git-scm.com) |
| Rust | 1.75+ | `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \| sh` |
| Python | 3.11+ | [python.org](https://python.org) or pyenv |
| Node.js | 18+ | [nodejs.org](https://nodejs.org) or nvm |
| Poetry | 1.7+ | `pipx install poetry` |
| pnpm | 8+ | `npm install -g pnpm` |

### Optional Tools

- **Docker** - For containerized development
- **GitHub CLI** (`gh`) - For repository management
- **direnv** - For automatic environment setup

## Repository Setup

### 1. Clone the Ecosystem

```bash
# Create ecosystem directory
mkdir -p ~/Code/ecosystem
cd ~/Code/ecosystem

# Clone core repositories
gh repo clone sebastienrousseau/codex
gh repo clone sebastienrousseau/commons
gh repo clone sebastienrousseau/pipelines
gh repo clone sebastienrousseau/devkit
gh repo clone sebastienrousseau/pulse
```

### 2. Development Environment

Use **devkit** to set up your environment:

```bash
cd devkit
./scripts/setup.sh
```

This will:
- Install required dependencies
- Configure git hooks
- Set up editor configurations
- Create development environment

### 3. Verify Installation

```bash
# Rust
rustc --version
cargo --version

# Python
python --version
poetry --version

# Node.js
node --version
pnpm --version
```

## Project Workflow

### Starting a New Feature

```bash
# 1. Create feature branch
git checkout -b feat/your-feature

# 2. Make changes
# ... edit files ...

# 3. Run tests locally
cargo test           # Rust
pytest               # Python
pnpm test            # JavaScript

# 4. Commit with conventional commits
git commit -m "feat: add new feature"

# 5. Push and create PR
git push -u origin feat/your-feature
gh pr create
```

### Commit Message Format

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

Types:
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation
- `style` - Formatting
- `refactor` - Code restructuring
- `test` - Adding tests
- `chore` - Maintenance

## Understanding the Ecosystem

### Project Relationships

```
┌─────────────────────────────────────────────────────┐
│                      codex                           │
│            (documentation & standards)               │
└─────────────────────────────────────────────────────┘
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
┌───────────────┐ ┌───────────────┐ ┌───────────────┐
│   commons     │ │   pipelines   │ │    devkit     │
│ (Rust utils)  │ │  (CI/CD)      │ │   (tools)     │
└───────────────┘ └───────────────┘ └───────────────┘
        │                │                │
        └────────────────┼────────────────┘
                         ▼
                 ┌───────────────┐
                 │     pulse     │
                 │ (monitoring)  │
                 └───────────────┘
```

### How Projects Interact

1. **codex** defines standards all projects follow
2. **commons** provides shared Rust utilities
3. **pipelines** provides CI/CD templates used by all projects
4. **devkit** provides development environment tools
5. **pulse** monitors health across all repositories

## Next Steps

1. Read the [Coding Standards](../standards/) for your language
2. Review [Architecture Decision Records](../adrs/)
3. Check [Integration Guides](./cross-project.md)
4. Read [CONTRIBUTING.md](../CONTRIBUTING.md)

## Getting Help

- Open an issue in the relevant repository
- Check existing documentation in codex
- Review closed issues for similar problems
