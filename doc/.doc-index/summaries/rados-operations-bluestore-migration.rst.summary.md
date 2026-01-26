This documentation file, `rados/operations/bluestore-migration.rst`, serves as the definitive guide for transitioning a Ceph cluster's underlying object storage implementation from the deprecated **Filestore** to the modern **BlueStore** backend.

### 1. Primary Purpose
The file provides high-level strategies and step-by-step operational procedures for migrating OSDs (Object Storage Daemons) from Filestore to BlueStore. Since in-place conversion is impossible due to fundamental architecture differences, this document outlines how to leverage Ceph's native replication and recovery mechanisms to perform the swap safely.

### 2. Key Topics Covered
*   **Deprecation Notice**: Formal warning that Filestore is unsupported as of the **Reef** release.
*   **Default Behavior**: Guidance on deploying new or replacement OSDs (which default to BlueStore).
*   **Migration Strategies**:
    *   **Mark-"out" Replacement**: A per-OSD approach using standard cluster healing. It is simple but causes high network traffic and double data migration.
    *   **"Whole host" Replacement**: A host-level approach using a spare host or evacuated capacity. This is more efficient as data moves across the network only once.
    *   **Per-OSD Device Copy**: A localized approach using `ceph-objectstore-tool` to copy data directly between disks on a single host. (Note: This is noted as partially unsupported/experimental).

### 3. Technical Keywords
*   **Components/APIs**: `BlueStore`, `Filestore`, `OSD`, `CRUSH map`, `LVM`.
*   **Commands**: 
    *   `ceph osd out` / `in`
    *   `ceph osd safe-to-destroy`
    *   `ceph-volume lvm create --bluestore`
    *   `ceph-volume lvm zap`
    *   `ceph osd crush swap-bucket`
    *   `ceph osd purge`
    *   `ceph-objectstore-tool`
*   **Configuration/Metadata**: `osd_objectstore`, `osd-id`, `osd_id_retrieval`.

### 4. Target Audience
*   **Storage Administrators**: Managing existing Ceph clusters that pre-date the BlueStore default.
*   **Operations Teams**: Planning hardware refreshes or software upgrades to Ceph Reef or later versions.
*   **DevOps Engineers**: Automating OSD lifecycle management.

### 5. Related Concepts
*   **RADOS Operations**: General cluster maintenance and OSD management.
*   **Data Durability/Redundancy**: Concepts of failure domains (host vs. rack) and replica health during migration.
*   **Deployment Tools**: Relates heavily to `ceph-volume` and its orchestration of physical storage into OSDs.
*   **Ceph Reef Release**: The version milestone that mandates this migration.

---

### Update Triggers for AI Maintenance
This file should be updated if any of the following code-level or architectural changes occur:
1.  **Command Deprecations**: If `ceph-volume` flags change (e.g., if `--bluestore` becomes implicit and the flag is removed).
2.  **Tooling Improvements**: If `ceph-objectstore-tool` or a new utility gains official support for "in-place" or "device-to-device" copying (addressing the current "unsupported" caveats).
3.  **Filestore Removal**: If the Filestore code is completely stripped from the codebase, the migration guide may need to transition to a legacy recovery guide or be archived.
4.  **Automation Changes**: If the `ceph-volume` LVM workflow changes how `osd-id` is reclaimed or how `zap` functions.
5.  **CRUSH Logic**: If new bucket-swapping capabilities are added to the `ceph` CLI that simplify the "Whole host" replacement logic.