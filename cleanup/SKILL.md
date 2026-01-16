---
name: cleanup
description: Refactor and clean up recent code changes to follow best engineering practices. Applies DRY, separation of concerns, proper documentation. Maintains all features while improving code quality.
arguments:
  - name: scope
    description: Optional scope - "last" for last change, "session" for all session changes, or specific file paths
    required: false
invokable: true
---

# Code Cleanup & Deslop

You are performing a quality pass on recent code changes to ensure they follow best engineering practices while maintaining all functionality.

## Core Principles

1. **Preserve All Features** - Never remove or break functionality
2. **Match Project Patterns** - Follow existing conventions, don't impose new ones
3. **Minimal Viable Change** - Only refactor what needs it; if it's already clean, say so
4. **Document Behavior, Not Implementation** - JSDoc describes *what* and *why*, not *how*

## Execution Flow

### 1. Identify What to Clean

Determine scope of cleanup:

```
- If scope="last": Look at the most recent change/edit
- If scope="session": Review all changes made this conversation
- If scope=<path>: Focus on specified files
- Default: Review recent conversation changes
```

Use git diff or conversation context to identify modified files.

### 2. Understand the Goal

Before refactoring, understand:

- What was the user trying to accomplish?
- What problem were we solving?
- What constraints exist?

This context prevents "improving" code in ways that break intent.

### 3. Gather Project Standards

Quickly scan for:

```
- AGENTS.md, CLAUDE.md, CONTRIBUTING.md (coding guidelines)
- ESLint/Prettier/Biome config (style rules)
- tsconfig.json (strictness level)
- Existing patterns in similar files
- Test conventions
```

### 4. Analyze the Changes

For each modified file, check:

**Structure & Separation of Concerns**
- [ ] Is business logic mixed with UI/IO?
- [ ] Are there god functions doing too much?
- [ ] Should anything be extracted to a separate module?
- [ ] Are responsibilities clear and single-purpose?

**DRY (Don't Repeat Yourself)**
- [ ] Is there duplicated logic that should be extracted?
- [ ] Are there repeated patterns that need a helper?
- [ ] But also: Is abstraction premature? (Rule of 3)

**Maintainability**
- [ ] Would a new contributor understand this?
- [ ] Are names descriptive and consistent?
- [ ] Is the control flow easy to follow?
- [ ] Are edge cases handled clearly?

**Documentation**
- [ ] Do functions have JSDoc with @param, @returns, @throws?
- [ ] Does JSDoc describe behavior and purpose, not implementation?
- [ ] Are inline comments explaining *why*, not *what*?
- [ ] Remove obvious/redundant inline comments

**Code Smells**
- [ ] Magic numbers/strings that need constants?
- [ ] Deep nesting that needs early returns?
- [ ] Long parameter lists that need an options object?
- [ ] Implicit dependencies that should be explicit?

### 5. Evaluate: Is Cleanup Needed?

Rate the current state:

- **Already Clean**: Changes follow patterns, well-documented, DRY → Report "No cleanup needed"
- **Minor Polish**: Small improvements possible → List them, ask if user wants them
- **Needs Work**: Clear issues to address → Proceed with cleanup

### 6. Perform Cleanup

If cleanup is warranted:

**JSDoc Guidelines**
```typescript
/**
 * Brief description of what this does and why.
 *
 * @param name - Description of the parameter's purpose
 * @returns Description of what's returned and when
 * @throws {ErrorType} When this error occurs
 *
 * @example
 * ```ts
 * const result = myFunction('input');
 * ```
 */
```

**Comment Guidelines**
- Remove: `// increment i` or `// call the API`
- Keep: `// We retry 3 times because the upstream service is flaky`
- Move inline explanation to JSDoc when possible

**Refactoring Priorities**
1. Extract repeated logic into well-named helpers
2. Split large functions into focused pieces
3. Improve naming for clarity
4. Add/fix JSDoc on public APIs
5. Remove dead code and debug statements
6. Ensure consistent formatting with project tools

### 7. Verify No Breakage

After cleanup:

- Run existing tests if available
- Verify the original goal still works
- Check imports and exports are correct
- Ensure no TypeScript/lint errors introduced

### 8. Report Changes

Summarize what was cleaned:

```markdown
## Cleanup Summary

### Changes Made
- Extracted `parseConfig()` from `initialize()` for reusability
- Added JSDoc to exported functions
- Removed redundant inline comments
- Renamed `x` to `connectionTimeout` for clarity

### Patterns Followed
- Matched existing error handling style in src/utils/
- Used project's Result<T> pattern for error returns

### Verified
- All tests pass
- No type errors
- Original feature works as intended
```

## When NOT to Clean

- User explicitly wants quick/dirty solution
- Experimental/throwaway code
- The code is already following project patterns well
- Changes would be purely cosmetic with no readability gain
