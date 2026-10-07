# Replay-Execution Log — Baseline

Approved prompts and outcomes for the unnumbered baseline buildout.

### 001 · meta/risk-level · —

- **Timestamp:** 2026-10-07T01:59:57Z
- **Approval outcome:** Approved
- **Approved prompt:**

  ```text
  Assess the project against the active SDLC and agentic-app risk ladders. Confirm L2 — internal workflow, low blast radius — because the repository demonstrates bounded remediation through human-reviewed pull requests, excludes auto-merge and production operation, and remains intended for workshops, demonstrations, and proofs of concept.
  ```

- **Execution outcome:** L2
- **Artifact / path changed:** `twtty/replay-execution/execution-log.md`
- **Notes:** L2 applies baseline controls including SAST, dependency scanning, code review, CI checks, branch protection, SBOM generation, and append-only replay-log enforcement. The agentic overlay requires a representative evaluation harness, a minimum stochastic run count of N=3, and a declared pass rule.

### 002 · seed/0a · SEED-EXIT

- **Timestamp:** 2026-10-07T02:01:24Z
- **Approval outcome:** Approved
- **Approved prompt:**

  ```text
  Goal: Prepare the baseline seed and repository for SEED-EXIT.
  Scope: Preserve the seed's substantive intent; normalize its headings into the required "What I Want To Build" and "Done Looks Like" sections; add the approved engineering standards; create a minimal C++/CMake .gitignore; initialize main; create a public GitHub repository named demo-agentic-workflow-sample under the authenticated personal account; configure origin; commit and push the seed bootstrap.
  Inputs: Existing project seed, confirmed sdlc-for-agentic-apps specialization, confirmed L2 risk level, and adopted standards.
  Expected output: A pushed GitHub repository with the canonical seed at twtty/seed/seed.md, a minimal .gitignore, and no product implementation.
  Acceptance criteria: Required seed sections exist and are non-empty; no template placeholders remain; the repository is on main; origin is configured; the bootstrap commit is pushed; no secret or identifiable data is written to TWTTY replay artifacts.
  Tools and integrations: Local file editing, local git, and authenticated GitHub CLI for account verification and public repository creation.
  ```

- **Execution outcome:** Baseline project seed approved; repository initialized and pushed
- **Artifact / path changed:** `twtty/seed/seed.md`
- **Artifact / path changed:** `twtty/replay-execution/state.md`
- **Artifact / path changed:** `twtty/replay-execution/execution-diagram.md`
- **Notes:** Self-review verified each SEED-EXIT condition against the artifacts: (1) the seed exists at `twtty/seed/seed.md`; (2) both required H2 sections are present and non-empty; (3) no template placeholder remains; (4) the repository uses `main`, has a configured remote, and the seed commit is pushed; (5) entry 001 records the confirmed L2 risk level; and (6) the Human User approved the result. The seed was fixed before the gate by normalizing heading levels and adding the approved engineering standards. Precision, scope alignment, deliverable coverage, and consistency with the approved intent were also reviewed.

### 003 · seed/0a · SEED-EXIT

- **Timestamp:** 2026-10-07T02:06:56Z
- **Approval outcome:** Approved
- **Approved prompt:**

  ```text
  Goal: Replace only the sample application domain with a regular single-domain to-do app while preserving all approved self-healing architecture, scenarios, restrictions, runtime targets, and human-approval controls.
  Scope: Add a lightweight browser UI and project-owned API; model to-do creation, completion, duplicate detection, and summaries; map the three defects to deterministic unit-test, coverage, and performance failures in those functions.
  Unchanged: C++17 core, one remediation agent, one attempt, fail-closed escalation, deterministic verification, PR-only outcome, no auto-merge, under-10-minute target, and future extension points.
  Expected output: Revised twtty/seed/seed.md, append-only corrective SEED-EXIT replay entry, refreshed state and diagram, committed and pushed.
  Acceptance criteria: Required seed headings and remote conditions continue to pass; old engineering-job domain terms are absent; the three scenarios remain independently runnable and retain their restrictions.
  Tools: Local file editing and local git; no new external service.
  ```

- **Execution outcome:** Project seed refined to a single-domain to-do sample application
- **Artifact / path changed:** `twtty/seed/seed.md`
- **Artifact / path changed:** `twtty/replay-execution/state.md`
- **Artifact / path changed:** `twtty/replay-execution/execution-diagram.md`
- **Notes:** Corrects the scope recorded by entry 002 after Human User refinement during Discovery. Self-review verified each SEED-EXIT condition against the revised artifact: canonical path and non-empty content; both required H2 sections; no template placeholders; configured remote with the revised seed commit pushed; L2 calibration remains entry 001; and explicit Human User approval. The old engineering-job terms were mechanically checked as absent. The bounded remediation architecture, three scenario restrictions, runtime objective, and human-approval controls remain consistent.
