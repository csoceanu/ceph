This documentation file serves as the definitive guide for managing **Object Storage Device (OSD) Services** within a Ceph cluster using the `cephadm` orchestrator.

### 1. Primary Purpose
The file documents the lifecycle of OSDs—from hardware discovery and inventory scanning to deployment, automated memory tuning, and removal. It focuses on the **declarative nature of `cephadm`**, where the user defines a desired state (via OSD Specs) and the orchestrator ensures the physical hardware matches that state.

### 2. Key Topics Covered
*   **Device Inventory**: How Ceph scans hosts to identify eligible storage devices.
*   **Hardware Monitoring Integration**: Using `libstoragemgmt` to pull health data and control drive LEDs (Ident/Fault).
*   **OSD Deployment**: Strategies for adding OSDs, including "all-available-devices" and host-specific additions.
*   **OSD Specs (DriveGroups)**: Advanced YAML-based configurations to target specific hardware based on vendor, model, size, or rotational attributes.
*   **OSD Removal & Replacement**: The workflow for draining data (PGs), zapping drives, and replacing failed units while preserving OSD IDs.
*   **Memory Autotuning**: How `cephadm` dynamically calculates and sets `osd_memory_target`.
*   **Disaster Recovery**: Procedures for activating existing OSDs after a host OS reinstallation.

### 3. Technical Keywords
*   **Commands**: `ceph orch device ls`, `ceph orch apply osd`, `ceph orch osd rm`, `ceph orch device zap`, `ceph cephadm osd activate`.
*   **Configuration Options**: `mgr/cephadm/device_enhanced_scan`, `osd_memory_target_autotune`, `mgr/cephadm/autotune_memory_target_ratio`.
*   **APIs/Tools**: `ceph-volume`, `libstoragemgmt` (LSM), `LVM`, `BlueStore`, `TPM2` (for LUKS2 encryption).
*   **OSD Spec Fields**: `data_devices`, `db_devices`, `wal_devices`, `db_slots`, `osds_per_device`, `crush_device_class`.

### 4. Target Audience
*   **Ceph Cluster Administrators**: Responsible for scaling storage and replacing hardware.
*   **System Architects**: Designing hardware layouts and storage tiering (e.g., SSD for WAL/DB, HDD for Data).
*   **Site Reliability Engineers (SREs)**: Automating cluster deployment and managing resource limits (RAM).

### 5. Related Concepts
*   **Cephadm Orchestrator**: The underlying framework managing these services.
*   **CRUSH Hierarchy**: How OSDs are organized for data placement.
*   **Placement Groups (PGs)**: The logical units moved during OSD removal.
*   **Host Management**: SSH keys and container registry logins required for `cephadm` to function.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **Orchestrator CLI**: Changes to `ceph orch osd` or `ceph orch device` subcommands or flags.
2.  **Ceph-Volume Logic**: If the criteria for what constitutes an "available" device changes (e.g., new partition types or filesystem detection).
3.  **OSD Spec Schema**: If new filters are added to the `DriveGroupSpec` (e.g., targeting specific bus types or NVMe namespaces).
4.  **Hardware Support**: If `libstoragemgmt` integration is expanded to support NVMe or if encryption standards (like TPM2) are updated.
5.  **Memory Management**: If the algorithm for `osd_memory_target_autotune` is modified in the `mgr/cephadm` module.