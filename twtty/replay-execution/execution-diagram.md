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
    B["008 spec/1b<br/>Business requirements approved<br/>Interactive"]
    U["009 spec/1c<br/>Use cases approved<br/>Interactive"]
    X["010 spec/1d<br/>SPEC-EXIT approved<br/>Interactive"]
    P["011 meta/autopilot-enable<br/>PLAN and EXECUTE<br/>Human anchor verified"]
    S1["012 meta/skill-install<br/>Impeccable installed<br/>Human approved"]
    C1["013 meta/config<br/>GitHub agentic stack<br/>Autopilot"]
    N["Next: PLAN mode offer and plan/2a"]

    R --> S --> C --> A --> I --> G --> D --> B --> U --> X --> P --> S1 --> C1 --> N
```
