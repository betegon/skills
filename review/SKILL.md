---
name: review
description: Review recent code changes for correctness, bugs, edge cases, and goal alignment. Asks "what would happen if..." questions. Does not refactor - use /cleanup for that.
arguments:
  - name: scope
    description: Optional scope - "last" for last change, "session" for all session changes, or specific file paths
    required: false
invokable: true
---

# Code Review

You are reviewing recent code changes for correctness and completeness. Your job is to find bugs, missed edge cases, and verify the code achieves its intended goal. This is NOT about style or refactoring - use `/cleanup` for that.

## Review Mindset

Think like:
- A skeptical reviewer who's seen production incidents
- A user who will try weird inputs
- A future maintainer debugging at 2am
- An attacker looking for exploits

## Execution Flow

### 1. Identify What to Review

```
- If scope="last": Review the most recent change
- If scope="session": Review all changes this conversation
- If scope=<path>: Review specified files
- Default: Review recent conversation changes
```

### 2. Understand the Goal

Before reviewing, be crystal clear on:

- What is this code supposed to do?
- What problem does it solve?
- What are the requirements/acceptance criteria?
- What did the user ask for?

You cannot evaluate correctness without knowing the goal.

### 3. Goal Alignment Check

Ask yourself:

- [ ] Does this code actually achieve what was requested?
- [ ] Are there requirements that weren't implemented?
- [ ] Does it do anything beyond what was asked? (scope creep)
- [ ] Would the user be satisfied with this result?

### 4. Bug Hunt

Look for common bugs:

**Logic Errors**
- Off-by-one errors (< vs <=, array bounds)
- Wrong boolean logic (AND vs OR, negation errors)
- Incorrect operator (= vs ==, + vs -)
- Wrong variable used (copy-paste errors)
- Unreachable code or dead branches

**Null/Undefined**
- Accessing properties on potentially null values
- Missing null checks before operations
- Optional chaining needed but not used
- Default values missing

**Async Issues**
- Missing await
- Race conditions
- Unhandled promise rejections
- Callback not called in all paths

**State Issues**
- Mutating shared state unexpectedly
- Stale closures capturing old values
- State not reset when it should be
- Initialization order problems

**Type Issues**
- Type coercion surprises (== vs ===)
- Assuming array when could be single item
- String when expecting number
- Incorrect type assertions hiding errors

### 5. Edge Case Analysis

For each input/parameter, ask "What happens if...":

**Boundary Values**
- Empty string, empty array, empty object?
- Zero, negative numbers, very large numbers?
- null, undefined, NaN?
- Single item vs multiple items?
- Maximum length/size?

**Unexpected Input**
- Wrong type passed in?
- Malformed data?
- Unicode, special characters, emoji?
- Very long strings?
- Deeply nested objects?

**Timing & State**
- Called twice in rapid succession?
- Called before initialization complete?
- Called after cleanup/dispose?
- Network timeout or failure?
- Concurrent modifications?

**Environment**
- Different OS behavior?
- Missing environment variables?
- File/directory doesn't exist?
- No network connectivity?
- Insufficient permissions?

### 6. Security Review

Check for:

- [ ] User input used unsanitized (XSS, injection)
- [ ] Secrets/credentials exposed or logged
- [ ] Path traversal vulnerabilities
- [ ] Missing authentication/authorization checks
- [ ] Sensitive data in error messages
- [ ] Insecure defaults

### 7. Error Handling Review

- [ ] Are errors caught where they should be?
- [ ] Are errors propagated correctly?
- [ ] Are error messages helpful for debugging?
- [ ] Is there proper cleanup in error paths?
- [ ] Can the system recover gracefully?

### 8. Integration Points

Where this code touches other systems:

- [ ] Are API contracts respected?
- [ ] Are database queries correct?
- [ ] Are external service failures handled?
- [ ] Are retries/timeouts appropriate?
- [ ] Is backwards compatibility maintained?

### 9. Report Findings

Structure your review:

```markdown
## Review Summary

### Goal Alignment
[Does it achieve what was requested? Yes/No/Partially, with explanation]

### Bugs Found
1. **[Severity: High/Medium/Low]** Description
   - Location: `file.ts:123`
   - Issue: What's wrong
   - Impact: What could go wrong
   - Suggested fix: How to fix it

### Edge Cases to Handle
1. **What if [scenario]?**
   - Current behavior: [what happens now]
   - Expected behavior: [what should happen]
   - Location: `file.ts:45`

### Security Concerns
[Any security issues found, or "None identified"]

### Questions for Clarification
[Anything unclear about requirements or expected behavior]

### Verdict
- [ ] Ready to ship
- [ ] Minor issues (list them)
- [ ] Needs changes before shipping (list blockers)
```

## What This Review is NOT

- **Not a style review** - Use `/cleanup` for formatting, naming, DRY
- **Not a refactoring session** - We're checking correctness, not improving structure
- **Not nitpicking** - Focus on things that matter, not preferences

## Severity Guide

- **High**: Will cause crashes, data loss, security issues, or core feature broken
- **Medium**: Bug that affects some users/cases, degraded experience
- **Low**: Minor issue, unlikely to occur, minimal impact
