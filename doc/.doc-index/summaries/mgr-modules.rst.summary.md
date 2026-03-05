This documentation serves as the authoritative developer's guide for creating, installing, and maintaining **Ceph Manager (ceph-mgr) modules**. It provides the technical specifications for the Python-based framework that allows developers to extend Ceph's functionality.

### 1. Primary Purpose
The file documents the **API and lifecycle requirements** for `ceph-mgr` modules. It explains how to build Python classes that interface with the Ceph Manager daemon to monitor cluster state, handle CLI commands, manage configurations, and interact with the Ceph storage layer.

### 2. Key Topics Covered
*   **Module Structure**: Requirements for inheriting from `MgrModule` and the necessity of a `module.py` file.
*   **Lifecycle Methods**: Key overrides including `serve()` (blocking execution), `notify()` (state changes), and `shutdown()`.
*   **Command Exposure**: Two methods for adding CLI functionality: the modern `@CLICommand` decorator (with type hinting) and the legacy `COMMANDS` table.
*   **Logging & Debugging**: Managing log levels via Ceph config, logging to dedicated files, and using `ceph_mgr_repl.py` for interactive debugging.
*   **Configuration & Storage**: Using `MODULE_OPTIONS` for user-facing settings and the KV Store for persistent module-internal data.
*   **Cluster Interaction**: Methods for accessing OSD maps, health metrics, and direct RADOS/CephFS integration.
*   **High Availability**: Implementation of `StandbyModule` for non-active manager daemons.
*   **Advanced Formatting**: Use of the `object_format` module and `Responder` decorators for standardized JSON/YAML output.

### 3. Technical Keywords
*   **Classes/Inheritance**: `MgrModule`, `MgrStandbyModule`, `Orchestrator`, `SimpleDataProvider`.
*   **Decorators**: `@CLICommand`, `@CLIReadCommand`, `@CLIWriteCommand`, `@Responder`, `@ErrorResponseHandler`, `@EmptyResponder`.
*   **Configuration Types**: `uint`, `secs`, `millisecs`, `addr`, `uuid`, `size`.
*   **Core APIs**: `get_module_option`, `set_store`, `get_perf_schema`, `set_health_checks`, `send_command`, `self.rados`.
*   **Commands**: `ceph mgr module enable`, `ceph config set mgr/...`, `ceph config-key`.

### 4. Target Audience
*   **Ceph Core Developers**: Maintaining existing manager modules (e.g., dashboard, balancer).
*   **External Integration Developers**: Writing orchestrator modules or custom monitoring plugins.
*   **System Architects**: Designing extensions for Ceph cluster management.

### 5. Related Concepts
*   **C++ Ceph Context**: The underlying native layer the Python module interfaces with.
*   **RADOS**: The object store accessible via the module's `self.rados` attribute.
*   **Ceph CLI**: The primary consumer of the commands exposed by these modules.
*   **Monitors (MONs)**: The source of cluster maps and the backing store for configuration/KV data.

### When to Update this File
This documentation must be updated if any of the following code changes occur:
1.  **MgrModule Class Changes**: Modifications to the base `MgrModule` or `MgrStandbyModule` in `mgr_module.py`.
2.  **New Decorators**: Addition of new utilities in the `object_format` or CLI handling logic.
3.  **Config System Evolutions**: Changes to how `MODULE_OPTIONS` are parsed or new supported data types (e.g., adding a new unit to `secs`).
4.  **Notification Types**: Updates to `NOTIFY_TYPES` or the internal `should_notify` logic in the C++ manager.
5.  **Deprecations**: When existing methods (like the `COMMANDS` list approach) are officially deprecated or removed.
6.  **Tooling Changes**: Updates to the `ceph_mgr_repl.py` debugging utility or environment variable requirements.