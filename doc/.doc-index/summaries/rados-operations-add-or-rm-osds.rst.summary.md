This documentation file serves as the definitive operational guide for the manual lifecycle management of **Object Storage Daemons (OSDs)** within a Ceph cluster. It provides the procedural steps required to scale a cluster’s capacity up or down and replace failing hardware.

### 1. Primary Purpose
The file documents the standard operating procedures for **adding, removing, and replacing OSDs** in an existing RADOS cluster. It focuses on manual workflows, hardware preparation, and ensuring data integrity during rebalancing.

### 2. Key Topics Covered
*   **Expansion (Adding OSDs):** Hardware deployment, software installation, manual OSD creation, and integration into the CRUSH hierarchy.
*   **Replacement (Hardware Failure):** Safety checks (`safe-to-destroy`), destroying OSD IDs while preserving their place in the map, and zapping/re-preparing disks using `ceph-volume`.
*   **Decommissioning (Removing OSDs):** Taking OSDs "out" to trigger data migration, stopping daemons, and purging OSD metadata from the cluster.
*   **Data Rebalancing:** Monitoring placement group (PG) migration states (e.g., `active+clean` to `active+degraded`).
*   **Operational Safety:** Managing capacity thresholds (`near full` and `full` ratios) to prevent cluster deadlocks during maintenance.

### 3. Technical Keywords
*   **Daemons/Services:** `ceph-osd`, `systemctl`, `ceph-volume`
*   **Commands:** 
    *   `ceph osd create`, `ceph osd destroy`, `ceph osd purge`
    *   `ceph osd out` / `ceph osd in`
    *   `ceph osd crush add` / `reweight` / `remove`
    *   `ceph-volume lvm (zap, prepare, activate, create)`
*   **Configuration & Auth:** `ceph.conf`, `ceph auth add/del`, `keyring`, `UUID`, `FSID`
*   **Internal Concepts:** CRUSH map, Buckets, Placement Groups (PGs), Weight vs. Reweight, `safe-to-destroy`.

### 4. Target Audience
*   **Storage Administrators:** For day-to-day cluster maintenance and scaling.
*   **Site Reliability Engineers (SREs):** For troubleshooting hardware failures and managing capacity.
*   **DevOps Engineers:** For understanding the manual steps required to automate OSD deployments via scripts or configuration management.

### 5. Related Concepts
*   **CRUSH Maps:** The underlying algorithm that determines data placement; adding/removing OSDs directly modifies this map.
*   **Monitoring:** Using `ceph -w` or `ceph status` to track cluster health during OSD transitions.
*   **Hardware Recommendations:** Minimum specs for OSD performance and resilience.
*   **Cephadm:** The modern orchestration tool (this document provides the manual alternative to `cephadm` workflows).

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **CLI Syntax Changes:** Any modifications to `ceph osd` subcommands (especially `purge`, `destroy`, or `create`).
2.  **Logic in `ceph-volume`:** Changes in how disks are initialized (LVM vs. Raw) or the introduction of new backend types (e.g., moving beyond BlueStore).
3.  **Default Pathing:** Changes to standard Linux paths for OSD metadata or data directories (`/var/lib/ceph/osd/`).
4.  **Cluster Safety Logic:** Changes to how "full ratios" are calculated or handled by the Monitor.
5.  **CRUSH Algorithm Updates:** New bucket types or changes in how `reweight` vs. `in/out` affects data distribution.