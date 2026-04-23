---
name: agents-md
description: Author or refine the AGENTS.md file at a repository root so coding agents (Claude Code, Cursor, Codex, Copilot, etc.) produce measurably better output. Use this skill whenever the user asks to write, create, update, or bootstrap AGENTS.md, CLAUDE.md, .cursorrules, .github/copilot-instructions.md, or any "agent rules / agent instructions / agent docs" file for the repo. Also use it when the user says things like "tell the agent how this codebase works", "onboard Claude to this repo", "set up coding agent conventions", or "what should we put in AGENTS.md" — even if they don't name the file directly.
invokable: true
---

# AGENTS.md Writer

You are authoring (or refining) an `AGENTS.md` at the root of the current repository. AGENTS.md is the one piece of documentation that coding agents load automatically, on every invocation, before they know what task they're doing. Every sentence you put in it is paid for in context on every future run. A good one measurably improves agent output; a bad one measurably degrades it. Treat this as a high-leverage writing task, not a dumping ground.

## The two failure modes to design against

1. **Overexploration.** Long architecture overviews and big reference sections pull the agent into reading dozens of files before it touches the task. On complex feature work this shows up as dropped completeness and unnecessary abstractions.
2. **Warning bloat.** A wall of "don'ts" without matching "dos" makes the agent cautious and verification-heavy. It reads each rule, decides whether it applies, and starts checking code that isn't on the task's path.

If your draft trips either of these, cut it. Shorter and more specific always wins.

## Workflow

Don't write the file until step 5. The survey is where the value comes from — not the template.

### 1. Survey the repo

In parallel:

