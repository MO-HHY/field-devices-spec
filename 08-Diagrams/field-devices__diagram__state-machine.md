---
uid: field-devices__diagram__state-machine
type: diagram
domain: field-devices
diagram_type: stateDiagram-v2
status: active
created: 2026-02-08
version: "1.0.0"
tags: [diagram, mermaid, state-machine, device]
---

# Device State Machine

```mermaid
stateDiagram-v2
    [*] --> PowerOn
    PowerOn --> Init: user_app_init()
    Init --> Advertising: Configuration Complete
    
    Advertising --> Sleep: Advertising Timeout
    Sleep --> Advertising: Wakeup (Motion/Button)
    
    Advertising --> Connected: BLE Connection
    Connected --> Advertising: Disconnected
    
    Connected --> Authenticated: DAS Auth Success
    Authenticated --> Busy: Start Data Task
    Busy --> Authenticated: Task Completed / Timeout
    
    Authenticated --> Advertising: Disconnected
    
    state Busy {
        TASK_IDLE --> TASK_RUNNING
        TASK_RUNNING --> TASK_RUNNING: mutate()
        TASK_RUNNING --> TASK_SUCCEEDED
        TASK_RUNNING --> TASK_FAILED
    }
```
