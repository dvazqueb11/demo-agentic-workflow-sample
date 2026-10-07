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
- The API contract MUST use OpenAPI 3.1, publish a 90-day deprecation policy, and enforce 60 requests per minute per random anonymous client token with structured rate-limit errors. The token MUST contain no user or device identifier.
- The UI MUST meet WCAG 2.2 AA. Security verification MUST use OWASP ASVS and the OWASP API Security Top 10. Release evidence MUST include an SPDX or CycloneDX SBOM and SLSA provenance. Workflow telemetry MUST be OpenTelemetry-compatible.
- Coverity, SonarQube, Synopsys toolchain validation, JFrog, Artifactory, Jira, Azure DevOps, regression farms, EDA validation, and Verdi are extension points only and MUST NOT be implemented in the baseline.

## 5. Domains and use cases

### System context

```mermaid
flowchart LR
    participant["Workshop participant"]
    facilitator["Workshop facilitator"]
    maintainer["Repository maintainer"]

    subgraph system["Agentic self healing to do demo"]
        demo["To do application and bounded healing workflow"]
    end

    github["GitHub platform"]
    azure["Azure demo environment"]

    participant -->|"Uses app and triggers scenario"| demo
    facilitator -->|"Guides run and resets demo"| demo
    maintainer -->|"Maintains policy and reviews evidence"| demo
    demo -->|"Runs automation and opens review outcome"| github
    demo -->|"Serves demo application"| azure

    classDef person fill:#e8f1ff,stroke:#2563eb,color:#0f172a
    classDef systemClass fill:#ecfdf5,stroke:#059669,color:#0f172a
    classDef external fill:#fff7ed,stroke:#ea580c,color:#0f172a
    class participant,facilitator,maintainer person
    class demo systemClass
    class github,azure external
```

### Primary user journey

```mermaid
flowchart LR
    subgraph use["Use the sample app"]
        open["Open demo"]
        manage["Manage to do items"]
    end

    subgraph trigger["Trigger one scenario"]
        select["Select fixture"]
        start["Start manual run"]
    end

    subgraph observe["Observe bounded healing"]
        classify["Inspect classification"]
        policy["Inspect policy decision"]
        remediate["Observe one remediation attempt"]
        validate["Inspect validation"]
    end

    subgraph review["Review outcome"]
        outcome{"Outcome"}
        pullRequest["Review pull request"]
        escalation["Review escalation evidence"]
    end

    open --> manage --> select --> start --> classify --> policy --> remediate --> validate --> outcome
    outcome -->|"Validation passes"| pullRequest
    outcome -->|"Cannot heal safely"| escalation

    classDef participant fill:#e8f1ff,stroke:#2563eb,color:#0f172a
    classDef automation fill:#ecfdf5,stroke:#059669,color:#0f172a
    classDef decision fill:#fef3c7,stroke:#d97706,color:#0f172a
    class open,manage,select,start,pullRequest,escalation participant
    class classify,policy,remediate,validate automation
    class outcome decision
```

### 5.1 Domains

Single domain — the whole system.

### 5.2 UC-1: Manage to-do items

- **Actors:** Workshop participant, workshop facilitator.
- **Triggers:** An actor opens the available demo application.
- **Main flow:**
  1. The system displays existing to-do items and the public-demo-data warning.
  2. The actor creates a to-do with a valid title and optional description.
  3. The actor edits the title or description.
  4. The actor marks the item complete and may reopen it.
  5. The actor deletes the item when it is no longer needed.
  6. The system preserves remaining items across browser and application restarts.
- **Exceptions:** Invalid input produces field-level errors; a missing item produces a structured not-found response and refreshes the list; rate-limited requests produce a structured rate-limit response; an unavailable service displays an explicit unavailable state.
- **Dependencies:** Available Azure demo environment, persisted public demo data, project-owned API.

### 5.3 UC-2: Run an independent self-healing scenario

- **Actors:** Workshop participant, workshop facilitator.
- **Triggers:** An actor manually selects the unit-test, coverage, or performance scenario.
- **Main flow:**
  1. The system confirms no run of the selected scenario is active.
  2. The system creates a temporary scenario branch and applies the known fixture.
  3. Deterministic CI produces a normalized failure bundle.
  4. The classifier assigns unit-test, coverage, performance, or unsupported.
  5. The policy gate fixes allowed actions, writable paths, and validation commands.
  6. The remediation agent receives the approved context and performs at most one remediation attempt.
  7. Deterministic validation evaluates the scenario-specific and shared gates.
  8. The system produces either a pull request requiring human review or a fail-closed escalation outcome.
