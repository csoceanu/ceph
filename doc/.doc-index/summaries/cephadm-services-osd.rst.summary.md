This documentation file serves as the definitive guide for managing **Object Storage Devices (OSDs)** using the `cephadm` orchestrator. It covers the entire lifecycle of an OSD, from hardware discovery and deployment to monitoring, tuning, and removal.

### 1. Primary Purpose
The file documents how `cephadm` interacts with physical storage hardware to create, manage, and decommission Ceph OSD services. It explains how to automate OSD deployment at scale using declarative specifications rather than manual disk-by-disk configuration.

### 2. Key Topics Covered
*   **Device Inventory**: Procedures for scanning hosts to identify available storage media and checking hardware health/LED status via `libstoragemgmt`.
*   **OSD Deployment**: Methods for adding OSDs, ranging from simple one-line commands (using all available devices) to complex YAML-based "Drive Groups."
*   **Declarative State**: Understanding how `cephadm` continuously reconciles the desired state, automatically turning new or wiped disks into OSDs.
*   **OSD Removal & Replacement**: Safe workflows for draining data (PGs) from an OSD, stopping/starting the removal process, and replacing failed hardware while preserving OSD IDs.
*   **Memory Autotuning**: How the orchestrator dynamically allocates RAM to OSD daemons based on total system memory and OSD count.
*   **Advanced Filtering**: Using attributes like vendor, model, size, and rotational status to target specific disks for data, WAL, or DB roles.
*   **OSD Activation**: Reconnecting existing OSDs to a cluster after a host OS reinstallation.

### 3. Technical Keywords
*   **Orchestrator Commands**: `ceph orch device ls`, `ceph orch apply osd`, `ceph orch daemon add osd`, `ceph orch osd rm`, `ceph orch device zap`.
*   **Configuration Options**: `mgr/cephadm/device_enhanced_scan`, `osd_memory_target_autotune`, `mgr/cephadm/autotune_memory_target_ratio`.
*   **Components/Tools**: `ceph-volume`, `libstoragemgmt` (`lsmcli`), `BlueStore`, `LVM`, `LUKS2`, `TPM2`.
*   **OSD Spec Fields**: `data_devices`, `db_devices`, `wal_devices`, `db_slots`, `osds_per_device`, `crush_device_class`.
*   **Logic**: `unmanaged: true`, `host_pattern`, `filter_logic: OR/AND`.

### 4. Target Audience
*   **Ceph Administrators**: Managing storage clusters and scaling capacity.
*   **Site Reliability Engineers (SREs)**: Automating hardware provisioning and handling disk failures.
*   **Storage Architects**: Designing the physical layout and performance tiers (HDD vs. SSD vs. NVMe) of a cluster.

### 5. Related Concepts
*   **Cephadm Orchestrator**: The underlying framework for managing Ceph services.
*   **Drive Groups / OSDSpecs**: The high-level abstraction for hardware-to-OSD mapping.
*   **CRUSH Hierarchy**: How OSDs are placed within failure domains (Host, Rack, etc.).
*   **Placement Groups (PGs)**: The data units that must be "drained" before an OSD can be safely removed.

---

### Update Triggers for AI Maintenance
This file should be updated if code changes occur in the following areas:
1.  **Orchestrator CLI**: If subcommands under `ceph orch osd` or `ceph orch device` are added, renamed, or deprecated.
2.  **Ceph-Volume Evolution**: If the criteria for what constitutes an "available" device change (e.g., support for new filesystem types or partition schemes).
3.  **OSDSpec Schema**: If new filters are added to the `DriveGroupSpec` Python class (e.g., filtering by firmware version or bus type).
4.  **Hardware Support**: If `libstoragemgmt` integration is expanded to include NVMe or SAN LUNs.
5.  **Security Features**: If new encryption or authentication methods (like further TPM2/LUKS enhancements) are implemented.
6.  **Memory Management**: If the algorithm for `osd_memory_target` calculation is modified in the `mgr` daemon.