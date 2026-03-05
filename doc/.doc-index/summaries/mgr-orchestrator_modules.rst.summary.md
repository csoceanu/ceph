This documentation provides a technical overview of the **Ceph Manager (ceph-mgr) Orchestrator Framework**. It defines the internal API and structural requirements for modules that bridge Ceph management commands with external deployment tools (like Rook or cephadm).

### 1. Primary Purpose
The file serves as a **developer guide** for creating and maintaining `ceph-mgr` orchestrator modules. It defines a common interface (`Orchestrator` class) so that higher-level components—such as the Ceph Dashboard or CLI—can manage hardware, services, and daemons consistently, regardless of the underlying deployment backend.

### 2. Key Topics Covered
*   **Orchestrator Interface**: The use of abstract base classes to ensure different backends (Rook, cephadm) support a unified set of operations.
*   **Service Classification**: Distinction between *Stateful* services (OSD, Mon) and *Stateless* services (MDS, RGW, NFS).
*   **Resource Management**: Mechanisms for host discovery, inventory (device) scanning, and host labeling.
*   **Operational Patterns**:
    *   **Completions/Batching**: Handling long-running asynchronous operations.
    *   **Placement**: Defining where daemons should run using `PlacementSpec`.
    *   **OSD Lifecycle**: Two-phase replacement process for physical storage devices.
*   **Error Handling**: Standardized exception classes for validation, missing features, or execution failures.
*   **Scope Boundaries**: Explicitly states what the orchestrator does *not* do (e.g., host bootstrapping, multipath management).

### 3. Technical Keywords
*   **Core Classes**: `Orchestrator`, `OrchestratorError`, `Completion`, `HostSpec`, `ServiceSpec`, `PlacementSpec`, `DriveGroupSpec`.
*   **Methods/APIs**: `add_host`, `get_inventory`, `create_osds`, `apply_mon`, `apply_rgw`, `list_daemons`, `upgrade_start`.
*   **Ceph Components**: `ceph-mgr`, `OSD`, `MDS`, `RGW`, `nfs-ganesha`, `rbd-mirror`.
*   **Backends**: `Rook`, `cephadm`.
*   **Data Structures**: `Drive Group`, `Labels`, `InventoryFilter`.

### 4. Target Audience
*   **Ceph Internal Developers**: Specifically those writing or debugging `ceph-mgr` modules.
*   **Orchestrator Maintainers**: Developers integrating external tools (like Kubernetes/Rook) into the Ceph ecosystem.

### 5. Related Concepts
*   **`mgr-module-dev`**: The general documentation for developing Ceph Manager modules.
*   **`orchestrator-cli`**: The command-line interface that consumes this API.
*   **`ceph-mgr-dashboard`**: A primary consumer of the orchestrator interface for GUI-based management.
*   **`Drive Groups`**: The declarative specification used to define how OSDs should be created across disks.

---

### Triggering Updates: When to modify this file
This file must be updated if any of the following code changes occur:
1.  **API Surface Changes**: Adding, renaming, or removing methods in the `Orchestrator` base class (e.g., adding support for a new service type like `apply_iscsi`).
2.  **Logic Shifts**: Changes to how Ceph handles asynchronous "Completions" or background processing logic.
3.  **New Data Models**: Introduction of new specification objects (e.g., a new field in `PlacementSpec` or `ServiceSpec`).
4.  **Error Handling Policies**: Changes to the hierarchy of `OrchestratorError` or how validation failures are expected to be reported.
5.  **Glossary/Definitions**: If the fundamental definition of a "Stateful Service" or "Drive Group" evolves within the Ceph architecture.