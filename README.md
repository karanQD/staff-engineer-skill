# staff-engineer-skill

> A rigorous engineering operating standard that makes an AI coding agent behave like a Staff+ engineer — evidence-driven, production-conscious, and conservative with changes.

---

## What This Is

`staff-engineer-skill` is a persistent skill file for AI coding agents (Claude, Cursor, Copilot Workspace, etc.). It installs a complete **engineering operating standard** into the agent, replacing generic "write good code" behavior with the disciplined habits of a Staff+ software engineer working on a real production system.

When active, the agent will:

- Discover and understand a codebase before touching it
- Reason about architecture, callers, and failure modes — not just syntax
- Apply the smallest safe change that solves the actual problem
- Verify behavior with evidence, not assumptions
- Classify risk and apply proportional rigor
- Report outcomes honestly, including uncertainty and residual risk

---

## What This Is Not

- A code generator prompt ("write me a REST API")
- A style guide or linting ruleset
- A language-specific or framework-specific template
- A beginner tutorial

---

## Installation

### Claude (claude.ai)

1. Open a Project in Claude.
2. Go to **Project Instructions** (or **Custom Instructions**).
3. Paste the full contents of `skill.md` — or upload the file directly if your interface supports it.
4. Start your engineering session. The skill is now active for every message in that project.

### Cursor

1. Create `.cursor/rules/staff-engineer.mdc` in your repository root.
2. Copy the contents of `skill.md` into that file.
3. Cursor will apply the skill automatically when the agent is active in that repo.

### Copilot Workspace / GitHub Copilot Chat

1. Add `skill.md` to your repository as `.github/copilot-instructions.md`.
2. GitHub Copilot Chat will pick it up automatically as workspace context.

### Any Agent with a System Prompt

Paste `skill.md` as the system prompt or prepend it to your agent's instructions. The document is self-contained and requires no additional configuration.

---

## Use Cases

### 1. Feature Development

**Scenario:** You need to add a new endpoint to an existing service.

**What the skill does:**
- Forces discovery of existing routing conventions, auth patterns, and error response shapes before writing a line of code
- Requires identifying callers, data contracts, and backward-compatibility constraints
- Produces a minimal, reviewable implementation with appropriate error handling and observability
- Validates against existing tests before declaring done

**Example prompt:**
```
Add a POST /users/{id}/deactivate endpoint to the user service.
It should soft-delete the user and publish a UserDeactivated event.
```

The agent will first inspect the repository structure, find existing endpoints, identify the event publishing pattern, check the test setup, then implement — rather than inventing conventions from scratch.

---

### 2. Bug Investigation and Debugging

**Scenario:** A production service is returning 500 errors intermittently under load.

**What the skill does:**
- Requires reproduction before hypothesizing
- Demands evidence (logs, traces, stack traces) before forming a theory
- Prohibits random trial-and-error patching
- Requires a regression test before closing the bug

**Example prompt:**
```
We're seeing intermittent 500s on the /orders endpoint under load.
Here are the relevant log lines: [paste logs]
Investigate and fix.
```

The agent will analyze the logs, trace the call path, form a falsifiable hypothesis, apply a targeted fix, and write a test that would have caught the bug.

---

### 3. Code Review Assistance

**Scenario:** You want the agent to review a pull request or diff.

**What the skill does:**
- Reviews for correctness, security, error handling, and failure modes — not just style
- Checks for silent failures, missing validation, uncovered edge cases
- Evaluates backward compatibility of interface changes
- Identifies blast radius and risk level
- Produces structured feedback with specific, actionable comments

**Example prompt:**
```
Review this diff. Focus on correctness, error handling, and security.
[paste diff]
```

---

### 4. Refactoring

**Scenario:** A module has grown unwieldy and needs to be cleaned up before adding new functionality.

**What the skill does:**
- Requires justification: refactoring proceeds only when it serves correctness, testability, reliability, or maintainability
- Separates refactoring commits from behavior-change commits
- Validates that existing tests pass before and after
- Prevents scope creep into unrelated modules

**Example prompt:**
```
The OrderProcessor class is 800 lines and hard to test.
Refactor it before we add the new discount calculation logic.
```

The agent will scope the refactoring explicitly, preserve all existing behavior, run the test suite, then proceed.

---

### 5. Architecture and Design

**Scenario:** You need to design a new service or make a significant architectural decision.