- List the repo root.
- Read existing `README.md`, `CONTRIBUTING.md`, and any existing `AGENTS.md` / `CLAUDE.md` / `.cursorrules` / `.github/copilot-instructions.md`.
- Identify the package/build manifest (`package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `Gemfile`, `pom.xml`, etc.).
- Look for `Makefile`, `justfile`, `tsconfig.json`, lint/format config, CI workflow files.
- Detect monorepo shape (`workspaces`, `packages/`, `apps/`, `crates/`, `services/`).

### 2. Extract the non-obvious essentials

These are what an agent cannot easily guess. Pull them verbatim from real files, not from memory:

- **Setup** — the exact command(s) to get to a working local state.
- **Run / dev** — how to start the app.
- **Test** — the test command, and how to run a *single* test file.
- **Lint / format / typecheck** — the checks that must pass before a change is done.
- **Build** — only if it's part of the verification loop.

If these live in `package.json` scripts, `Makefile`, or `justfile`, cite them verbatim.

### 3. Find the real decision points

The highest-leverage thing an AGENTS.md can do is resolve ambiguity *before* the agent writes code. Look for places in the codebase where a new feature could reasonably be implemented more than one way:

- Two libraries that do overlapping jobs (React Query + Zustand, Redux + Context, Zod + Yup).
- ORM vs raw SQL.
- Sync vs async variants of the same helper.
- Two logging frameworks both actively used.
- Different testing styles between directories.

Each genuine ambiguity is a candidate for a decision table. If you find none, don't invent one.

### 4. Collect 2–4 real examples

Find short (3–10 line) snippets of real production code that teach the house style. Good targets: a typical handler/route, a typical test, a typical component. Copy verbatim with the file path. Don't paraphrase — the agent pattern-matches better on real code than on abstractions. Stop at 4; more and the agent starts pattern-matching on the wrong thing.

### 5. Draft the file

Target **100–150 lines total**. Research on real codebases finds this is where gains peak; beyond it, performance reverses. If content exceeds the budget, move it into scoped reference files (see Progressive disclosure, below) rather than inflating the main file.

Use this section order. Skip any section that has nothing genuine to say — empty sections are worse than missing ones.

- **Project one-liner** — one sentence on what this is. Skip for private repos where the team already knows.
- **Setup** — install command, env vars.
- **Commands** — run, test (including single-test), lint, typecheck, build.
- **Code layout** — 3–8 lines max, only non-obvious directories.
- **Conventions** — 5–10 tight rules. Every "don't" paired with a "do".
- **Patterns** — decision tables *only where real ambiguity exists*.
- **Example** — 1–2 verbatim snippets from real files, with paths.
- **Workflows** — numbered steps *only for multi-step tasks where agents currently miss wiring*.
- **References** — at most ~10, each with a one-line "read this when…" trigger.

### 6. Sanity check before writing

Walk through this list. If any answer is "no", fix it before saving.

- Under 150 lines?
- Every command works when copy-pasted?
- No architecture essay, history section, or philosophy?
- Every "don't" paired with a concrete "do"?
- Decision tables only where >1 real pattern exists in the code?
- Examples copied from real files, with paths?
- ≤10 outbound references, each with a scoped trigger?
- Nothing duplicated from the README that a human reads?
- Nothing describing an aspirational pattern that isn't in the code yet?

### 7. Write and report

Write `AGENTS.md` at the repo root. If `CLAUDE.md`, `.cursorrules`, or similar already exist, don't silently overwrite — ask the user whether to consolidate, keep both, or symlink. AGENTS.md is a superset most tools now read, so consolidation is usually the right answer.

Report back with: final line count; which sections you included and why; any decision tables or workflows added with a one-sentence rationale; any existing doc you chose *not* to reference, and why.

## Style rules for the content

**Pair every "don't" with a "do".** A bare prohibition makes the agent cautious and exploratory. A paired rule tells it what to do and moves on.

- Weak: *Don't instantiate HTTP clients directly.*
- Strong: *Use the shared `apiClient` from `src/http` (it handles retries and auth). Don't instantiate `fetch` or `axios` directly.*

**Decision tables over prose.** When two patterns compete, a table forces the agent to choose before writing code.

| Situation                      | Use             | Not                    |
|--------------------------------|-----------------|------------------------|
| Server state (from API)        | React Query     | Zustand / Redux        |
| Client-only UI state           | Zustand         | React Query / Context  |

**Numbered workflows for multi-step tasks.** If agents keep missing wiring for a recurring task (new route, new migration, new integration), write the steps as an ordered list. This pattern has the largest measured effect on completeness.

**Explain the *why* when it isn't obvious.** "Use X" lands better as "Use X because Y" — the agent can then generalize to edge cases instead of following the rule blindly.

**No all-caps MUSTs unless genuinely load-bearing.** They cost more than they buy. Save them for the one or two rules that would cause real damage if violated.

## Progressive disclosure

If you have detail that's important but doesn't belong in every invocation, push it into a referenced file and link with a scoped trigger:

- `.agents/migrations.md` — read before writing a DB migration.
- `.agents/deploy.md` — read before modifying CI or infra config.

References get loaded on demand, so they're nearly free if scoped well and nearly lethal if not. Keep the total under ~10. Every reference you add is a tax on every task that *isn't* that topic.

## Anti-patterns — don't ship these

| Anti-pattern                                          | Do this instead                                       |
|-------------------------------------------------------|-------------------------------------------------------|
| "Architecture Overview" with history and rationale    | Cut it. Agents don't need it to ship a fix.           |
| 30+ assorted gotchas and warnings                     | Keep only rules that are specific *and* enforceable.  |
| Warning-only list ("Don't X. Don't Y. Don't Z.")      | Pair each with the alternative.                       |
| Long reference list pointing into `_docs/*`           | ≤10 references, each with a "read this when…" trigger. |
| Describing a pattern that doesn't exist yet           | Wait until it's in the code. Otherwise the doc lies.  |
| Verbatim duplication of the README                    | Trim hard — cut anything a human skims.               |
| Restating facts visible from `ls` or `package.json`   | Cut. Agents read those already.                       |

## Edge cases

**Monorepo.** Keep the root AGENTS.md high-level (shared commands, workspace layout, pointers). Recommend a per-package/app AGENTS.md for the real conventions — module-level docs outperform one giant root-level file because they describe isolated surface area. Tell the user this is the plan; don't unilaterally create per-package files.

**Existing CLAUDE.md / .cursorrules / copilot-instructions.md.** AGENTS.md is a superset most tools now support. Offer to consolidate: move content into `AGENTS.md`, leave the old file as a short pointer or symlink.

**The repo has a big doc sprawl** (many `.md` files, a sizeable `docs/`). Flag it. A lean AGENTS.md won't save the agent if the surrounding specs keep getting read via grep and search. Suggest referencing the 2–3 that matter and either pruning the rest or making them clearly scoped.

**The user wants to "document everything".** Push back. Every line costs context on every future invocation. Ask what tasks agents have been getting wrong recently — those are what the file should address. Don't write it as an insurance policy against hypothetical future failures.

**No established conventions.** Don't invent them. Document what's actually in the code. If there's genuine inconsistency between places, ask the user which pattern to canonize — and note in the file that this is now a choice, not a description.

## Updating an existing AGENTS.md

If `AGENTS.md` already exists:

1. Read it fully.
2. Before editing, ask the user: *"What's been going wrong in agent output that this update should fix?"* — the answer drives what changes.
3. Flag sections that are now outdated, duplicative with the README, warning-only, or architecture essay. Propose cuts.
4. Prefer surgical edits to a full rewrite. If the existing file is already lean and accurate, say so and stop.
