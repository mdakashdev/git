```mermaid
stateDiagram-v2
[*] --> Pending
Pending --> Approved
Approved --> Completed
Completed --> [*]
```