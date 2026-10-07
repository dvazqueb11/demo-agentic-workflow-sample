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

**Spec Agent:** Where should the completed demo application run?

**Human User:** Azure demo or sandbox environment via GitHub Actions (Recommended).

**Spec Agent:** For the capability config, should the project inherit the shipped defaults—GitHub Copilot harness, GitHub devtools, Azure cloud, default L1–L5 ladder at confirmed L2, default policy/best-practices/UX profiles, no pinned technical stack, built-in reusable skills, and PR-comment escalation—unless later requirements force an override?

**Human User:** Inherit all shipped defaults (Recommended).

**Spec Agent:** What persistence behavior should the to-do app provide?

**Human User:** Persist to-do items across browser sessions for the shared demo environment (Recommended).

**Spec Agent:** Who may access and modify the shared demo to-do data?

**Human User:** Anyone with the demo URL; show a clear non-sensitive-demo-data warning (Recommended).

**Spec Agent:** Which to-do fields and actions are required for the baseline product?

**Human User:** Title, optional description, completed state; create, list, update, complete or reopen, and delete (Recommended).

**Spec Agent:** How should workshop users trigger each self-healing scenario?

**Human User:** Separate GitHub Actions manual-dispatch inputs that apply a known scenario fixture on a temporary branch (Recommended).

**Spec Agent:** Which performance envelope should the specification use for the demo?

**Human User:** UI feedback within 100 ms, API p95 under 500 ms for 20 concurrent users, and each workflow under 10 minutes (Recommended).

**Spec Agent:** What benchmark gate should detect the duplicate-search regression without being too runner-sensitive?

**Human User:** 10,000 items; median of 5 runs under 250 ms, with correctness tests unchanged (Recommended).

**Spec Agent:** What line-coverage threshold should Scenario 2 restore?

**Human User:** 80% repository line coverage and 80% changed-line coverage (Recommended).

**Human User:** lets do everyting else and other rounds in autopilot

**Recorded execution-mode request:** Enable Autopilot for the remainder of the active baseline buildout, including the remaining discovery rounds, SPEC, PLAN, and EXECUTE. Activation is pending the required Human-User-authored version-control anchor.

The required Human-User-authored commit was not created after two verification attempts, so Autopilot never activated.

**Spec Agent:** Choose how to proceed: personally create the required authorization commit, or continue with explicit approvals in Interactive mode.

**Human User:** Continue in Interactive mode.

**Spec Agent:** What gate-time reliability should the remediation agent meet on the representative evaluation set?

**Human User:** At least 2 of 3 independent runs produce a policy-compliant patch that passes scenario validation (Recommended).

**Spec Agent:** What runtime LLM cost caps should the self-healing workflow enforce?

**Human User:** Track cost without enforcing caps.

**Spec Agent:** How tightly should remediation context be constrained?

**Human User:** Let the agent decide what repository context it needs.

**Spec Agent:** Should the agent be allowed to request additional read-only files when it states a reason, while secrets remain excluded and scenario-specific writable paths stay fixed?

**Human User:** No — allow unrestricted read access to the repository.

**Spec Agent:** How should this conflict with the approved seed be resolved?

**Human User:** Keep the approved seed and use bounded, reasoned read expansion (Recommended).

**Spec Agent:** How long should shared demo to-do data be retained?

**Human User:** Retain until manually deleted.

**Spec Agent:** What availability expectation should apply to the Azure demo environment?

**Human User:** No formal uptime SLA; health check, restart policy, and clear unavailable state are required (Recommended).

**Spec Agent:** Should escalation be overridden to GitHub Issues for this project?

**Human User:** Yes — use GitHub Issues (Recommended).

**Spec Agent:** Does this demo intentionally process any personal, customer-confidential, regulated, or production data?

**Human User:** No — public demo data only; prohibit secrets and PII (Recommended).

### Approved Round 2 content

- Product workflow: anonymous demo users create, list, edit, complete or reopen, and delete to-dos with a title, optional description, and completion state.
- Scenario workflow: separate manual GitHub Actions inputs apply deterministic fixtures on temporary branches; classification, policy, one remediation attempt, and deterministic validation produce a pull request or escalation issue.
- Agent context: normalized failure evidence and relevant files, with bounded and reasoned read-only expansion; secrets excluded and scenario-specific writable paths fixed.
- Thresholds: 80% repository and changed-line coverage; 10,000-item duplicate benchmark with median of five runs under 250 ms; UI feedback within 100 ms; API p95 under 500 ms at 20 concurrent users; workflow under 10 minutes.
- Agent evaluation: at least two of three gate-time runs produce a policy-compliant patch that passes scenario validation.
- Runtime: Azure demo or sandbox deployed only through GitHub Actions with OIDC and workload identity.
- Availability: no uptime SLA; health check, restart policy, and clear unavailable state required.
- Data: public demo data only, retained until manual deletion; PII, secrets, customer-confidential, regulated, and production data prohibited.
- Config: inherit shipped defaults except escalation is overridden to GitHub Issues; product runtime usage is tracked without enforced caps.

