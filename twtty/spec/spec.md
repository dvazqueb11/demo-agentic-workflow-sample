# Technical Specification — Baseline

## Metadata

- **Iteration ID:** baseline
- **Discovery mode:** Interactive
- **Specialization:** sdlc-for-agentic-apps
- **Runtime target:** azure
- **UX mode:** Delegated
- **API mode:** Delegated
- **Spec confirmed date:** 2026-10-07

## 1. Goals

- Deliver a familiar single-domain to-do web application that lets workshop participants create, view, edit, complete or reopen, and delete to-do items.
- Demonstrate three independently triggered self-healing CI scenarios: a unit-test failure, insufficient test coverage, and a performance regression.
- Keep reasoning-heavy automation limited to one bounded remediation attempt while classification, policy enforcement, validation, and outcome handling remain deterministic.
- Produce a human-review-required pull request when remediation succeeds and a deduplicated escalation issue or downloadable escalation artifact when remediation cannot safely complete.
- Provide a reusable reference implementation whose provider extension points can support future engineering toolchains without redesigning the core orchestration.

## 2. Stakeholders

- **Workshop participant or customer engineer:** Uses the to-do application, triggers one demonstration scenario, inspects workflow evidence, and explains the control flow after a guided run.
- **Workshop facilitator:** Prepares the demo environment, guides scenario execution, resets demo data, and explains successful and escalated outcomes.
- **Repository maintainer:** Adapts the template, maintains deterministic policies and fixtures, reviews generated pull requests, and preserves the security and runtime controls.
- **Human reviewer:** Reviews and explicitly approves or rejects every remediation pull request; no workflow may substitute for this authority.
- **Cloud and developer-platform owner:** Maintains the Azure sandbox, GitHub repository controls, short-lived identity federation, and platform availability needed by the demo.

## 3. Success metrics

- **Scenario completion:** During each baseline release validation, 100% of the three scenarios can be triggered and executed independently, with each workflow reaching a terminal outcome in less than 10 minutes.
- **Successful healing:** During baseline acceptance, each supported scenario produces a policy-compliant patch that passes its declared validation and opens a pull request in at least 2 of 3 independent gate-time evaluation runs.
- **Fail-closed handling:** Across the baseline failure-mode test suite, 100% of unsupported, denied, low-confidence, unavailable-agent, and failed-revalidation cases make no prohibited code change and produce exactly one deduplicated escalation outcome.
- **Human authority:** Across static workflow inspection and baseline end-to-end tests, zero paths auto-merge, push directly to `main`, bypass branch protection, or remove the required human review.
- **Application performance:** In the baseline load-test report, API latency is below 500 ms at p95 with 20 concurrent users and zero unexpected 5xx responses.
- **UI responsiveness:** In the baseline browser performance report, every primary user action displays feedback within 100 ms.
- **Coverage:** At baseline acceptance and after Scenario 2 remediation, repository line coverage and changed-line coverage are each at least 80%.
- **Duplicate-search performance:** At baseline acceptance and after Scenario 3 remediation, the median of five runs over 10,000 items is below 250 ms while all correctness tests pass unchanged.
- **Accessibility:** Before baseline closeout, automated and manual checks report zero critical or serious WCAG 2.2 AA violations, and all primary interactions are keyboard operable.
- **Workshop comprehension:** In a baseline usability session with at least three representative participants, every participant completes the primary CRUD journey without facilitator intervention and correctly identifies the six control-flow steps plus the human merge requirement after one guided scenario.

## 4. Constraints

- The application core MUST use C++17 and remain intentionally small enough for workshop participants to understand.
- The product MUST be a single-domain to-do application with a lightweight browser UI and a project-owned, versioned API contract.
- GitHub MUST provide repository hosting, manual scenario dispatch, CI, pull requests, issues, and the agentic remediation integration.
- The Azure target MUST be a demo or sandbox environment and MUST be deployed only through GitHub Actions using short-lived identity federation and workload identity; local or portal deployment is prohibited.
- The workflow MUST use one remediation agent, one diagnosis, one remediation proposal, and one validation per run. Remediation MUST never retry.
- Deterministic infrastructure steps MAY retry once only for checkout, artifact-upload, or network-timeout failures.
- The workflow MUST fail closed when classification is unsupported, policy denies the action, confidence is insufficient, the agent service is unavailable, validation fails, or the required platform outcome cannot be created.
- Remediation MUST start with a normalized failure bundle and relevant files. Additional read-only context MUST require a stated reason and audit record. Secrets remain excluded, and scenario-specific writable paths remain fixed.
- The workflow MUST NOT delete, disable, or skip tests; lower coverage or benchmark thresholds; reduce benchmark data; modify workflow policy to evade a gate; add unsafe concurrency; auto-merge; push to `main`; or override branch protection.
- Shared to-do content MUST be treated as public demo data. The UI MUST prohibit secrets, PII, customer-confidential, regulated, and production data; no external regulatory regime applies to the approved scope.
- Runtime LLM token and estimated cost usage MUST be measured and reported per remediation, session, and day, but the baseline imposes no product-runtime token or currency cap.
- The API contract MUST use OpenAPI 3.1, publish a 90-day deprecation policy, and enforce 60 requests per minute per source IP with structured rate-limit errors.
- The UI MUST meet WCAG 2.2 AA. Security verification MUST use OWASP ASVS and the OWASP API Security Top 10. Release evidence MUST include an SPDX or CycloneDX SBOM and SLSA provenance. Workflow telemetry MUST be OpenTelemetry-compatible.
- Coverity, SonarQube, Synopsys toolchain validation, JFrog, Artifactory, Jira, Azure DevOps, regression farms, EDA validation, and Verdi are extension points only and MUST NOT be implemented in the baseline.