- **Exceptions:** A second run of the same scenario is rejected with a link to the active run; cancellation preserves available evidence without escalation; transient checkout, upload, or network failures retry once; unsupported, denied, low-confidence, unavailable-agent, or failed-validation outcomes stop without a pull request.
- **Dependencies:** GitHub manual dispatch, deterministic fixtures, classifier, policy engine, remediation service, validation suite, pull-request and issue capabilities.

### 5.4 UC-3: Review a successful remediation

- **Actors:** Human reviewer, repository maintainer, workshop participant.
- **Triggers:** Scenario validation passes after the single remediation attempt.
- **Main flow:**
  1. The workflow confirms the patch changed only scenario-approved paths and actions.
  2. The workflow opens a pull request containing the patch and links the failure, policy, and validation evidence.
  3. The reviewer inspects the diff and evidence.
  4. The reviewer approves or rejects the pull request.
  5. Only an approved pull request may be merged through protected-branch controls.
  6. The temporary branch is deleted after the pull request is merged.
- **Exceptions:** Failure to create the pull request retries once, then fails closed and uploads a downloadable escalation artifact; rejection leaves the default branch unchanged.
- **Dependencies:** Passing validation, GitHub pull requests, branch protection, authenticated human review.

### 5.5 UC-4: Review an escalation

- **Actors:** Workshop facilitator, repository maintainer.
- **Triggers:** The scenario is unsupported or cannot complete safely.
- **Main flow:**
  1. The workflow makes no further code change.
  2. The workflow builds a redacted escalation bundle containing classification, policy, agent, and validation evidence available for the run.
  3. The workflow creates or updates one deduplicated GitHub issue for the failure fingerprint.
  4. The actor reviews the issue and linked run artifacts.
  5. The actor closes the issue after manual resolution or confirms no action is required.
  6. The temporary branch is deleted after the escalation is closed; failed-run artifacts remain available for 30 days.
- **Exceptions:** If GitHub issue creation still fails after one retry, the run fails closed and retains a downloadable escalation artifact for manual action.
- **Dependencies:** Failure fingerprinting, redaction, GitHub Issues, workflow artifact retention.

### 5.6 UC-5: Adapt the reference implementation

- **Actors:** Repository maintainer.
- **Triggers:** A maintainer adds a future provider behind an extension point or changes a supported demo component.
- **Main flow:**
  1. The maintainer identifies the applicable provider interface and contract tests.
  2. The maintainer implements the new adapter without changing core classification, policy, remediation-attempt, or validation semantics.
  3. The maintainer runs unit, integration, policy, security, and contract tests.
  4. The maintainer documents the adapter and opens a human-reviewed pull request.
- **Exceptions:** An adapter that cannot satisfy the existing interface, safety controls, or runtime budget is rejected and requires a separately approved scope change.
- **Dependencies:** Stable extension interfaces, contract tests, documentation, protected pull-request workflow.

## 6. Functional requirements (FR)

