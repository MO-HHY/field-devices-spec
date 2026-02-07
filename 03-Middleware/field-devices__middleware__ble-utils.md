---
uid: field-devices__middleware__ble-utils
type: spec
domain: field-devices
status: active
owner: auto-generated
created: 2026-02-07
updated: 2026-02-07
version: "1.0.0"
tags: [embedded, ble, firmware, renesas]
---

# BLE Utils

## Analysis
The BLE abstraction layer facilitates high-level communication by wrapping the Dialog SDK's low-level BLE stack. The primary implementation is the Data Acquisition Service (DAS).

### Data Acquisition Service (DAS)
- **Purpose**: Provides a robust protocol for bi-directional data transfer between the field device and a mobile app or gateway.
- **Fragmentation**: Large data blocks are split into chunks (`user_das_transmit_dp_chunk`). The chunk size is negotiated based on the MTU (`user_ble_env[conidx].packet_size`).
- **Operation Handlers**: DAS uses a table of operations (`user_das_operations_definition`) to map command codes to specific C handlers.
- **State Integration**: Like the Task Manager, DAS stores its transfer environment in retention memory, allowing large transfers to continue across connection intervals and sleep modes.

### Authentication Logic
The service includes an authentication mechanism (`user_das_auth_req_handler`) that uses a challenge-response pattern or a specific evaluation function (`user_run_eval_func`) to verify the peer.

### Diagrams
## BLE Request Lifecycle
```mermaid
sequenceDiagram
    participant Peer as BLE Peer (App)
    participant SDK as SDK BLE Stack
    participant CUSTS1 as Custom Service 1
    participant DAS as DAS Module
    participant App as App Logic

    Peer->>SDK: Write Characteristic (Request)
    SDK->>CUSTS1: GATTC_WRITE_REQ_IND
    CUSTS1->>DAS: user_das_request_ind()
    DAS->>DAS: Look up operation
    DAS->>App: Callback / Dispatch Event
    App-->>DAS: Operation result
    DAS->>CUSTS1: user_custs1_svc1_send_ntf()
    CUSTS1->>SDK: GATTC_SEND_EVT_CMD
    SDK-->>Peer: Notification (Response)
```

## Findings
- **Robust Protocol**: The chunking mechanism ensures that memory constraints do not limit the size of transferable data.
- **Modular Design**: Adding new BLE commands only requires adding an entry to the operation table.
- **Security**: The presence of an `auth` flag in `user_ble_env` suggests a multi-level access control system.

## Dependencies
- [[field-devices__driver__peripheral-drivers|uses]]

## Next Steps
Investigate the peripheral drivers (Layer 3, [[field-devices__driver__peripheral-drivers|Peripheral Drivers]]) which provide the data that DAS eventually transmits.
