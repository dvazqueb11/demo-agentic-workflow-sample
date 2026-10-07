# Project: Fast Self-Healing CI with GitHub Agentic Workflows

## What I Want To Build

### Background

This project demonstrates how GitHub Copilot Coding Agent and GitHub Agentic Workflows can enable self-healing CI/CD pipelines.

The goal is not to build a production-ready enterprise platform. The goal is to build a simple, understandable, and reusable reference implementation that demonstrates the pattern.

This implementation should complete in less than 10 minutes per workflow execution and be suitable for customer workshops, demonstrations, and proof-of-concept environments.

The design should intentionally avoid large chains of agents, excessive workflow orchestration, or long-running autonomous loops.

Instead, use:

- One deterministic CI workflow
- One bounded remediation agent
- Deterministic policy validation
- Deterministic post-remediation verification
- Human approval before merge

The architecture should demonstrate how the same pattern could later scale to:

- Coverity findings
- Static analysis remediation
- Synopsys toolchain validation
- Regression farms
- JFrog/Artifactory validation
- Internal engineering workflows

without implementing those integrations.

---

### Objective

Build a repository that demonstrates three self-healing scenarios.

The repository should contain:

- Small C++17 to-do application
- Lightweight browser UI
- Project-owned API
- Unit tests
- Coverage validation
- Lightweight performance benchmark
- GitHub Actions workflows
- GitHub Agentic Workflow
- Pull Request generation
- Escalation workflow when healing fails

The completed repository should be easy to understand and reusable as a template.

---

### Repository Theme

The application should be a regular, single-domain to-do app that customers can use while learning how agentic workflows and self-healing remediation pipelines operate.

Example functions:

- calculate_completion_rate()
- find_duplicate_todos()
- summarize_todos()

Users should be able to create, view, complete, and delete to-do items through a lightweight browser UI backed by a project-owned API.

The application itself is intentionally small and familiar. The focus remains the self-healing automation.

---

### Architecture Principles

#### Keep Agent Usage Minimal

Use a single remediation agent.

Do NOT create multiple agent roles such as:

- Diagnostician Agent
- Reviewer Agent
- Validator Agent
- Planner Agent
- Security Agent

These responsibilities should instead be handled through deterministic workflows whenever possible.

The only reasoning-heavy step should be remediation.

---

#### Fail Closed

If confidence is low:

- Do not modify code
- Do not retry repeatedly
- Create escalation issue
- Stop execution

---

#### Single Remediation Attempt

Each workflow run may perform:

- One diagnosis
- One remediation proposal
- One validation

If validation fails:

Create escalation issue.

Do not attempt another fix.

---

#### Human Approval Required

The workflow may:

- Create branch
- Create pull request

The workflow may NOT:

- Auto merge
- Push to main
- Override branch protection

A human reviewer must approve any change.

---

### Scenario 1: Easy

#### Self-Healing Unit Test Failure

##### Description

Introduce a deterministic defect inside:

calculate_completion_rate()

The defect should cause one unit test failure.

##### Workflow

1. CI runs.
2. Unit test fails.
3. Failure is classified.
4. Policy gate approves remediation.
5. Agent receives:
   - Failing test
   - Relevant source file
   - Relevant test file
   - Failure logs
6. Agent proposes minimal fix.
7. Validation runs.
8. Pull Request created.

##### Restrictions

The agent may:

- Modify application code

The agent may NOT:

- Delete tests
- Disable tests
- Skip tests
- Modify workflow configuration

##### Expected Outcome

A healing PR fixes the bug and all tests pass.

---

### Scenario 2: Medium

#### Self-Healing Test Coverage

##### Description

Introduce a commit that adds new functionality to:

summarize_todos()

without adding sufficient unit tests.

Coverage should intentionally fall below a configured threshold.

##### Workflow

1. CI runs.
2. Coverage gate fails.
3. Coverage analyzer identifies uncovered lines.
4. Policy gate allows changes only in tests.
5. Agent generates meaningful tests.
6. Validation reruns:
   - unit tests
   - coverage checks
7. Pull Request created.

##### Restrictions

The agent may:

- Add tests

The agent may NOT:

- Modify production code
- Lower coverage thresholds
- Exclude files from coverage
- Add trivial tests with meaningless assertions

##### Expected Outcome