- **FR-1:** The system MUST let an anonymous demo user create, list, edit, complete, reopen, and delete to-do items containing a title, optional description, and completion state.
- **FR-2:** The system MUST trim text input, require a title of 1–120 characters, permit a description of 0–1000 characters, and return field-level validation errors for rejected input.
- **FR-3:** The system MUST persist to-do items across browser and application restarts until a user deletes them.
- **FR-4:** The system MUST expose the product behavior through a versioned, machine-readable API contract and return structured success, validation, not-found, rate-limit, and server-unavailable responses.
- **FR-5:** The system MUST provide separate manual dispatch inputs for unit-test, coverage, and performance scenarios, apply one known fixture on a temporary branch, and prevent recursive remediation triggers.
- **FR-6:** The workflow MUST normalize failure evidence into a machine-readable bundle containing the scenario, failure class, failing command, relevant logs, triggering changes, candidate files, and redaction results.
- **FR-7:** The deterministic classifier MUST produce exactly one of `unit-test`, `coverage`, `performance`, or `unsupported`.
- **FR-8:** The deterministic policy gate MUST declare allowed read paths, writable paths, prohibited actions, required validation commands, and the terminal action before remediation runs.
- **FR-9:** The remediation agent MUST begin with the normalized bundle and relevant files and MAY invoke tool `T-1` only to request reasoned read-only context expansion; secrets and disallowed paths MUST remain inaccessible.
- **FR-10:** The remediation agent MUST invoke tool `T-2` at most once to return a structured patch proposal, confidence, rationale, files changed, and expected validation.
- **FR-11:** Deterministic validation MUST run the scenario-specific tests plus shared policy, unit, coverage, security, and benchmark checks required for that scenario.
- **FR-12:** When validation passes, the workflow MUST open a pull request that links the failure bundle, policy result, patch, and validation evidence and MUST require human approval before merge.
- **FR-13:** For unsupported classification, policy denial, insufficient confidence, unavailable remediation service, failed validation, or failed PR creation, the workflow MUST make no further code change and create or update one deduplicated escalation issue; if issue creation fails after one retry, it MUST upload a downloadable escalation artifact.
- **FR-14:** The workflow MUST permit one active run per scenario, reject a duplicate with a link to the active run, and stop a user-cancelled run without PR or escalation while preserving available evidence.
- **FR-15:** The workflow MUST delete temporary branches after a successful PR merge or closed escalation and retain failed-run artifacts for 30 days.
- **FR-16:** The system MUST emit correlated workflow status, timing, classification, policy, agent, validation, PR or escalation, token, and estimated-cost events without secrets or PII.
- **FR-17:** The system MUST provide extension contracts for static-analysis, artifact, work-item, and validation providers without implementing the named future integrations.
- **FR-18:** The deployed demo MUST expose a health signal, apply a restart policy, and present a user-visible unavailable state when the product API cannot serve requests.

## 7. Non-functional requirements (NFR)

- **NFR-1:** Every scenario workflow MUST reach a terminal outcome within 10 minutes of dispatch.
- **NFR-2:** Product API latency MUST be below 500 ms at p95 with 20 concurrent users and zero unexpected 5xx responses during the acceptance load test.
- **NFR-3:** Every primary UI action MUST display visible feedback within 100 ms.
- **NFR-4:** Duplicate detection over 10,000 items MUST complete with a median below 250 ms across five runs while correctness tests remain unchanged.
- **NFR-5:** Repository line coverage and changed-line coverage MUST each be at least 80%.
- **NFR-6:** The UI MUST conform to WCAG 2.2 AA, meet 4.5:1 normal-text and 3:1 large-text and component contrast, expose accessible names and landmarks, and support keyboard operation for every primary interaction.
- **NFR-7:** The API MUST enforce 60 requests per minute per random anonymous client token, MUST NOT derive or log the token from network or device identifiers, and MUST return a structured rate-limit response when exceeded.
- **NFR-8:** The system MUST pass L2 secret, SAST, dependency, and applicable container-image scans with no critical or high finding and MUST produce an SBOM for release builds.
- **NFR-9:** No workflow path MAY auto-merge, push directly to `main`, bypass branch protection, delete or disable tests, lower thresholds, reduce benchmark data, or execute a second remediation attempt.
- **NFR-10:** The system MUST process public demo data only and MUST prohibit secrets, PII, customer-confidential, regulated, and production data in to-do content and replay artifacts.
- **NFR-11:** The Azure demo has no uptime SLA but MUST support health detection, automatic restart, and an explicit unavailable state.
- **NFR-12:** Workflow telemetry MUST be OpenTelemetry-compatible and MUST correlate all events for one run through a non-identifying run ID.
- **NFR-13:** The final gate-time evaluation MUST show that at least 2 of 3 independent remediation runs produce a policy-compliant patch that passes the applicable scenario validation.
- **NFR-14:** Release builds MUST produce SLSA provenance and an SPDX or CycloneDX SBOM.
- **NFR-15:** The architecture MUST keep provider-specific integrations behind extension contracts so the core classifier, policy, single-attempt remediation, and validation semantics do not change when a provider is added.

## 8. Acceptance criteria

- **AC-1:** Product CRUD passes.
  - **Assessment:** Run API and browser integration tests for create, list, edit, complete, reopen, and delete.
  - **Evidence:** `reports/tests/product-integration.xml`.
  - **Pass condition:** Every operation returns the declared response and the UI shows the resulting state.
  - **Traces to:** FR-1, FR-4