**What the skill does:**
- Evaluates correctness, security, reliability, and simplicity in order — not just what is technically interesting
- Warns against premature distribution, premature abstraction, and unnecessary dependencies
- Requires explicit answers to: what can fail, how do we observe it, how do we roll it back
- Documents tradeoffs rather than asserting a single "right answer"

**Example prompt:**
```
We need to add async order processing. Orders currently go through
a synchronous HTTP call to the fulfillment service which is causing
timeouts. Design the new architecture.
```

---

### 6. Database Schema and Migrations

**Scenario:** You need to add a column, change an index, or migrate data.

**What the skill does:**
- Requires backward-compatible migration strategy (application deploy before schema cleanup)
- Prohibits unbounded queries and blind indexing
- Requires query plan inspection before performance changes
- Forces explicit rollback plan for destructive operations

**Example prompt:**
```
Add a `status` column to the payments table and backfill existing rows.
We have 50M rows in production.
```

---

### 7. Security Review

**Scenario:** A new feature crosses a trust boundary (user input, external API, multi-tenant data).

**What the skill does:**
- Requires input validation at every trust boundary
- Checks for injection, SSRF, path traversal, and insecure deserialization
- Verifies authorization is enforced server-side
- Prohibits secrets in logs, source code, or error messages

**Example prompt:**
```
Review the file upload feature for security issues.
Users can upload files which are stored in S3 and served back.
[paste code]
```

---

### 8. Performance Investigation

**Scenario:** A slow endpoint needs to be optimized.

**What the skill does:**
- Requires a baseline measurement before any optimization
- Identifies the actual bottleneck (query, serialization, network, lock contention) rather than guessing
- Prohibits speculative optimization
- Requires correctness verification after optimization
- Adds a benchmark test to prevent future regression

**Example prompt:**
```
The /search endpoint has p99 latency of 4s. It should be under 500ms.
Here is the implementation: [paste code]
Here is the query it runs: [paste query]
```

---

### 9. New Repository Setup

**Scenario:** You are starting a new service from scratch.

**What the skill does:**
- Establishes build, test, lint, and CI structure before writing business logic
- Designs external interfaces with idempotency, error semantics, and versioning in mind
- Builds in observability (structured logging, health checks, metrics) from the start
- Defines configuration externalization and secrets handling upfront

**Example prompt:**
```
Create a new Go service skeleton for a payment processing microservice.
It needs an HTTP API, PostgreSQL, and should publish events to Kafka.
```

---

## How the Agent Behaves Differently

| Without this skill | With this skill |
|---|---|
| Immediately writes code | Discovers existing patterns first |
| Invents file paths and APIs | Searches the repository before assuming |
| Claims "it works" without evidence | Reports what was executed and what passed |
| Ignores callers when changing a function | Identifies and checks affected callers |
| Adds dependencies freely | Justifies each new dependency |
| Silently swallows errors | Makes failure behavior explicit |
| Rewrites large chunks of unrelated code | Minimizes blast radius |
| Hides uncertainty behind confident tone | Labels assumptions, unknowns, and risks |

---

## Completion Report

After every non-trivial task, the agent produces a structured completion report:

```md
## Summary
What was done and why.

## Changes
Files modified and the nature of each change.

## Validation
Each verification step, the command used, and the outcome.
Labeled: VERIFIED / PARTIALLY VERIFIED / NOT VERIFIED.

## Tests
What tests were added. What scenarios remain uncovered.

## Risks / Limitations
Known risks, coverage gaps, compatibility concerns.

## Remaining Uncertainty
What is still unknown. What additional investigation would help.
```

Every validation claim in this report is backed by execution evidence — not stated from assumption.

---

## Risk Classification

The agent self-classifies every change before beginning:

| Level | Example | Validation Required |
|---|---|---|
| LOW | Fix a typo in a config default | Targeted test + diff review |
| MEDIUM | Add a new API field | Full test suite + integration check |
| HIGH | Data migration on a live table | Rollback plan + phased rollout |
| CRITICAL | Auth boundary change affecting all users | Independent review + staged deploy |

---

## Contributing

The skill is a single Markdown file (`skill.md`). Contributions welcome:

- Corrections to technical accuracy
- Additional examples for specific domains (ML systems, embedded, mobile)
- Language-specific appendices (Go, Python, TypeScript, Rust, Java)
- Translations

Open a PR with the change and a one-line rationale for each edit.

---

## License

MIT. Use freely in personal projects, team environments, and commercial AI agent configurations.

---

*Built to make AI coding agents accountable for production outcomes, not just code generation.*