**Spec Agent:** Approve Round 2 Deep-dive content and proceed to Round 3 Edge cases?

**Human User:** Approved — proceed (Recommended).

## Round 3 — Edge cases

**Spec Agent:** What to-do input limits should the app enforce?

**Human User:** Trim input; title 1–120 characters, description 0–1000 characters; reject excess or empty titles with field errors (Recommended).

**Spec Agent:** How should concurrent manual runs of the same healing scenario be handled?

**Human User:** Allow one active run per scenario; reject a second run with a link to the active run (Recommended).

**Spec Agent:** If the one remediation attempt produces a patch but revalidation fails, what evidence should remain?

**Human User:** Do not open a PR; retain patch and validation artifacts on the run; create one deduplicated escalation issue linking the evidence (Recommended).

**Spec Agent:** What should happen for unsupported classifications, policy denial, low confidence, or unavailable agent service?

**Human User:** Make no code change; create or update one deduplicated escalation issue and stop (Recommended).

**Spec Agent:** May deterministic infrastructure steps retry transient failures without counting as a second remediation attempt?

**Human User:** Yes — one retry for checkout, artifact upload, or network timeout only; never retry remediation (Recommended).

**Spec Agent:** If source files, issue text, test names, or logs contain instruction-like text aimed at the remediation agent, how should the workflow respond?

**Human User:** Treat it as untrusted data, ignore embedded instructions, flag the event, and continue only with the approved scenario policy (Recommended).

**Spec Agent:** How should the API handle stale updates or deletes for a to-do that no longer exists?

**Human User:** Return a structured 404 error and refresh the UI list without retrying the mutation (Recommended).

**Spec Agent:** If GitHub cannot create the required PR or escalation issue after one transient retry, what should the run do?

**Human User:** Fail closed and retain a downloadable local escalation artifact in the workflow run for manual action (Recommended).

**Spec Agent:** How should temporary scenario branches be cleaned up?

**Human User:** Delete automatically after successful PR merge or closed escalation; retain failed-run artifacts for 30 days (Recommended).

**Spec Agent:** What should happen when a user manually cancels a scenario run?

**Human User:** Stop immediately, make no PR, preserve available evidence, and mark the run cancelled without escalation (Recommended).

### Approved Round 3 content

- Input limits, one-active-run concurrency, failed-remediation evidence, fail-closed unsupported states, one infrastructure retry, untrusted-instruction handling, structured stale-item errors, GitHub-outage evidence, branch cleanup, artifact retention, and cancellation behavior were approved as proposed.
- Recursive remediation, direct pushes, auto-merge, test disabling, threshold reduction, and benchmark-data reduction remain prohibited.

**Spec Agent:** Approve Round 3 Edge-case content and proceed to Round 4 Acceptance criteria?

**Human User:** Approved — proceed (Recommended).

## Round 4 — Acceptance criteria

**Spec Agent:** Should the UI acceptance evidence include a lightweight usability test with at least 3 representative workshop participants?

**Human User:** Yes — N≥3 before EXECUTE-EXIT (Recommended).

**Spec Agent:** What minimum deprecation notice should the versioned demo API require before removing a published version?

**Human User:** 90 days (Recommended).

**Spec Agent:** What abuse-control limit should apply to the anonymous demo API?

**Human User:** FOR ROUND 4 USE THE DEFAULT RECOMMENDED OPTIONS

**Recorded delegation for remaining Round 4 choices:** Apply every recommended default without further elicitation.

### Approved Round 4 content

The Human User approved a 30-item acceptance-evidence matrix covering product CRUD and validation, persistence, stale mutations, rate limiting, API contract, performance, accessibility, usability, scenario independence and runtime, all three healing outcomes, classification and policy, three-run agent evaluation, single-attempt fail-closed behavior, context and prompt-injection safety, concurrency and cancellation, human authority, platform outage, Azure delivery, security and supply chain, data controls, observability, cost tracking, cleanup and retention, extension points, and workshop comprehension. Each criterion defines what is assessed, its method and evidence, and an objective pass condition.

Recommended defaults applied include 60 requests per minute per source IP with structured `429` responses, a 90-day API deprecation window, and usability testing with at least three representative participants.

**Spec Agent:** Approve Round 4 Acceptance criteria and end the four-round requirements elicitation?

**Human User:** Approved — end the four rounds (Recommended).

### Acceptance refinement during SPEC self-review

**Spec Agent:** The recommended per-source-IP rate limit conflicts with the approved no-PII processing rule. How should the conflict be resolved?

**Human User:** Use an anonymous random client token and process no source IP in application telemetry (Recommended).

**Resolution:** The API rate limit is 60 requests per minute per random non-identifying client token. The token is not derived from a user, device, or network identifier, and application telemetry excludes source IP.

## Discovery termination

All required discovery dimensions were checked. Regulatory processing is N/A because the approved scope prohibits PII, secrets, customer-confidential, regulated, and production data. No unresolved ambiguity blocks specification drafting.

**Spec Agent:** End of discovery; proceed to 1b Business requirements?

**Human User:** Approved — end discovery and proceed (Recommended).