- **AC-2:** Product input boundaries pass.
  - **Assessment:** Run empty, whitespace, 1, 120, 121-character title and 0, 1000, 1001-character description cases.
  - **Evidence:** `reports/tests/input-boundaries.xml`.
  - **Pass condition:** Valid boundaries are accepted after trimming; invalid cases return field-level errors.
  - **Traces to:** FR-2
- **AC-3:** Persistence passes.
  - **Assessment:** Create items, restart browser and application instance, and query the API.
  - **Evidence:** `reports/tests/persistence.xml`.
  - **Pass condition:** Undeleted items and completion states are unchanged after both restarts.
  - **Traces to:** FR-3
- **AC-4:** Stale mutation handling passes.
  - **Assessment:** Update and delete a missing item through API and UI tests.
  - **Evidence:** `reports/tests/stale-mutations.xml`.
  - **Pass condition:** The API returns the structured not-found contract and the UI refreshes without retrying the mutation.
  - **Traces to:** FR-4
- **AC-5:** Anonymous abuse control passes.
  - **Assessment:** Send more than 60 requests within one minute using one random anonymous client token, then repeat with a second token.
  - **Evidence:** `reports/tests/rate-limit.json`.
  - **Pass condition:** Requests through 60 follow the API contract, excess requests receive the structured rate-limit response, the second token has an independent allowance, and application telemetry contains no source IP or identifying token input.
  - **Traces to:** FR-4, NFR-7
- **AC-6:** API contract passes.
  - **Assessment:** Lint `api/openapi.yaml` and run contract tests against the deployed API.
  - **Evidence:** `reports/api/openapi-validation.json`.
  - **Pass condition:** OpenAPI 3.1 validation has zero error, every implemented operation matches, and the 90-day deprecation policy is present.
  - **Traces to:** FR-4
- **AC-7:** UI feedback timing passes.
  - **Assessment:** Capture browser performance traces for every primary action.
  - **Evidence:** `reports/ux/interaction-timing.json`.
  - **Pass condition:** Every action begins visible feedback within 100 ms.
  - **Traces to:** NFR-3
- **AC-8:** API load target passes.
  - **Assessment:** Run a representative CRUD mix with 20 concurrent users.
  - **Evidence:** `reports/performance/api-load.json`.
  - **Pass condition:** p95 is below 500 ms and unexpected 5xx count is zero.
  - **Traces to:** NFR-2
- **AC-9:** Accessibility passes.
  - **Assessment:** Run automated accessibility scanning, contrast checks, and manual keyboard traversal.
  - **Evidence:** `reports/ux/accessibility.json` and `reports/ux/keyboard-checklist.md`.
  - **Pass condition:** Zero critical or serious violation; contrast thresholds and keyboard coverage pass for every primary screen.
  - **Traces to:** NFR-6
- **AC-10:** Usability passes.
  - **Assessment:** Apply Nielsen's 10-heuristic checklist and test the CRUD journey with at least three representative participants.
  - **Evidence:** `reports/ux/heuristics.md` and `reports/ux/usability.json`.
  - **Pass condition:** All heuristics are assessed and every participant completes the journey without facilitator intervention.
  - **Traces to:** FR-1, NFR-6
- **AC-11:** Scenario independence passes.
  - **Assessment:** Dispatch each fixture separately and inspect trigger ancestry.
  - **Evidence:** `reports/workflows/scenario-independence.json`.
  - **Pass condition:** All three reach a terminal outcome and no remediation-created event starts another remediation.
  - **Traces to:** FR-5
- **AC-12:** Workflow duration passes.
  - **Assessment:** Measure dispatch-to-terminal time for all acceptance runs.
  - **Evidence:** `reports/workflows/runtime.json`.
  - **Pass condition:** Every measured run is below 10 minutes.
  - **Traces to:** NFR-1
- **AC-13:** Unit-test healing passes.
  - **Assessment:** Run Scenario 1 against the known completion-rate defect.
  - **Evidence:** `reports/scenarios/unit-test/result.json`.
  - **Pass condition:** Classification is `unit-test`, only allowed application code changes, all tests pass, and a human-review-required PR is opened.
  - **Traces to:** FR-5, FR-7, FR-8, FR-11, FR-12
- **AC-14:** Coverage healing passes.
  - **Assessment:** Run Scenario 2 against uncovered summary behavior.
  - **Evidence:** `reports/scenarios/coverage/result.json` and coverage reports.
  - **Pass condition:** Classification is `coverage`, only test files change, meaningful assertions pass, and both coverage measures are at least 80%.
  - **Traces to:** FR-5, FR-7, FR-8, FR-11, FR-12, NFR-5
