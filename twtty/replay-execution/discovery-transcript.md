# Discovery Transcript — Baseline

## Upfront mode selections

**Spec Agent:** Which Discovery Mode should govern the four-round requirements process?

**Human User:** Interactive — I interview you with targeted questions each round.

**Recorded choice:** Discovery mode = `Interactive`.

**Spec Agent:** Which UX Mode applies to this scope?

**Human User:** Delegated — let the AI define custom UI requirements.

**Recorded choice:** UX mode = `Delegated`.

**Spec Agent:** Which API Mode applies to this scope?

**Human User:** Delegated — let the AI define API contract requirements.

**Recorded choice:** API mode = `Delegated`.

## Round 1 — Surface

**Spec Agent:** Who is the primary user whose successful experience should drive the repository, workflow, dashboard, and documentation?

**Human User:** Workshop participant or customer engineer evaluating the pattern (Recommended).

**Spec Agent:** Which secondary personas must the baseline explicitly support?

**Human User:** Both workshop facilitators and repository maintainers (Recommended).

**Spec Agent:** What should be the primary end-to-end user journey?

**Human User:** Select one scenario, trigger its failure, observe classification and bounded remediation, then review the resulting PR or escalation evidence (Recommended).

**Spec Agent:** Which top-level success signal should govern the baseline demonstration?

**Human User:** Each scenario completes independently in under 10 minutes and a participant can explain the control flow after one guided run (Recommended).

**Spec Agent:** Should the custom UI be scoped as a read-only demonstration dashboard that visualizes workflow status and evidence, without directly approving merges or changing policies?

**Human User:** no, jjust make it a regular app

**Spec Agent:** What should users do in the regular app UI?

**Human User:** it sould be a sample app like a to do app

**Spec Agent:** Should the sample app keep the seed's engineering-job-results domain but use a simple to-do-style list interface, or replace the domain entirely with a to-do app?

**Human User:** to do app as the single domain but containing all the agentic workflows that we talked about and the purpose to show the customer how agentic workflows and self healing remediation pipelines work

### Scope refinement during Round 1

The last answer materially changed the approved project seed. Discovery paused while the Human User approved a SEED refinement that replaced the engineering-job-results sample with a single-domain to-do application while preserving the bounded self-healing architecture and three scenarios. Corrective `SEED-EXIT` entry `003` records the approved result.

**Spec Agent:** Should the SPEC stage adopt this recommended standards set: OpenAPI 3.1, WCAG 2.2 AA, OWASP ASVS and API Security Top 10, SLSA provenance, SPDX or CycloneDX SBOM, and OpenTelemetry-compatible traces?

**Human User:** Adopt the recommended set (Recommended).

### Approved Round 1 content

- Primary persona: workshop participant or customer engineer evaluating the pattern.
- Secondary personas: workshop facilitator and repository maintainer.
- Product: regular, single-domain to-do web application with a C++17 core and project-owned API.
- Primary product use case: create, view, complete, and delete to-do items.
- Primary demonstration journey: select one failure scenario, trigger it, observe deterministic classification and policy gating, permit one bounded remediation attempt, then review a human-approval-required pull request or escalation issue.
- Success signal: each scenario completes independently in under 10 minutes, and a participant can explain the control flow after one guided run.
- Adopted specification standards: OpenAPI 3.1, WCAG 2.2 AA, OWASP ASVS and API Security Top 10, SLSA provenance, SPDX or CycloneDX SBOM, and OpenTelemetry-compatible tracing.
- Boundary: future enterprise integrations remain extension points only.

**Spec Agent:** Approve Round 1 Surface content and proceed to Round 2 Deep-dive?

**Human User:** Approved — proceed (Recommended).

## Round 2 — Deep-dive

**Human User:** lets do everyting else and other rounds in autopilot

**Recorded execution-mode request:** Enable Autopilot for the remainder of the active baseline buildout, including the remaining discovery rounds, SPEC, PLAN, and EXECUTE. Activation is pending the required Human-User-authored version-control anchor.

The required Human-User-authored commit was not created after two verification attempts, so Autopilot never activated.

**Spec Agent:** Choose how to proceed: personally create the required authorization commit, or continue with explicit approvals in Interactive mode.

**Human User:** Continue in Interactive mode.
