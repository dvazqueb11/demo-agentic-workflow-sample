# TWTTY runtime config — demo-agentic-workflow-sample

This folder holds this project's explicit TWTTY capability-binding overrides and runtime selections. It is committed with the project so each run can resolve the same effective controls without changing the shared methodology.

## Resolution rule

- A setting present in `runtimeconfig.md` overrides or records a project choice.
- A setting absent from `runtimeconfig.md` inherits the active specialization's shipped `default-config.md` chain.
- The `specialization` field selects that inheritance chain.
- File-backed overrides belong in matching subfolders under this directory and use paths relative to this directory.

## This project's settings

- Specialization: `sdlc-for-agentic-apps`.
- Override: escalation uses GitHub Issues instead of the baseline PR-comment default.
- Runtime selections: risk level `L2`; runtime target `azure`.
- All other bindings inherit shipped defaults, including the GitHub Copilot harness, GitHub devtools, Azure cloud provider, default risk ladder and agentic overlay, policy and best-practices profiles, UX profile, reusable skills, build tokenomics, product tokenomics schema, and agentic stack.

## How to change a binding

1. Add a provider or profile file under the matching subfolder here.
2. Reference it from `runtimeconfig.md` under the appropriate specialization override heading.
3. Commit the change.
4. Re-run the Discovery config interview so the effective bindings are validated and pinned in a new `meta/config` replay entry.

Never place secrets, tokens, connection strings, or personal data in this folder.