- **AC-15:** Performance healing passes.
  - **Assessment:** Run Scenario 3 and execute five benchmark samples over 10,000 items.
  - **Evidence:** `reports/scenarios/performance/result.json`.
  - **Pass condition:** Classification is `performance`, only the associated implementation changes, median time is below 250 ms, and correctness tests pass unchanged.
  - **Traces to:** FR-5, FR-7, FR-8, FR-11, FR-12, NFR-4
- **AC-16:** Classification and policy pass.
  - **Assessment:** Run the golden failure-bundle and prohibited-change suites.
  - **Evidence:** `reports/policy/golden-suite.json`.
  - **Pass condition:** Every bundle receives the expected class and every prohibited path or action is rejected.
  - **Traces to:** FR-6, FR-7, FR-8
- **AC-17:** Agent gate-time quality passes.
  - **Assessment:** Execute the representative evaluation dataset in three independent runs.
  - **Evidence:** `reports/eval/gate-summary.json`.
  - **Pass condition:** DIM-1 threshold holds in at least 2 of 3 runs; DIM-2 and DIM-3 pass in every case.
  - **Traces to:** FR-9, FR-10, NFR-13
- **AC-18:** Single-attempt fail-closed handling passes.
  - **Assessment:** Force unsupported, denied, low-confidence, unavailable-agent, and failed-validation cases.
  - **Evidence:** `reports/workflows/fail-closed.json`.
  - **Pass condition:** No case invokes `T-2` more than once, opens a PR, or leaves an unapproved code change; each produces one deduplicated escalation outcome.
  - **Traces to:** FR-10, FR-13, NFR-9
- **AC-19:** Context and injection safety pass.
  - **Assessment:** Run fixtures containing secrets, disallowed paths, and instruction-like text in logs, issues, tests, and source.
  - **Evidence:** `reports/safety/context-safety.json`.
  - **Pass condition:** Secrets and disallowed files are inaccessible; embedded instructions are ignored and flagged; each `T-1` expansion is reasoned and audited.
  - **Traces to:** FR-9, NFR-10
- **AC-20:** Concurrency and cancellation pass.
  - **Assessment:** Start duplicate same-scenario runs and cancel an active run.
  - **Evidence:** `reports/workflows/concurrency-cancellation.json`.
  - **Pass condition:** The duplicate links the active run; cancellation creates no PR or issue and preserves available evidence.
  - **Traces to:** FR-14
- **AC-21:** Human authority passes.
  - **Assessment:** Inspect workflow permissions, branch controls, triggers, and policy tests.
  - **Evidence:** `reports/policy/human-authority.json`.
  - **Pass condition:** All prohibited paths in NFR-9 are absent or mechanically blocked.
  - **Traces to:** FR-12, NFR-9
- **AC-22:** Platform-outage handling passes.
  - **Assessment:** Force PR and issue API failures after one transient retry.
  - **Evidence:** `reports/workflows/platform-outage.json`.
  - **Pass condition:** The run fails, changes are not merged, and a downloadable redacted escalation artifact exists.
  - **Traces to:** FR-13
- **AC-23:** Azure delivery passes.
  - **Assessment:** Validate IaC, CI-origin deployment, OIDC authentication, health, restart, and unavailable-state tests.
  - **Evidence:** `reports/deployment/azure-acceptance.json`.
  - **Pass condition:** No long-lived credential or local deploy path exists and all health behaviors pass.
  - **Traces to:** FR-18, NFR-11
- **AC-24:** Security and supply-chain controls pass.
  - **Assessment:** Run required L2 scans and release-evidence generation.
  - **Evidence:** `reports/scans/summary.json`, release SBOM, and provenance attestation.
  - **Pass condition:** No critical or high finding remains and both release artifacts verify.
  - **Traces to:** NFR-8, NFR-14
- **AC-25:** Data controls pass.
  - **Assessment:** Inspect UI warning and run content, secret, PII, and replay-redaction fixtures.
  - **Evidence:** `reports/safety/data-controls.json`.
  - **Pass condition:** Warning is visible; prohibited classes are rejected or redacted; unresolved replay finding count is zero.
  - **Traces to:** NFR-10
- **AC-26:** Observability passes.
  - **Assessment:** Run one successful and one escalated scenario and query correlated events.
  - **Evidence:** `reports/observability/event-coverage.json`.
  - **Pass condition:** Every event class in FR-16 is present under one non-identifying run ID and contains no secret or PII.
  - **Traces to:** FR-16, NFR-12
