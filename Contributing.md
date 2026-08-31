# Contributing to staff-engineer-skill

Thank you for taking the time to improve this skill. Contributions are welcome from anyone who works with AI coding agents on real production systems.

This document explains how to propose changes, what makes a good contribution, and how the review process works.

---

## What We Are Optimizing For

Every change to `skill.md` should make an AI coding agent behave more like a disciplined Staff+ engineer — not more verbose, not more cautious in a way that blocks useful work, and not more opinionated about style preferences.

A good contribution makes agent behavior:

- More correct (closes a gap in reasoning or coverage)
- More specific (replaces vague guidance with actionable rules)
- More accurate (fixes a technical error)
- More usable (improves clarity without losing precision)

A contribution that adds words without changing behavior is not a good contribution.

---

## Types of Contributions

### Bug Reports — Skill Produces Wrong Behavior

If the skill causes an agent to behave incorrectly — for example, it follows a rule that produces a bad outcome, misses a failure mode, or gives conflicting guidance — open an issue describing:

1. What task or prompt you gave the agent
2. What the agent did
3. What you expected it to do instead
4. Which section of `skill.md` you believe is responsible

Include the agent and interface you used (Claude Projects, Cursor, Copilot, etc.) if relevant.

### Corrections and Clarifications

If a rule is technically inaccurate, ambiguous, or contradicts another rule, open a PR with:

- The specific line(s) changed
- A one-sentence rationale for each change in the PR description
- Version bump: **patch** (`1.0.x`)

### New Sections or Domain Extensions

If an entire engineering domain is missing or underrepresented (examples: ML systems, mobile, embedded, frontend, infrastructure-as-code), open an issue first to discuss scope before writing.

New sections should follow the existing style:

- Strong imperative language (`DO`, `DO NOT`, `REQUIRE`, `PREFER`, `AVOID`)
- Specific, observable rules — not vague aspirations
- No repetition of content already covered in another section
- Version bump: **minor** (`1.x.0`)

### Language-Specific Appendices

The core `skill.md` is intentionally language-agnostic. Language-specific guidance belongs in separate files under `appendices/`:

```
appendices/
  go.md
  python.md
  typescript.md
  rust.md
  java.md
```

If your contribution is language-specific, add it there rather than modifying the core skill. Open a PR with the new file.

### Examples

Real session transcripts showing the agent using the skill on a concrete task are among the most valuable contributions. Add them under `examples/`:

```
examples/
  feature-development.md
  bug-investigation.md
  code-review.md
  database-migration.md
```

Format: prompt → agent response (summarized or full) → outcome. Scrub any proprietary code or business context before submitting.

### README Improvements

Corrections to installation instructions, new agent integrations, or additional use cases are welcome. Follow the same PR process as other changes.

---

## How to Submit a Pull Request

1. **Fork** the repository and create a branch from `main`.

   ```bash
   git checkout -b fix/concurrency-deadlock-rule
   ```

2. **Make your change.** Keep it focused. One logical change per PR.

3. **Update `CHANGELOG.md`** under `[Unreleased]` with a brief description of your change.

4. **Write a clear PR description** covering:
   - What you changed and where
   - Why the change improves agent behavior
   - What the agent does differently after the change (if testable)

5. **Open the PR** against `main`.

---

## What Makes a Good PR

### The rule is specific and actionable

Bad:
```
Handle concurrency carefully.
```

Good:
```
Before writing concurrent code, explicitly name the shared state,
its owner, and the happens-before relationship the synchronization
mechanism provides.
```

### The rule does not duplicate existing content

Search `skill.md` before adding a new rule. If a related concept exists, extend that section rather than adding a parallel one.

### The change does not add length without adding value

The skill must remain usable by an agent actively working through a repository. Every sentence must earn its place. If a sentence could be removed without losing a distinct behavioral instruction, remove it.

### The rationale is documented

In your PR description, explain concretely: what does an agent do differently because of this change? If you cannot answer that question, the change may be too abstract.

---

## What We Will Not Merge

- Style or formatting preferences with no behavioral impact
- Rules that apply only to a specific framework or tool (put these in an appendix)
- Vague additions ("be more careful about security")
- Rewrites of sections that are working correctly
- Changes that contradict the Minimal Change Principle without strong justification
- AI-generated contributions that have not been reviewed and validated by the submitter

---

## Review Process

All PRs are reviewed against the following criteria:

1. Does the change produce a measurably better agent behavior?
2. Is it consistent with the rest of the skill?
3. Does it avoid duplicating existing content?
4. Is the CHANGELOG updated?
5. Is the PR description clear and specific?

Review turnaround target: **5 business days** for small changes, longer for new sections.

---

## Style Guide for `skill.md`

Follow these conventions when writing or editing content:

**Voice:** Strong imperative. Use `DO`, `DO NOT`, `REQUIRE`, `PREFER`, `AVOID`, `VERIFY`, `STOP`.

**Specificity:** Every rule must describe observable behavior, a decision criterion, or a verification step. No vague aspirations.

**Tables:** Use for comparisons, taxonomies, and checklists. Keep columns narrow.

**Code blocks:** Use for workflows, sequences, and shell commands. Use `text` language tag for non-code sequences.

**No repetition:** If a concept is covered in one section, reference it rather than restating it elsewhere.

**No filler:** Every sentence must add a distinct behavioral instruction or clarification. If it could be cut without losing meaning, cut it.

---

## Questions

Open an issue with the label `question` for anything not covered here.

---

*The goal is a skill that makes AI coding agents genuinely more reliable on production systems — not longer, not more cautious, not more impressive-sounding. Specific and useful beats comprehensive and vague every time.*
