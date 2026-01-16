---
name: pr
description: Commit changes with conventional commits and create a draft PR following repo standards. Checks CONTRIBUTING.md, PR templates, existing PRs, runs linters, and writes human-sounding descriptions.
arguments:
  - name: title
    description: Optional PR title override
    required: false
invokable: true
---

# Create Pull Request

You are preparing and submitting code changes as a proper contribution. This means committing with good hygiene, following repo standards, and creating a draft PR that feels human-written.

## Core Principles

1. **Commit First, Then PR** - Get commits right, then worry about PR metadata
2. **Follow Repo Standards** - Every repo has its own way; match it
3. **Human Voice** - Clear, concise, no AI fluff or excessive formality
4. **Draft Mode** - Always create as draft; let the user promote when ready
5. **Ask When Uncertain** - If something's off, stop and discuss

## Execution Flow

### 1. Pre-Flight Checks

Before doing anything, verify:

```bash
git status  # What's changed?
git diff    # What are we committing?
```

**Stop and ask the user if:**
- Working directory is clean (nothing to commit)
- There are unrelated changes mixed in
- You're on main/master branch (should be on feature branch)

### 2. Create Commits

**Commit Granularity**

Split into multiple commits when changes are logically separate:
- One commit per feature/fix/refactor
- Separate test commits if substantial
- Keep each commit atomic and buildable

**Conventional Commits Format**

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `style`: Formatting, no code change
- `refactor`: Code change that neither fixes nor adds
- `test`: Adding/updating tests
- `chore`: Maintenance, deps, config

Examples:
```
feat(auth): add OAuth2 login flow
fix(api): handle null response from user endpoint
docs(readme): add installation instructions
refactor(utils): extract date formatting helpers
```

**Commit Message Style**
- Lowercase after type
- No period at end
- Imperative mood ("add" not "added")
- Under 72 chars for subject
- Body explains *why*, not *what*

### 3. Check for Lint/Format Requirements

Look for and run project tooling:

```bash
# Check for common patterns
cat package.json        # scripts: lint, format, check
cat Makefile           # lint, fmt, check targets
cat pyproject.toml     # ruff, black, etc.
```

Common commands to look for:
- `npm run lint` / `npm run format`
- `make lint` / `make fmt`
- `bun run check`
- `cargo fmt && cargo clippy`
- `ruff check --fix && ruff format`

**Run these before committing.** If they modify files, include those in your commits.

### 4. Research Repo Standards

Now that commits are ready, gather PR context:

**Check for Contributing Guide**
```
- CONTRIBUTING.md
- docs/CONTRIBUTING.md
- .github/CONTRIBUTING.md
```

**Check for PR Template**
```
- .github/PULL_REQUEST_TEMPLATE.md
- .github/PULL_REQUEST_TEMPLATE/
- docs/pull_request_template.md
```

**Review Existing PRs**
```bash
gh pr list --limit 10  # See recent PR titles and style
gh pr view <number>    # Look at a good example PR
```

Note:
- How are PR titles formatted?
- What sections do descriptions have?
- How detailed are they?
- What's the tone/voice?

**Check for Related Issues**
```bash
gh issue list --limit 10
gh issue view <number>  # If this PR closes an issue
```

**Check for Duplicate PRs**
```bash
gh pr list --state open  # Is there already a PR for this?
```

**Stop and tell the user if:**
- A PR already exists for the same thing
- An open issue conflicts with this change
- Contributing guide has requirements you can't meet

### 5. Push the Branch

```bash
# Create branch if needed
git checkout -b <branch-name>  # e.g., feat/oauth-login, fix/null-response

# Push with upstream tracking
git push -u origin <branch-name>
```

Branch naming:
- `feat/<description>` for features
- `fix/<description>` for fixes
- `docs/<description>` for docs
- `refactor/<description>` for refactors

### 6. Create Draft PR

**Title**
- Match repo's existing PR title style
- Often mirrors the main commit message
- Clear and specific

**Description**

Write like a human, not a robot. Avoid:
- "This PR implements..."
- "The following changes were made..."
- Bullet lists of every single change
- Overly formal language
- Emojis (unless repo uses them)

Instead, write naturally:
- What does this change do and why?
- Any context the reviewer needs?
- How to test it?
- Link related issues with "Closes #123" or "Fixes #456"

**If PR Template Exists**

Follow it exactly. Fill in each section. Don't delete sections - write "N/A" if not applicable.

**If No Template**

Use a simple structure:

```markdown
## Summary

[2-3 sentences: what this does and why]

## Changes

[Brief description of approach, not a file-by-file list]

## Test Plan

[How to verify this works]

---
Closes #<issue> (if applicable)
```

**Create the PR**

```bash
gh pr create --draft --title "..." --body "..."
```

Always `--draft`. User promotes when ready.

### 7. Final Report

```markdown
## PR Created

**Branch**: `feat/your-feature`
**PR**: #123 (link)
**Status**: Draft

### Commits
- `abc1234` feat(scope): description
- `def5678` test(scope): add tests for feature

### Followed
- [x] Conventional commits
- [x] Ran linter/formatter
- [x] PR template used
- [x] Draft mode

### Notes
[Any concerns, things to mention, or follow-ups needed]
```

## When to Stop and Ask

**Don't push forward if:**

- You're unsure which branch to target (main? develop? release?)
- The contributing guide has requirements you haven't met
- There's already a PR or issue for this
- You don't have push access / auth fails
- The changes seem incomplete
- Tests are failing
- Lint is failing and you can't fix it

**Just tell the user.** It's better to ask than to create a messy PR.

## Writing Style Guide

The PR description should sound like you wrote it:

**Don't write:**
> This pull request introduces a new authentication module that leverages OAuth2 protocol to enable secure user authentication. The implementation includes comprehensive error handling and follows industry best practices.

**Do write:**
> Adds OAuth2 login so users can sign in with Google/GitHub. Handles token refresh and logout. Tested against the staging OAuth app.

Keep it:
- Direct
- Specific
- Lowercase (unless starting sentence)
- No buzzwords
- No "comprehensive" or "robust" or "leverage"
