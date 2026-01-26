This documentation file, `rados/operations/bluestore-migration.rst`, serves as the definitive guide for transitioning a Ceph cluster's underlying storage engine from the deprecated **Filestore** to the modern **BlueStore**.

### 1. Primary Purpose
The file documents the strategic and operational procedures for migrating OSDs (Object Storage Daemons) from Filestore to BlueStore. Since these two formats are fundamentally different, the file explains why in-place conversion is impossible and outlines three distinct methodologies to achieve the migration while maintaining data integrity and cluster availability.

### 2. Key Topics Covered
*   **Deprecation Notice**: Alerts users that Filestore is unsupported as of the Ceph **Reef** release.
*   **Migration Strategies**:
    *   **"Mark-out" Replacement**: A device-by-device approach using Ceph's native replication to move data off an OSD before reprovisioning it.
    *   **"Whole host" Replacement**: A more efficient strategy using a spare host or evacuated host to migrate data in large batches (host-by-host).
    *   **Per-OSD Device Copy**: A low-network-traffic method using `ceph-objectstore-tool` to copy data locally between disks.
*   **Operational Safety**: Detailed warnings regarding data destruction, CRUSH failure domains, and maintenance of data redundancy during the transition.
*   **Verification**: Procedures to identify current object store types across the cluster.

### 3. Technical Keywords
*   **Components/Tools**: `BlueStore`, `Filestore`, `OSD`, `ceph-volume`, `ceph-objectstore-tool`, `LVM`, `CRUSH map`.
*   **Commands/APIs**: 
    *   `ceph osd metadata` (to check storage type)
    *   `ceph osd out` / `ceph osd in`
    *   `ceph osd safe-to-destroy`
    *   `ceph-volume lvm zap` / `ceph-volume lvm create`
    *   `ceph osd crush swap-bucket`
    *   `ceph osd purge`
*   **Configuration/States**: `osd_objectstore`, `up`, `down`, `out`, `safe-to-destroy`.

### 4. Target Audience
*   **Ceph Administrators**: Those managing older clusters (pre-Reef) who need to upgrade to supported versions.
*   **System Architects**: Planning cluster expansions or hardware refreshes.
*   **Site Reliability Engineers (SREs)**: Automating the deprecation of legacy storage formats.

### 5. Related Concepts
*   **RADOS Operations**: General management of the Ceph storage pool.
*   **Data Durability/Redundancy**: Understanding how replication and erasure coding protect data during OSD downtime.
*   **CRUSH Hierarchy**: Managing how data is distributed across hosts, racks, and roots.
*   **LVM Management**: Since `ceph-volume` interacts directly with Logical Volume Manager for OSD provisioning.

---

### Update Triggers for AI Maintenance
An AI system should monitor and suggest updates to this file if the following code or environmental changes occur:
*   **Command Line Changes**: If `ceph-volume` or `ceph-objectstore-tool` deprecates existing flags (like `--bluestore`) or introduces new, more efficient migration sub-commands.
*   **Orchestration Evolution**: If `cephadm` or other orchestrators introduce automated "blue-green" OSD migration features that render manual "Mark-out" or "Whole host" steps obsolete.
*   **Release Lifecycle**: If new Ceph releases remove Filestore code entirely (making it impossible to even boot a Filestore OSD), the migration instructions must change to "offline recovery" only.
*   **Tooling Support**: If the "unsupported" `ceph-migrate-bluestore.bash` script is moved, promoted to a supported tool, or deprecated.
*   **Metadata Changes**: If the way Ceph reports `osd_objectstore` in its metadata JSON changes, the `grep` examples will need adjustment.