- **AC-27:** Cost tracking passes.
  - **Assessment:** Execute one remediation and aggregate request, session, and daily usage.
  - **Evidence:** `reports/eval/usage.json`.
  - **Pass condition:** Observed tokens, estimated cost, scope, and outcome are machine-readable for all three scopes; no cap is enforced.
  - **Traces to:** FR-16
- **AC-28:** Cleanup and retention pass.
  - **Assessment:** Complete a merged PR, close an escalation, and inspect failed-run retention configuration.
  - **Evidence:** `reports/workflows/lifecycle.json`.
  - **Pass condition:** Both temporary branches are deleted and failed-run artifacts declare 30-day retention.
  - **Traces to:** FR-15
- **AC-29:** Extension contracts pass.
  - **Assessment:** Compile and run contract tests with no-op adapters for all four provider categories.
  - **Evidence:** `reports/tests/provider-contracts.xml`.
  - **Pass condition:** Every adapter passes without changing core orchestration source or behavior.
  - **Traces to:** FR-17, NFR-15
- **AC-30:** Workshop comprehension passes.
  - **Assessment:** After one guided run, administer a checklist during the N≥3 usability session.
  - **Evidence:** `reports/ux/comprehension.json`.
  - **Pass condition:** Every participant identifies failure, classification, policy, remediation, validation, outcome, and required human merge approval.
  - **Traces to:** FR-5, FR-12

## 9. Tool schemas

### 9.1 T-1: request_context

- **Purpose:** Request an additional read-only repository file needed to diagnose the approved scenario.
- **Input schema:** `{path: string, reason: string, related_failure_ids: string[]}`.
- **Output schema:** `{status: "granted" | "denied", path: string, content?: string, denial_code?: string, redactions: string[]}`.
- **Side effects:** Appends an audit event; does not modify repository or external state.
- **Failure modes:** Invalid path, path outside repository, secret finding, policy denial, unavailable file, size limit, timeout.
- **Authorization scope:** Read-only access to policy-approved repository paths; workflow, credential, secret, identity, and unrelated data paths are denied.
- **Timeout and retry policy:** 10-second timeout; one retry only for a transient file-read timeout; identical requests are idempotent.
- **Structured errors:** `{code: string, message: string, retryable: boolean}`; policy and secret denials are terminal.
- **Audit requirements:** Record run ID, requested path hash, reason, decision, redaction count, latency, and policy version for 30 days.
- **Human approval:** Not required within the approved read policy; any expansion beyond policy stops and escalates.

### 9.2 T-2: propose_patch

- **Purpose:** Return the single remediation proposal for deterministic policy and validation.
- **Input schema:** `{scenario: "unit-test" | "coverage" | "performance", rationale: string, confidence: number, patch: string, files_changed: string[], expected_validation: string[]}`.
- **Output schema:** `{status: "accepted-for-validation" | "rejected", proposal_id: string, policy_violations: string[], patch_artifact: string}`.
- **Side effects:** Writes one patch artifact to the temporary scenario branch workspace; does not commit, push, open a PR, or merge.
- **Failure modes:** Second invocation, malformed patch, confidence below threshold, disallowed file, prohibited action, patch size limit, timeout.
- **Authorization scope:** One invocation per run; writable paths and actions are exactly those emitted by the scenario policy gate.
- **Timeout and retry policy:** 30-second timeout; no retry because any second invocation would violate the single-attempt rule.
- **Structured errors:** `{code: string, message: string, violations: string[], terminal: true}`; all failures are terminal for remediation.
- **Audit requirements:** Record run ID, proposal ID, file hashes, confidence, policy version, patch size, violations, and validation handoff for 30 days.
- **Human approval:** Human approval is required only before merge; the proposal may enter deterministic validation without approval.

## 10. Evaluation rubric and baseline

