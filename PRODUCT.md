# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Stack

Delegated: C++17 product core with a browser UI and project-owned API, deployed to an Azure demo or sandbox through GitHub Actions. GitHub Agentic Workflows with Copilot coding agent provides the single bounded remediation agent.

## Users

- Workshop participants and customer engineers use a familiar to-do app while evaluating self-healing CI.
- Workshop facilitators guide scenario runs and explain the control flow.
- Repository maintainers adapt the template and preserve its deterministic controls.

## Product Purpose

Demonstrate how one bounded remediation agent can repair unit-test, coverage, and performance failures while deterministic classification, policy, validation, and human review retain authority. Success means each scenario terminates in under 10 minutes with either a reviewable pull request or fail-closed escalation evidence.

## Positioning

The product is a working to-do app and a transparent reference pipeline in one repository: the product stays familiar while every remediation boundary and outcome remains inspectable.

## Operating Context

Users manage public demo to-dos in a shared Azure sandbox. Workshop users manually dispatch one scenario in GitHub Actions, inspect classification and policy evidence, observe one remediation attempt, then review a pull request or escalation issue.

## Capabilities and Constraints

- Create, list, edit, complete, reopen, and delete to-dos.
- Public demo data only; secrets, PII, customer-confidential, regulated, and production data are prohibited.
- One active run per scenario and one remediation proposal per run.
- No auto-merge, direct push to `main`, branch-protection bypass, disabled tests, or reduced thresholds.
- UI feedback begins within 100 ms and meets WCAG 2.2 AA.
- API p95 remains below 500 ms at 20 concurrent users.
- The UI is a regular task application, not a workflow-control dashboard.

## Evidence on Hand

- Approved TWTTY seed, specification, and discovery transcript under `twtty/`.
- Three approved deterministic scenario definitions and a 30-item acceptance-evidence matrix.
- No existing product UI, visual identity, logo, customer claim, testimonial, or production benchmark may be fabricated.

## Product Principles

- Familiar product behavior keeps attention on the remediation pattern.
- Deterministic controls own every decision that does not require reasoning.
- Fail closed when evidence, confidence, policy, or platform capability is insufficient.
- Human review is the only merge authority.
- Every claim is backed by inspectable workflow evidence.

## Accessibility & Inclusion

WCAG 2.2 AA, full keyboard operation, accessible names and landmarks, visible focus, live status announcements, and automated accessibility checks are required.
