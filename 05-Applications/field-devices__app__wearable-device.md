---
uid: field-devices__app__wearable-device
type: spec
domain: field-devices
status: active
owner: auto-generated
created: 2026-02-07
updated: 2026-02-07
version: "1.0.0"
tags: [embedded, ble, firmware, renesas]
---

# App Reference WD

## Analysis
The Wearable Device (WD) application serves as the reference implementation for a complex, multi-sensor field device in this monorepo.

### Application Logic
- **Data Packages (DP)**: The WD app manages several logical data streams:
    - `DP_SYSTEM_DATA`: Device health and status.
    - `DP_USER_DATA`: User-specific interactions.
    - `DP_LOCATION_DATA`: Proximity and location tracking.
    - `DP_SANITIZATION_DATA`: Specific use-case data (e.g., hand hygiene).
    - `DP_CONTACT_DATA`: Contact tracing or peer-to-peer interaction logs.
- **Routines**: It implements `user_hy_routine` (Hybrid) and `user_lo_routine` (Low-power), which define how the device behaves in different activity states.
- **Sensor Integration**: Integrates the LSM6DSOX IMU for activity tracking and an nPM1300 for power/LED management.

### Data Flow
1. **Acquisition**: Sensors (IMU, Battery) are polled or trigger interrupts.
2. **Processing**: Data is formatted into the relevant Data Package (DP).
3. **Storage**: DPs are temporarily stored in retention RAM or moved to external Flash via [[field-devices__middleware__memory-helpers|Memory Helpers]].
4. **Transmission**: When a BLE connection is established, the DAS service ([[field-devices__middleware__ble-utils|BLE Utils]]) transmits the accumulated DPs to the peer.

### Diagrams
## WD Component Tree
```mermaid
graph TD
    WD[WD App] --> Sensors[Sensors Module]
    WD --> Logic[Business Logic]
    WD --> Comms[BLE Comms]
    
    Sensors --> LSM[LSM6DSOX]
    Sensors --> BATT[Battery/PMIC]
    
    Logic --> HY[Hybrid Routine]
    Logic --> LO[Low Power Routine]
    Logic --> DPS[Data Package System]
    
    Comms --> DAS[DAS Service]
```

## User Interaction Flow
```mermaid
sequenceDiagram
    participant User
    participant IMU as LSM6DSOX
    participant App as WD App
    participant BLE as BLE Stack

    User->>IMU: Movement (Pickup)
    IMU->>App: Wakeup Interrupt
    App->>App: Transition to HY Routine
    App->>App: Start Data Logging (DP)
    User->>BLE: Connect via Phone
    BLE->>App: Connection Event
    App->>BLE: Flush DP via DAS
```

## Findings
- **High Complexity**: The WD app is the most feature-rich, utilizing almost all available modules.
- **Persistence**: Systematic use of retention memory ensures that no data is lost during the frequent sleep/wake cycles required for long battery life.
- **Modularity**: The use of independent Data Packages allows for granular data management and transmission.

## Dependencies
- [[field-devices__middleware__ble-utils|uses]]
- [[field-devices__middleware__memory-helpers|uses]]
- [[field-devices__app__smartband|consumes]]

## Next Steps
Compare this reference implementation with the specialized Smartband (SP) application in [[field-devices__app__smartband|Smartband]].
