# staff-engineer-skill

> A rigorous engineering operating standard that makes an AI coding agent behave like a Staff+ engineer — evidence-driven, production-conscious, and conservative with changes.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](CHANGELOG.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

---

## Table of Contents

- [What This Is](#what-this-is)
- [What This Is Not](#what-this-is-not)
- [Repository Structure](#repository-structure)
- [Installation](#installation)
- [Use Cases](#use-cases)
- [How the Agent Behaves Differently](#how-the-agent-behaves-differently)
- [Skill Structure](#skill-structure)
- [Completion Report](#completion-report)
- [Risk Classification](#risk-classification)
- [Versioning](#versioning)
- [Contributing](#contributing)
- [License](#license)

---

## What This Is

`staff-engineer-skill` is a persistent skill file for AI coding agents (Claude, Cursor, GitHub Copilot, and others). It installs a complete **engineering operating standard** into the agent, replacing generic "write good code" behavior with the disciplined habits of a Staff+ software engineer working on a real production system.

The gap this fills:

| Level | What most agents do | What this skill targets |
|---|---|---|
| Compiles | ✅ | ✅ |
| Passes narrow tests | ✅ | ✅ |
| Satisfies the happy path | ✅ | ✅ |
| Handles failure modes | ❌ | ✅ |
| Preserves existing contracts | ❌ | ✅ |
| Verifies with evidence | ❌ | ✅ |
| Classifies and communicates risk | ❌ | ✅ |
| Considers security and operability | ❌ | ✅ |

When this skill is active, the agent will:

- Discover and understand a codebase **before** touching it
- Reason about architecture, callers, and failure modes — not just syntax
- Apply the **smallest safe change** that solves the actual problem
- Verify behavior with **evidence**, not assumptions
- Classify risk and apply **proportional rigor**
- Report outcomes honestly, including **uncertainty and residual risk**

---

## What This Is Not

- A code generator prompt ("write me a REST API")
- A style guide or linting ruleset
- A language-specific or framework-specific template
- A beginner tutorial
- A list of buzzwords or aspirational principles

---

## Repository Structure

```
staff-engineer-skill/
├── skill.md              # Core engineering operating standard (install this)
├── README.md             # This file
├── CHANGELOG.md          # Version history
├── CONTRIBUTING.md       # Contribution guidelines
├── LICENSE               # MIT License
```

The only file you need to get started is **`skill.md`**. Everything else is supporting material.

---

## Installation

### Claude (claude.ai) — Recommended

1. Open or create a **Project** in Claude.
2. Go to **Project Instructions**.
3. Paste the full contents of `skill.md` into the instructions field, or upload the file directly.
4. Every conversation in that project will now use the skill automatically.

> **Tip:** Create one Claude Project per codebase or team. The skill works best when paired with repository context uploaded to the project.

---

### Cursor

1. Create `.cursor/rules/staff-engineer.mdc` in your repository root.
2. Copy the full contents of `skill.md` into that file.
3. Cursor applies the skill automatically when the agent is active in that repository.

```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/staff-engineer.mdc \
  https://raw.githubusercontent.com/yourusername/staff-engineer-skill/main/skill.md
```

---

### GitHub Copilot / Copilot Chat

1. Add `skill.md` to your repository as `.github/copilot-instructions.md`.
2. Copilot Chat picks it up automatically as workspace context.

```bash
curl -o .github/copilot-instructions.md \
  https://raw.githubusercontent.com/yourusername/staff-engineer-skill/main/skill.md
```

---

### Any Agent with a System Prompt

Paste `skill.md` as the system prompt or prepend it to your agent's existing instructions. The document is self-contained and requires no additional configuration.

---

### Language-Specific Appendices

If your codebase uses a specific language, pair the core skill with the matching appendix from the `appendices/` folder:

```
skill.md + appendices/go.md          # Go services
skill.md + appendices/python.md      # Python / Django / FastAPI
skill.md + appendices/typescript.md  # Node.js / React
```

---

## Use Cases

### 1. Feature Development

**Scenario:** Add a new endpoint, job, or capability to an existing service.

**What the skill does:**
- Discovers existing routing conventions, auth patterns, and error response shapes before writing code
- Identifies callers, data contracts, and backward-compatibility constraints
- Produces a minimal, reviewable implementation with error handling and observability
- Validates against existing tests before declaring done

**Example prompt:**
```
Add a POST /users/{id}/deactivate endpoint to the user service.
It should soft-delete the user and publish a UserDeactivated event.
```

The agent will inspect the repository structure, find existing endpoints, identify the event publishing pattern, check the test setup — then implement, rather than inventing conventions from scratch.

---

### 2. Bug Investigation and Debugging

**Scenario:** A production service is returning errors intermittently.

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

The agent will analyze logs, trace the call path, form a falsifiable hypothesis, apply a targeted fix, and write a test that would have caught the bug.

---

### 3. Code Review

**Scenario:** Review a pull request or diff before merging.

**What the skill does:**
- Reviews for correctness, security, error handling, and failure modes — not just style
- Checks for silent failures, missing validation, and uncovered edge cases
- Evaluates backward compatibility of interface changes
- Identifies blast radius and risk level
- Produces structured, actionable feedback

**Example prompt:**
```
Review this diff. Focus on correctness, error handling, and security.
[paste diff]
```

---

### 4. Refactoring

**Scenario:** A module has grown unwieldy and needs cleanup before new functionality is added.

**What the skill does:**
- Requires justification — refactoring proceeds only when it serves correctness, testability, reliability, or maintainability
- Separates refactoring commits from behavior-change commits
- Validates that existing tests pass before and after
- Prevents scope creep into unrelated modules

**Example prompt:**
```
The OrderProcessor class is 800 lines and hard to test.
Refactor it before we add the new discount calculation logic.
```

The agent will scope the refactoring explicitly, preserve all existing behavior, confirm tests pass, then proceed.

---

### 5. Architecture and System Design

**Scenario:** Design a new service or make a significant architectural decision.

**What the skill does:**
- Evaluates correctness, security, reliability, and simplicity in order
- Warns against premature distribution, premature abstraction, and unnecessary dependencies
- Requires explicit answers to: what can fail, how do we observe it, how do we roll it back
- Documents tradeoffs rather than asserting a single "right answer"

**Example prompt:**
```
We need to add async order processing. Orders currently go through
a synchronous HTTP call to the fulfillment service causing timeouts.
Design the new architecture.
```

---

### 6. Database Schema and Migrations

**Scenario:** Add a column, change an index, or migrate data in a live database.

**What the skill does:**
- Requires backward-compatible migration strategy
- Prohibits unbounded queries and blind indexing
- Requires query plan inspection before performance changes
- Forces explicit rollback plan for destructive operations
- Flags lock implications on high-traffic tables

**Example prompt:**
```
Add a `status` column to the payments table and backfill existing rows.
We have 50M rows in production.
```

---

### 7. Security Review

**Scenario:** A new feature crosses a trust boundary — user input, external API, multi-tenant data.

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
- Adds a benchmark to prevent future regression

**Example prompt:**
```
The /search endpoint has p99 latency of 4s. It should be under 500ms.
Here is the implementation: [paste code]
Here is the query it runs: [paste query]
```

---

### 9. New Service Setup

**Scenario:** Start a new service from scratch.

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
| Silently swallows errors | Makes every failure mode explicit |
| Rewrites large chunks of unrelated code | Minimizes blast radius |
| Hides uncertainty behind confident tone | Labels assumptions, unknowns, and risks |
| Optimizes speculatively | Measures first, optimizes the bottleneck |
| Skips security analysis | Reviews trust boundaries on every change |

---

## Skill Structure

`skill.md` is organized into 26 sections that form a coherent engineering operating system:

| Section | Coverage |
|---|---|
| Role Definition | What production-ready means; the engineering bar |
| Non-Negotiable Principles | Hard rules, preferences, context-dependent tradeoffs |
| Repository Reconnaissance | Discovery workflow before any modification |
| Task Decomposition | Converting a request into an engineering problem |
| Architecture and Design | Boundaries, dependencies, failure, operability |
| API and Interface Design | Contracts, idempotency, compatibility, resilience |
| Code Quality | Naming, complexity, state, error handling |
| Failure Engineering | Failure taxonomy, retry rules, silent failure prohibition |
| Testing Strategy | Scope selection, what to test, what not to test |
| Verification Protocol | Evidence requirements; VERIFIED / PARTIALLY VERIFIED / NOT VERIFIED |
| Debugging Methodology | Evidence-driven process; no random patching |
| Performance Engineering | Measure-first; baseline → bottleneck → optimize → benchmark |
| Concurrency | Race conditions, deadlocks, ownership, backpressure |
| Distributed Systems | Partial failure, retries, idempotency, consistency |
| Database Engineering | Schema, migrations, query plans, pooling |
| Security Engineering | Trust boundaries, injection, secrets, authorization |
| Observability | Metrics, logs, tracing, health checks, graceful shutdown |
| Production Readiness | Deployment, rollback, monitoring, runbooks |
| Git and Change Discipline | Minimal diffs, diff review, protecting user work |
| Dependency Management | Criteria for adding, auditing, minimizing dependencies |
| Refactoring Discipline | When to refactor; how to scope it safely |
| Minimal Change Principle | Smallest safe change without weakening guarantees |
| AI-Specific Rules | Fabrication prevention, invention prevention, stop conditions |
| Risk Classification | LOW / MEDIUM / HIGH / CRITICAL with proportional validation |
| Definition of Done | Proportional checklist by risk level |
| Completion Report Protocol | Structured evidence-based reporting format |

---

## Completion Report

After every non-trivial task, the agent produces a structured report. Every validation claim is backed by execution evidence — not stated from assumption.

```md
## Summary
What was done and why.

## Changes
Files modified and the nature of each change.

## Validation
Each verification step, the command used, and the outcome.
Labeled: VERIFIED / PARTIALLY VERIFIED / NOT VERIFIED.

## Tests
What tests were added. What scenarios remain uncovered and why.

## Risks / Limitations
Known risks, coverage gaps, compatibility concerns, rollback considerations.

## Remaining Uncertainty
What is still unknown. What additional investigation would reduce this uncertainty.
```

---

## Risk Classification

The agent self-classifies every change before beginning implementation and applies proportional validation:

| Level | Example | Required Validation |
|---|---|---|
| **LOW** | Fix a config default, rename a local variable | Targeted test + diff review |
| **MEDIUM** | Add a new API field, change business logic | Full test suite + integration check + diff review |
| **HIGH** | Data migration on live table, security boundary change | Rollback plan + phased rollout consideration + all of MEDIUM |
| **CRITICAL** | Auth change affecting all users, irreversible data operation | Independent review + staged deployment + all of HIGH |

---

## Versioning

This project follows [Semantic Versioning](https://semver.org/):

| Change | Bump |
|---|---|
| New section or major addition | Minor — `1.x.0` |
| Correction, clarification, rewording | Patch — `1.0.x` |
| Structural redesign or breaking reorganization | Major — `x.0.0` |

See [CHANGELOG.md](CHANGELOG.md) for the full version history.

---

## Contributing

Contributions are welcome from anyone who works with AI coding agents on real production systems.

Before opening a PR, read [CONTRIBUTING.md](CONTRIBUTING.md). The short version:

- One logical change per PR
- Every rule must be specific and actionable — no vague aspirations
- Update `CHANGELOG.md` under `[Unreleased]`
- Language-specific rules go in `appendices/`, not in `skill.md`
- Real session examples go in `examples/`

**Good contributions:**
- Close a gap in engineering coverage
- Replace a vague rule with a specific, observable one
- Fix a technical inaccuracy
- Add a language-specific appendix
- Add a real session example

**Not accepted:**
- Style preferences with no behavioral impact
- Rewrites of working sections
- Vague additions that don't change agent behavior
- AI-generated content not reviewed and validated by the submitter

---

## License

MIT — use freely in personal projects, team environments, and commercial AI agent configurations. See [LICENSE](LICENSE).

---

*Built to make AI coding agents accountable for production outcomes, not just code generation.*
