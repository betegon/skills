# Skills

Custom skills for Claude Code.

## Available Skills

| Skill | Description |
|-------|-------------|
| `/inspect` | Analyze repo structure and build context for a conversation |
| `/review` | Check code for bugs, edge cases, and goal alignment |
| `/cleanup` | Refactor code to follow best practices and clean up docs |
| `/pr` | Commit with conventional commits and create a draft PR |

## Installation

Copy skills to your global Claude config:

```bash
cp -r */  ~/.claude/skills/
```

Or symlink for easier updates:

```bash
ln -s $(pwd)/* ~/.claude/skills/
```

## Usage

Once installed, invoke skills by name:

```
/inspect focus="authentication module"
/review last
/cleanup
/pr
```
