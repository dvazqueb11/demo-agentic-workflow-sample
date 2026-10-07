# Agentic stack binding — GitHub Agentic Workflows with Copilot

**Category:** `agentic-stack` · **Status:** project override

## Stack mapping

| Capability | Choice |
| --- | --- |
| Agent framework | GitHub Agentic Workflows Markdown source compiled to locked GitHub Actions workflows |
| Agent engine | GitHub Copilot coding agent |
| Orchestration | One remediation agent per run; deterministic Actions jobs own classification, policy, and validation |
| Tool ecosystem | GitHub Agentic Workflow safe outputs plus repository-local `request_context` and `propose_patch` contracts |
| Evaluation | Repository eval harness with three gate-time runs and a 2-of-3 pass rule |
| Tracing and cost | GitHub Agentic Workflow audit logs, token-usage artifact, raw token counts, AI credits, and OpenTelemetry-compatible normalized events |
| Guardrails | Least-privilege workflow permissions, safe outputs, fixed writable paths, one proposal, branch protection, and mandatory human PR review |

## Contract mapping

| Required control | Implementation | Escalate if |
| --- | --- | --- |
| Reviewed merge | Agent creates an isolated-branch pull request; protected `main` requires human review | The platform can merge without the required human review |
| Independent deterministic validation | Non-agent GitHub Actions jobs rerun scenario and shared gates after the proposal | Validation cannot run independently of the agent |
| Bounded tools | Safe outputs and repository policy permit only reasoned read expansion and one patch proposal | The engine can write outside policy or invoke remediation twice |
| Cost metering | Structured agentic-workflow logs expose input/output token counts and AI-credit cost estimates | Machine-readable usage cannot be exported to `reports/eval/usage.json` |
| Fail-closed escalation | Unsupported or failed runs create/update a deduplicated GitHub issue or retain a downloadable artifact | Neither issue nor artifact can be produced |

## Currency evidence

Verified at PLAN time against current official GitHub Agentic Workflows documentation for workflow creation, permissions, Copilot engine usage, safe outputs, and cost management. External documentation is technology evidence only; the approved Spec remains authoritative.
