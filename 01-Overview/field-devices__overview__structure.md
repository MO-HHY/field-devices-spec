---
uid: field-devices__overview__structure
type: overview
domain: field-devices
status: active
owner: auto-generated
created: 2026-02-07
updated: 2026-02-07
version: "1.0.0"
tags: [embedded, ble, firmware, renesas]
---

# Structure

## Analysis
The `field-devices` repository is structured as a layered monorepo. This allows for code reuse across different device types while maintaining specific application logic separate.

### Directory Mapping
- **`apps/`**: Each subdirectory represents a specific device firmware.
    - `bc/`: Basic Controller?
    - `dc/`: Data Collector?
    - `sp/`: Sensor Probe?
    - `ta/`, `tl/`, `wd/`: Other variants.
    - Each app has a `src/` directory and `envs/` (e.g., Keil uVision projects).
- **`drivers/`**: Contains `standard/` drivers, presumably for peripherals.
- **`modules/`**: Contains `api/` and `src/`. This layer likely implements business logic shared across apps.
- **`sdk/`**: Contains `core/` and `third_party/`. 
- **`tools/`**: Contains `diactl/`, a custom tool for interacting with or building the firmware.

### Diagrams
## Repository Structure Map
```mermaid
graph TD
    Root[/] --> Apps[apps/]
    Root --> Drivers[drivers/]
    Root --> Modules[modules/]
    Root --> SDK[sdk/]
    Root --> Tools[tools/]
    Root --> Include[include/]

    Apps --> BC[bc/]
    Apps --> DC[dc/]
    Apps --> SP[sp/]
    Apps --> TA[ta/]
    Apps --> TL[tl/]
    Apps --> WD[wd/]

    Drivers --> Standard[standard/]

    Modules --> API[api/]
    Modules --> ModSrc[src/]

    SDK --> Core[core/]
    SDK --> ThirdParty[third_party/]

    Tools --> DiaCtl[diactl/]
```

## Findings
- High modularity: Separation of concerns between HAL (drivers), SDK, business logic (modules), and final applications (apps).
- Multi-target build support: Multiple apps sharing the same underlying modules and SDK.
- Tooling integration: Custom `diactl` tool suggests a managed build/deployment process.

## Dependencies
- [[field-devices__foundation__sdk-analysis|uses]]
- [[field-devices__foundation__build-system|uses]]
- [[field-devices__foundation__diactl-tool|uses]]

## Next Steps
Proceed to Layer 1 nodes: [[field-devices__foundation__sdk-analysis|SDK Analysis]], [[field-devices__foundation__build-system|Build System]], and [[field-devices__foundation__diactl-tool|Diactl tool]] to understand how these layers are integrated.