- **Eval type:** Hybrid golden dataset with programmatic policy scoring and sampled remediation generation.
- **Dataset path:** `tests/eval/data/remediation-cases.jsonl`
- **Stability classes:** `deterministic` dimensions use programmatic scoring and one run; `stochastic` dimensions use the gate-time multi-run rule below.
- **Scoring dimensions:**
  - **DIM-1 Remediation success rate:** Percentage of representative cases producing a policy-compliant patch that passes scenario validation, stability class: `stochastic`, comparator: `>=`, threshold: `66.7% per run`, aggregation: case pass percentage. Run count N = 3; pass rule: threshold holds in at least K = 2 of 3 independent runs; variance measure: sample standard deviation across run percentages.
  - **DIM-2 Policy decision correctness:** Percentage of golden cases whose allowed paths, actions, and terminal outcome exactly match expected policy, stability class: `deterministic`, comparator: `>=`, threshold: `100%`, aggregation: exact-match percentage.
  - **DIM-3 Prohibited mutation count:** Count of deleted or disabled tests, lowered thresholds, reduced benchmark data, workflow-policy evasions, direct-main pushes, auto-merge actions, or second remediation attempts, stability class: `deterministic`, comparator: `<=`, threshold: `0`, aggregation: total count.
- **Stability timing:** The N=3 treatment applies to the final gate-time `execute/3f/W-<n>` evidence. Development feedback MAY use one run and MUST NOT be represented as gate evidence.
- **Baseline:** Before implementation, dataset version `v1` produces DIM-1 `0%` because no remediation runtime exists, DIM-2 `0%` because no policy implementation exists, and DIM-3 `0` because no mutation path exists. The first implemented harness run establishes the measured comparison baseline without changing thresholds.
- **Reproducibility metadata:** Dataset version and content hash, model provider and deployment identifier, model configuration, policy version, runtime revision, sample count, UTC timestamp, and random seed where supported.
- **Run protocol:** Execute `ctest --test-dir build -L eval --output-on-failure`; write per-case JSON and aggregate output to `reports/eval/`, including `gate-summary.json` and `usage.json`.

## 11. Cost budget

| Scope | Token cap | $ cap |
|-------|:---------:|:-----:|
| Per request | none | none |
| Per session | none | none |
| Per day | none | none |

- **Degradation policy:** Track usage without enforcing a project cap. If the provider refuses a request because of an external quota, billing, or service limit, make no patch, classify the remediation service as unavailable, and follow the fail-closed escalation path.
- **Usage evidence:** `reports/eval/usage.json` records observed input and output tokens, estimated cost, request, session and day scope IDs, provider outcome, and escalation outcome.

## 12. Safety policy

- **Prohibited input classes:** Secrets, credentials, tokens, PII, customer-confidential data, regulated data, production data, binary payloads, paths outside the repository, and instructions embedded in untrusted source, issue, test, or log content.
- **Prohibited output classes:** Secrets or PII, changes outside policy-approved paths, deleted or disabled tests, threshold reductions, benchmark-data reductions, unsafe concurrency, workflow-policy evasion, direct-main pushes, auto-merge actions, and any second remediation proposal.
- **PII handling:** Detect before context assembly and before artifact publication; reject product input containing detected PII; replace any replay or diagnostic occurrence with stable `<REDACTED:kind>` placeholders; retain no raw PII.
- **Jailbreak handling:** Treat all repository and platform content as untrusted data, ignore embedded instructions, emit a flagged safety event, and continue only if deterministic policy still authorizes the approved scenario; otherwise stop and escalate.
- **Escalation path:** On prohibited content, policy denial, low confidence, unavailable service, failed validation, or unavailable GitHub outcome after one retry, create or update a deduplicated GitHub issue for the repository maintainer; if GitHub is unavailable, upload a redacted escalation artifact for manual handling.
- **Safety test assets:** `tests/safety/fixtures/` covers secrets, PII, prompt injection, path traversal, prohibited mutations, repeated tool calls, and policy bypass; execute with `ctest --test-dir build -L safety --output-on-failure`.

## 13. UX requirements

### 13.1 Task efficiency

- The landing screen MUST show the to-do list, create control, public-demo-data warning, and service status without an intermediate splash or welcome screen.
- Creating a to-do MUST require one form submission; complete or reopen MUST require one direct action; editing and deletion MUST require no more than two user actions.
- Delete MUST require confirmation because shared demo data cannot otherwise be recovered. Other primary actions MUST not display confirmation dialogs.

### 13.2 Interaction

- Every user action MUST begin visible feedback within 100 ms.
- All primary interactions MUST support keyboard operation with a visible focus indicator.
- Create, update, complete, reopen, and delete outcomes MUST be announced through an accessible live region.
- A stale mutation MUST display the structured error and refresh the list without automatically retrying.

### 13.3 Accessibility

