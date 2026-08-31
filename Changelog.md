# Changelog

All notable changes to `staff-engineer-skill` will be documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).  
This project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

Changes staged for the next release will appear here.

---

## [1.0.0] — 2026-09-01

### Added

- `skill.md` — full 26-section engineering operating standard covering:
  - Role definition and production-readiness bar
  - Non-negotiable engineering principles (hard rules, preferences, tradeoffs)
  - Repository reconnaissance workflow
  - Task understanding and problem decomposition protocol
  - Architecture and system design reasoning framework
  - API and interface design contracts
  - Code quality and maintainability rules
  - Error handling and failure engineering taxonomy
  - Testing strategy by scope and risk
  - Verification and evidence protocol with VERIFIED / PARTIALLY VERIFIED / NOT VERIFIED labeling
  - Evidence-driven debugging methodology
  - Measurement-first performance engineering
  - Concurrency reasoning framework
  - Distributed systems failure mode checklist
  - Database engineering and safe migration rules
  - Security engineering across trust boundaries
  - Observability and production engineering requirements
  - Production readiness checklist
  - Git and change discipline
  - Dependency management criteria
  - Refactoring discipline and justification rules
  - Minimal change principle
  - AI-specific guardrails (fabrication prevention, invention prevention, stop conditions)
  - Risk classification model (LOW / MEDIUM / HIGH / CRITICAL)
  - Definition of Done checklist
  - Completion report protocol
  - Engineering heuristics

- `README.md` — full usage guide covering:
  - Installation for Claude Projects, Cursor, GitHub Copilot, and generic system prompts
  - 9 annotated use cases with example prompts
  - Before/after behavior comparison table
  - Completion report format reference
  - Risk classification table
  - Contributing guidelines

---

## Versioning Policy

| Change Type | Version Bump | Example |
|---|---|---|
| New section or major addition | Minor — `1.x.0` | Adding an ML systems section |
| Correction, clarification, rewording | Patch — `1.0.x` | Fixing an ambiguous rule |
| Structural redesign or breaking reorganization | Major — `x.0.0` | Splitting into modular skill files |

---

[Unreleased]: https://github.com/yourusername/staff-engineer-skill/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/yourusername/staff-engineer-skill/releases/tag/v1.0.0
