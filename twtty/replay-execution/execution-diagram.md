# Baseline Execution Diagram

[Replay-execution log](./execution-log.md)

```mermaid
flowchart LR
    R["001 meta/risk-level<br/>L2 approved<br/>Interactive"]
    S["002 seed/0a<br/>SEED-EXIT approved<br/>Interactive"]
    C["003 seed/0a<br/>Corrective SEED-EXIT approved<br/>Interactive"]
    A["004 meta/autopilot-enable<br/>Approved<br/>Human anchor pending"]
    I["005 meta/mode-change<br/>Interactive selected<br/>Autopilot never activated"]
    G["006 meta/config<br/>Config resolved<br/>Interactive"]
    D["007 spec/1a<br/>End of discovery<br/>Interactive"]
    N["Next: SPEC / spec/1b"]

    R --> S --> C --> A --> I --> G --> D --> N
```
