---
name: inspect
description: Analyze and understand a repository's structure, purpose, and architecture. Use at conversation start or when switching context. Accepts optional focus area or external repo path.
arguments:
  - name: focus
    description: Optional context about what you'll be working on (e.g., "DNS server functionality", "authentication module")
    required: false
  - name: path
    description: Optional path to inspect (defaults to current repo, can point to external repos)
    required: false
invokable: true
---

# Repository Inspector

You are performing a systematic inspection of a codebase to build context for subsequent work.

## Execution Flow

### 1. Determine Scope

- If `path` argument provided, inspect that location
- Otherwise, inspect the current working directory
- If `focus` argument provided, prioritize areas relevant to that context

### 2. Quick Orientation (Always Do First)

Run these in parallel to get the lay of the land:

```
- List root directory structure
- Check for README, CONTRIBUTING, or docs/
- Identify package manager (package.json, Cargo.toml, pyproject.toml, go.mod, etc.)
- Look for configuration files (tsconfig, .env.example, docker-compose, etc.)
```

### 3. Understand Project Type & Architecture

Based on initial scan, identify:

- **Language/Framework**: What tech stack is this?
- **Project Type**: CLI, web app, library, monorepo, API, etc.
- **Entry Points**: Where does execution start? (main files, index files, bin/)
- **Build System**: How is it built/run? (scripts in package.json, Makefile, etc.)

### 4. Map the Structure

For each major directory, briefly note its purpose:

```
src/           → Main source code
  components/  → UI components
  services/    → Business logic
  utils/       → Helpers
tests/         → Test files
scripts/       → Build/dev scripts
```

### 5. Identify Key Files

Find and note the purpose of:

- Configuration files (what do they configure?)
- Environment variables (what's required?)
- Database schemas or migrations
- API routes or endpoints
- Core business logic files

### 6. If Focus Area Provided

When user specifies a focus (e.g., "DNS server"):

- Search for relevant files (grep for keywords)
- Identify the specific modules/directories involved
- Note dependencies between focused area and rest of codebase
- List the key files you'd need to modify for work in that area

### 7. Summarize Findings

Output a structured summary:

```markdown
## Project Overview
[One paragraph: what this project does]

## Tech Stack
- Language:
- Framework:
- Package Manager:
- Key Dependencies:

## Structure
[Directory map with purposes]

## Key Files
[List of important files with one-line descriptions]

## Development
- How to run:
- How to test:
- How to build:

## Focus Area (if provided)
[Specific files and modules relevant to the stated focus]

## Notes
[Anything unusual, important patterns, or gotchas discovered]
```

## Tips

- Don't read every file in detail - scan structure first, dive deep only where needed
- Use glob patterns to find files quickly
- Check git history for recently active areas if relevant
- Note any AGENTS.md, CLAUDE.md, or similar AI-specific documentation
- If the project has a monorepo structure, map the packages/workspaces