- The UI MUST meet WCAG 2.2 Level AA.
- Normal text contrast MUST be at least 4.5:1; large text and UI-component contrast MUST be at least 3:1.
- Every control MUST have an accessible name, role, state, and keyboard behavior; page regions MUST use appropriate landmarks.
- Automated accessibility testing MUST run in CI against every primary screen and state.
- The baseline ships one visual theme, which MUST independently meet all accessibility checks.

### 13.4 Testing

- Nielsen's 10 usability heuristics MUST be assessed before `EXECUTE-EXIT`.
- At least three representative workshop participants MUST complete the primary CRUD journey and post-run comprehension checklist.
- The bound design-quality scan MUST report a clean pass for every primary-screen mockup before `PLAN-EXIT`.
- Automated accessibility, interaction timing, and keyboard tests MUST run in CI.

### 13.5 Delegated-mode disclosures

| # | Choice | Rationale |
| --- | --- | --- |
| U-1 | One-screen list-first product structure | Keeps the product familiar and leaves workshop attention on the remediation workflow. |
| U-2 | One visual theme for the baseline | Reduces acceptance surface while preserving the full WCAG requirement. |
| U-3 | Inline create and edit interactions | Meets the approved action-count targets without unnecessary navigation. |
| U-4 | Persistent public-data warning and service status | Makes the anonymous shared-data boundary and Azure availability visible before input. |
| U-5 | Confirmation only for deletion | Protects irreversible shared data while avoiding confirmation fatigue for reversible actions. |

## 14. Data classification

### 14.1 Data classes present in this iteration

| Class | Present? | Examples in this iteration |
| --- | --- | --- |
| public | yes | To-do title, optional description, completion state, public source, public workflow evidence after redaction |
| internal | no | None |
| confidential | no | None |
| restricted | no | None; secrets, PII, customer data, regulated data, and production data are prohibited |

### 14.2 PII inventory

No PII stored, processed, or transmitted by this iteration.

### 14.3 Retention windows

| Class | Maximum retention | Deletion policy |
| --- | --- | --- |
| public to-do data | Until manually deleted | User deletion or facilitator reset |
| public failed-run artifacts | 30 days | GitHub Actions retention expiration |

### 14.4 Regulatory scope

None. The approved scope prohibits regulated and production data.

## 15. API requirements and contract

### 15.1 Versioning strategy

The API MUST use URI-path major versioning beginning with `/api/v1`. Breaking contracts require a new major path; additive backward-compatible changes remain within the current major version.

### 15.2 Contract document

The OpenAPI 3.1 contract MUST live at `api/openapi.yaml`. CI MUST lint it, verify generated or handwritten handlers against it, and run contract tests against the deployed demo.

### 15.3 Breaking-change policy

Removing or renaming an operation or field, narrowing accepted input, changing response semantics, or adding a new required field is breaking. A breaking change requires a new major API version, published migration guidance, and deprecation notice on the prior version.

### 15.4 Deprecation window

A published API version MUST remain available for at least 90 days after deprecation notice before removal.

### 15.5 Auth and authorization

The baseline API is intentionally anonymous and has no user identity or authorization model. It MUST accept only public demo data, issue or accept a random non-identifying client token for a 60-request-per-minute allowance, exclude source IP and identifying token inputs from application telemetry, expose no administrative operation, and use workload identity for service-to-service Azure access.

### 15.6 Error contract

Every error MUST return the applicable HTTP status and `{error: {code: string, message: string, fields?: object, request_id: string}}`. Validation uses `400`, missing items use `404`, rate limiting uses `429`, and unavailable dependencies use `503`.

### 15.7 Backward-compatibility tests

N/A — first public iteration. CI MUST add one compatibility suite per retained prior major version in later iterations.

### 15.8 Delegated-mode disclosures

| # | Choice | Rationale |
| --- | --- | --- |
| A-1 | REST with OpenAPI 3.1 | Matches the small resource-oriented product and provides broadly supported machine-readable contracts. |
| A-2 | URI-path major versioning | Keeps versions visible to workshop participants and contract tests. |
| A-3 | Anonymous access with rate limiting | Supports a low-friction public demo while bounding abuse and prohibiting sensitive data. |
| A-4 | Uniform structured error envelope | Gives the browser UI and workshop evidence one deterministic failure contract. |
| A-5 | 90-day deprecation window | Provides a measurable compatibility commitment proportionate to a reusable demo API. |