A healing PR adds useful tests and restores coverage compliance.

---

### Scenario 3: Advanced

#### Self-Healing Performance Regression

##### Description

Introduce a deliberate regression in:

find_duplicate_todos()

using an inefficient nested-loop implementation.

A performance benchmark should fail when execution time exceeds a configured threshold.

##### Workflow

1. CI runs.
2. Benchmark fails.
3. Policy classifies issue as performance regression.
4. Agent receives:
   - Benchmark results
   - Relevant implementation
   - Associated tests
   - Triggering source changes
5. Agent optimizes implementation.
6. Validation reruns:
   - Unit tests
   - Benchmark
7. Pull Request created.

##### Restrictions

The agent may:

- Modify implementation associated with find_duplicate_todos()

The agent may NOT:

- Raise benchmark thresholds
- Reduce dataset sizes
- Disable benchmark execution
- Add unsafe concurrency
- Remove validations

##### Expected Outcome

A healing PR restores performance while preserving correctness.

---

### Workflow Design

#### Workflow 1

ci.yml

Responsibilities:

- Build
- Test
- Coverage analysis
- Performance validation
- Create normalized failure bundle
- Trigger self-healing workflow

---

#### Workflow 2

self-heal.yml

Responsibilities:

##### Step 1

Classify failure

Categories:

- Unit Test
- Coverage
- Performance
- Unsupported

##### Step 2

Policy Evaluation

Verify:

- Allowed file paths
- Allowed actions
- Scenario-specific restrictions

##### Step 3

Remediation Agent

Provide only:

- Failure evidence
- Relevant files
- Scenario instructions

Do not provide the entire repository unless necessary.

##### Step 4

Validation

Run:

- Build
- Tests
- Coverage
- Benchmark

as appropriate.

##### Step 5

Outcome

If validation passes:

- Create PR

If validation fails:

- Create escalation issue

---

### Runtime Requirements

Design for speed.

Target:

| Stage | Target |
|---------|---------|
| Checkout + Setup | < 1 min |
| Build + Validation | < 2 min |
| Agent Remediation | < 4 min |
| Revalidation | < 2 min |
| PR Creation | < 1 min |

Total objective:

Less than 10 minutes.

---

### Extension Points

Design interfaces that could later support:

#### Static Analysis Providers

Examples:

- Coverity
- SonarQube

#### Artifact Providers

Examples:

- JFrog
- Artifactory

#### Work Item Providers

Examples:

- Jira
- Azure DevOps

#### Validation Providers

Examples:

- Regression Farms
- EDA Validation Pipelines
- Verdi Verification Workflows

Do not implement these integrations.

Create extension points only.

---

### Deliverables

Implement:

- C++17 to-do application
- Lightweight browser UI
- Versioned API contract and implementation
- Tests
- Coverage validation
- Performance benchmark
- CI workflow
- Self-healing workflow
- Policy engine
- Remediation instructions
- Escalation flow
- Pull Request generation
- Demo scenarios
- Documentation

---

### Adopted Engineering Standards

- Use least-privilege GitHub permissions and pin third-party GitHub Actions to immutable revisions.
- Use no long-lived credentials; prefer short-lived identity federation for any cloud integration.
- Require human-reviewed pull requests and never auto-merge remediation changes.
- Permit one bounded remediation attempt, fail closed on uncertainty, and create an escalation issue.
- Build the C++17 sample with CMake and run tests through CTest.
- Keep classification, policy evaluation, and post-remediation validation deterministic.
- Emit an auditable, normalized failure bundle containing only the evidence needed for remediation.

### Mermaid Diagram

Include architecture documentation similar to:

CI Failure
→ Classification
→ Policy Gate
→ Remediation Agent
→ Validation
→ Pull Request

or

CI Failure
→ Classification
→ Policy Gate
→ Escalation Issue

---

## Done Looks Like

### Acceptance Criteria

1. All three scenarios can be executed independently.
2. Healing PRs are generated for successful remediations.
3. Failed remediations create escalation issues.
4. Human approval is always required.
5. No auto-merge exists.
6. No recursive remediation loops exist.
7. No direct pushes to main exist.
8. The solution remains understandable and reusable.
9. The workflow completes within the target runtime budget.
10. Future integrations can be added through extension points without redesigning the architecture.