---
uid: field-devices__foundation__sdk-analysis
type: spec
domain: field-devices
status: active
owner: auto-generated
created: 2026-02-07
updated: 2026-02-07
version: "1.0.0"
tags: [embedded, ble, firmware, renesas]
---

# SDK Analysis

## Analysis
The repository integrates the Dialog Semiconductor (now Renesas) SmartBond™ SDK for the DA14531/DA1458x family of SoCs.

### SDK Details
- **Version**: 6.22.1401 (confirmed by `sdk/version.txt`).
- **Target Architecture**: ARM Cortex-M0+.
- **Components**:
    - `sdk/core/platform/`: Low-level drivers (LLD), boot sequence, and CMSIS files.
    - `sdk/core/ble_stack/`: The proprietary Bluetooth Low Energy stack implementation.
    - `sdk/core/app_modules/`: SDK-provided application-level modules (e.g., battery service, device information service).
    - `sdk/third_party/`: External libraries like `micro_ecc`, `crc32`, and random number generators.

### Integration Points
- **Startup**: Files in `sdk/core/platform/arch/boot/` handle the reset vector and system initialization before jumping to `main`.
- **Configuration**: Managed via `da1458x_config_*.h` files, which are often included globally via compiler flags.

### Diagrams
## Architecture Overview
```mermaid
graph TD
    App[Application Code] --> Modules[Shared Modules]
    Modules --> SDK_App[SDK App Modules]
    SDK_App --> BLE[BLE Stack]
    BLE --> Platform[Platform / LLD]
    Platform --> HW[DA14531 Hardware]
```

## Tech Stack Layers
```mermaid
graph LR
    User[User Logic] --- API[Module API]
    API --- SDK_Core[SDK Core]
    SDK_Core --- HAL[Hardware Abstraction]
    HAL --- Silicon[Silicon]
```

## Findings
- The SDK is integrated as a local copy in the `sdk/` directory, ensuring build reproducibility.
- Heavy reliance on Dialog's proprietary architectural patterns (e.g., asynchronous task messaging).
- Well-organized separation between core SDK components and third-party utilities.

## Dependencies
- [[field-devices__foundation__build-system|depends_on]]

## Next Steps
Analyze the build system ([[field-devices__foundation__build-system|Build System]]) to see how these SDK components are compiled and linked.
