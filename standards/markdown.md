# Markdown Standards

Standards for Markdown documentation across the ecosystem.

## File Naming

- Use lowercase with hyphens: `getting-started.md`
- README files: `README.md` (uppercase)
- Changelog: `CHANGELOG.md`
- License: `LICENSE-MIT`, `LICENSE-APACHE`

## Document Structure

```markdown
# Document Title

Brief introduction paragraph explaining the document's purpose.

## Table of Contents

- [Section 1](#section-1)
- [Section 2](#section-2)
- [Section 3](#section-3)

## Section 1

Content here...

### Subsection 1.1

More content...

## Section 2

Content here...
```

## Headings

- One H1 (`#`) per document (the title)
- Don't skip heading levels
- Use sentence case: "Getting started" not "Getting Started"
- No trailing punctuation

```markdown
# Document title

## Main section

### Subsection

#### Sub-subsection
```

## Text Formatting

```markdown
**Bold text** for emphasis
*Italic text* for slight emphasis
`inline code` for code, commands, file names
~~Strikethrough~~ for deprecated content
```

## Lists

### Unordered Lists

```markdown
- Item one
- Item two
  - Nested item
  - Another nested item
- Item three
```

### Ordered Lists

```markdown
1. First step
2. Second step
3. Third step
   1. Sub-step
   2. Another sub-step
```

### Task Lists

```markdown
- [x] Completed task
- [ ] Incomplete task
- [ ] Another task
```

## Code Blocks

### Inline Code

```markdown
Run `cargo test` to execute tests.
```

### Fenced Code Blocks

Always specify the language:

````markdown
```rust
fn main() {
    println!("Hello, world!");
}
```

```python
def main():
    print("Hello, world!")
```

```bash
cargo build --release
```
````

### Shell Commands

```markdown
```bash
# Install dependencies
npm install

# Run tests
npm test
```
```

Show expected output when helpful:

```markdown
```bash
$ cargo --version
cargo 1.75.0
```
```

## Links

### External Links

```markdown
See the [Rust documentation](https://doc.rust-lang.org/) for details.
```

### Internal Links

```markdown
See the [contributing guide](./CONTRIBUTING.md) for details.
See the [testing section](#testing) below.
```

### Reference Links

```markdown
This project uses [Rust][rust-lang] and [Python][python].

[rust-lang]: https://www.rust-lang.org/
[python]: https://www.python.org/
```

## Images

```markdown
![Alt text describing the image](./images/screenshot.png)

<!-- With title -->
![Architecture diagram](./diagrams/architecture.png "System architecture")
```

## Tables

```markdown
| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Data 1   | Data 2   | Data 3   |
| Data 4   | Data 5   | Data 6   |

<!-- With alignment -->
| Left     | Center   | Right    |
|:---------|:--------:|---------:|
| Aligned  | Aligned  | Aligned  |
```

## Blockquotes

```markdown
> This is a blockquote.
> It can span multiple lines.

> **Note:** Important information here.

> **Warning:** Be careful about this.
```

## Admonitions (GitHub-flavored)

```markdown
> [!NOTE]
> Useful information that users should know.

> [!TIP]
> Helpful advice for doing things better.

> [!IMPORTANT]
> Key information users need to know.

> [!WARNING]
> Urgent info that needs immediate attention.

> [!CAUTION]
> Advises about risks or negative outcomes.
```

## Line Length

- Wrap prose at 80-100 characters
- Don't wrap code blocks
- Don't wrap tables
- Don't wrap links (break before if needed)

## Whitespace

- One blank line between sections
- One blank line before and after code blocks
- One blank line before and after lists
- No trailing whitespace
- Single newline at end of file

## README Structure

Standard README sections:

```markdown
# Project Name

Brief description of the project.

## Features

- Feature 1
- Feature 2
- Feature 3

## Installation

```bash
cargo add project-name
```

## Usage

```rust
use project_name::Thing;

let thing = Thing::new();
```

## Configuration

Optional configuration details.

## API Reference

Link to docs or brief API overview.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

Dual-licensed under [MIT](LICENSE-MIT) and [Apache-2.0](LICENSE-APACHE).
```

## CHANGELOG Format

Follow [Keep a Changelog](https://keepachangelog.com/):

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- New feature

## [1.0.0] - 2024-01-01

### Added
- Initial release

[Unreleased]: https://github.com/user/repo/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/user/repo/releases/tag/v1.0.0
```

## Linting

Use markdownlint:

```json
// .markdownlint.json
{
    "MD013": false,
    "MD033": false,
    "MD041": false
}
```

Common rules:
- MD001: Heading levels increment by one
- MD003: Heading style (ATX)
- MD009: No trailing spaces
- MD012: No multiple blank lines
- MD022: Headings surrounded by blank lines
- MD032: Lists surrounded by blank lines
