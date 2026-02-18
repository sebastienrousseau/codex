# Contributing to Codex

Thank you for your interest in contributing to the ecosystem documentation!

## Types of Contributions

### 1. Documentation Updates
- Fix typos and grammatical errors
- Improve clarity and examples
- Add missing documentation

### 2. Architecture Decision Records (ADRs)
- Propose new architectural decisions
- Update existing ADRs with new information
- Mark superseded decisions

### 3. Coding Standards
- Propose new standards
- Update existing standards
- Add examples and clarifications

### 4. Integration Guides
- Document new integration patterns
- Update guides for new versions
- Add troubleshooting sections

## Contribution Process

### Step 1: Open an Issue

Before making changes, open an issue to discuss:

```markdown
## Proposal: [Brief Title]

### Type
[ ] Documentation update
[ ] New ADR
[ ] Standards update
[ ] Guide update

### Description
What change are you proposing?

### Rationale
Why is this change needed?

### Impact
Which projects/areas are affected?
```

### Step 2: Fork and Branch

```bash
# Fork the repository on GitHub, then:
git clone https://github.com/YOUR-USERNAME/codex.git
cd codex
git checkout -b docs/your-change-description
```

### Step 3: Make Changes

Follow these guidelines:

#### For Documentation
- Use clear, concise language
- Include code examples where applicable
- Follow existing formatting patterns
- Test all code examples

#### For ADRs
- Use the [ADR template](adrs/000-template.md)
- Number sequentially (next available number)
- Include all required sections
- Reference related ADRs

#### For Standards
- Explain the rationale
- Provide good and bad examples
- Consider impact on existing code
- Include migration guidance if needed

### Step 4: Commit

Use conventional commits:

```bash
# Documentation
git commit -m "docs: improve getting-started guide clarity"

# ADR
git commit -m "docs(adr): add ADR for logging standards"

# Standards
git commit -m "docs(standards): add async guidelines for Python"
```

### Step 5: Submit Pull Request

```bash
git push -u origin docs/your-change-description
gh pr create --title "docs: your change title" --body "Description of changes"
```

### PR Requirements

- [ ] Clear description of changes
- [ ] Related issue linked
- [ ] All sections complete (no TODOs)
- [ ] Code examples tested
- [ ] No spelling/grammar errors

## Review Process

### Reviewers Look For

1. **Accuracy** - Is the information correct?
2. **Clarity** - Is it easy to understand?
3. **Completeness** - Are all necessary details included?
4. **Consistency** - Does it match existing patterns?
5. **Impact** - How does this affect other projects?

### Response Time

- Initial review: Within 3 business days
- Follow-up reviews: Within 2 business days

### Approval Requirements

- At least 1 maintainer approval
- All comments addressed
- CI checks passing

## Writing Guidelines

### Tone

- Professional but approachable
- Direct and actionable
- Avoid jargon unless defined

### Structure

- Start with a summary
- Use headers for organization
- Include examples
- End with next steps or references

### Formatting

```markdown
# Main Title

Brief introduction paragraph.

## Section Header

Content with [links](url) and `inline code`.

### Subsection

- Bullet points for lists
- Each point is concise

```language
// Code blocks with language specified
```

| Tables | For | Structured Data |
|--------|-----|-----------------|
| Value  | Val | Data            |
```

## ADR Lifecycle

```
Proposed → Accepted → [Active]
                   ↓
              Deprecated
                   ↓
             Superseded by ADR-XXX
```

### Proposing an ADR

1. Create ADR with status "Proposed"
2. Open PR for discussion
3. Address feedback
4. Maintainer updates status to "Accepted"

### Deprecating an ADR

1. Open issue explaining why
2. Create PR updating status
3. Link to replacement ADR if applicable

## Standards Update Process

### Minor Updates
- Typos, clarifications, examples
- Direct PR without issue

### Major Updates
- New requirements, breaking changes
- Requires issue discussion first
- May need ADR for significant changes

## Questions?

- Open a GitHub issue
- Check existing issues and discussions
- Review related documentation

## Code of Conduct

Be respectful, constructive, and professional. We're all working toward better documentation and a stronger ecosystem.
