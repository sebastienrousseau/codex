# Code Review Guidelines

Standards for reviewing code in the ecosystem.

## Review Goals

1. **Catch bugs** before they reach production
2. **Ensure quality** meets ecosystem standards
3. **Share knowledge** across the team
4. **Maintain consistency** in codebase

## Reviewer Responsibilities

### Before Reviewing

- Understand the context (read PR description, linked issues)
- Check CI status
- Review the diff size (request split if too large)

### During Review

Focus on:

1. **Correctness** - Does the code do what it's supposed to?
2. **Security** - Are there any security issues?
3. **Performance** - Any obvious performance problems?
4. **Maintainability** - Is the code readable and maintainable?
5. **Testing** - Are tests adequate and correct?
6. **Documentation** - Is documentation updated?

### Providing Feedback

#### Be Constructive

```markdown
# Good
Consider using `filter_map` here for cleaner code:
`items.filter_map(|x| x.value()).collect()`

# Avoid
This code is ugly.
```

#### Be Specific

```markdown
# Good
This function could panic on line 42 if `input` is empty.
Consider handling the empty case explicitly.

# Avoid
This might break.
```

#### Use Prefixes

```markdown
# Required change
MUST: Add error handling for the None case

# Suggestion (optional)
NIT: Consider renaming `x` to `count` for clarity

# Question
Q: Is this intentionally different from the other implementation?

# Praise
NICE: Great use of the builder pattern here!
```

#### Explain Why

```markdown
# Good
Consider using `&str` instead of `String` here.
This avoids an allocation since we only need to read the value.

# Avoid
Use `&str` instead.
```

## Author Responsibilities

### Before Requesting Review

- [ ] Self-review the diff
- [ ] Run tests locally
- [ ] Update documentation
- [ ] Write clear PR description
- [ ] Link related issues
- [ ] Keep PR focused and reasonably sized

### PR Description Template

```markdown
## Summary

Brief description of changes.

## Changes

- Added X
- Fixed Y
- Updated Z

## Testing

How was this tested?

## Checklist

- [ ] Tests pass
- [ ] Documentation updated
- [ ] No breaking changes (or documented)
```

### Responding to Feedback

- Respond to all comments
- Ask for clarification if needed
- Explain your reasoning when disagreeing
- Mark resolved when addressed
- Request re-review after changes

## What to Look For

### Rust Specific

```rust
// Error handling
// Check: Are all errors handled appropriately?
// Check: Is ? used where appropriate?
// Check: Are error messages helpful?

// Memory
// Check: Any unnecessary clones?
// Check: Appropriate use of references?

// Safety
// Check: Any unsafe code justified?
// Check: Panic paths documented?

// Concurrency
// Check: Proper synchronization?
// Check: Potential deadlocks?
```

### Python Specific

```python
# Type hints
# Check: Are type hints present and correct?
# Check: Complex types documented?

# Error handling
# Check: Exceptions handled appropriately?
# Check: Custom exceptions where needed?

# Style
# Check: Follows PEP 8?
# Check: Docstrings present?
```

### JavaScript/TypeScript Specific

```typescript
// Types
// Check: Are types correct and specific?
// Check: Avoiding `any` where possible?

// Async
// Check: Proper error handling in async code?
// Check: No unhandled promises?

// Security
// Check: User input sanitized?
// Check: No XSS vulnerabilities?
```

### General Checks

| Category | Questions |
|----------|-----------|
| Logic | Does the code do what it claims? Edge cases handled? |
| Security | Input validation? SQL injection? XSS? Secrets exposed? |
| Performance | Obvious inefficiencies? N+1 queries? Memory leaks? |
| Tests | Tests present? Cover edge cases? Assertions meaningful? |
| Docs | README updated? API docs correct? Comments helpful? |
| Style | Consistent with codebase? Follows standards? |

## Review Checklist

### Critical (Must Address)

- [ ] No security vulnerabilities
- [ ] No data loss risks
- [ ] No breaking changes (unless intended)
- [ ] Tests pass and are meaningful
- [ ] No obvious bugs

### Important (Should Address)

- [ ] Error handling is appropriate
- [ ] Performance is acceptable
- [ ] Code is readable
- [ ] Documentation is updated
- [ ] Naming is clear

### Nice to Have (Consider)

- [ ] Code could be simplified
- [ ] Additional tests would help
- [ ] Comments could be clearer
- [ ] Minor style improvements

## Handling Disagreements

1. **Discuss** - Explain your perspective
2. **Understand** - Listen to the other view
3. **Compromise** - Find middle ground
4. **Escalate** - If needed, involve a third party
5. **Document** - Record the decision

## Approval Guidelines

### Approve When

- All critical issues addressed
- Code meets quality standards
- Tests are adequate
- You're confident in the changes

### Request Changes When

- Security issues present
- Breaking changes undocumented
- Tests missing or failing
- Major bugs identified

### Comment Only When

- Minor suggestions only
- Questions that don't block merge
- Praise for good work

## Time Expectations

| PR Size | Review Time |
|---------|-------------|
| Small (<100 lines) | Same day |
| Medium (100-500 lines) | 1-2 days |
| Large (>500 lines) | Consider splitting |

## Tools

- **GitHub Reviews** - Primary review tool
- **Reviewable** - For complex reviews
- **CI Checks** - Automated verification

## Anti-Patterns

### Avoid These

- **Nitpicking** - Focusing on trivial issues
- **Rubber stamping** - Approving without reading
- **Scope creep** - Requesting unrelated changes
- **Blocking** - Holding up PRs unnecessarily
- **Ego** - Making it personal

### Watch For These

- PRs that are too large
- Insufficient testing
- Unclear commit messages
- Missing documentation
- Copy-pasted code
