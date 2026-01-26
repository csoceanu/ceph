This documentation file, `osd-config-ref.rst`, serves as the primary technical reference for configuring **Ceph Object Storage Daemons (OSDs)**. It bridges the gap between raw code parameters and operational deployment, detailing how to tune OSD behavior for performance, reliability, and resource management.

### 1. Primary Purpose
The file provides a comprehensive guide to the configuration variables (`confval`) available for Ceph OSDs. It explains the hierarchy of configuration (global `[osd]` vs. specific `[osd.ID]`), provides context for legacy versus modern storage backends (Filestore vs. BlueStore), and defines the operational mechanics of data integrity and Quality of Service (QoS).

### 2. Key Topics Covered
*   **General Configuration**: Basic identification (UUIDs), data paths, and the incremental numbering convention (e.g., `osd.0`).
*   **Storage Backends**: 
    *   **Filestore (Legacy)**: Detailed documentation on journals, file system mount options (XFS), and mkfs settings.
    *   **BlueStore (Current)**: Mentioned as the preferred default, though many settings in this specific file focus on tuning the I/O path.
*   **Data Integrity (Scrubbing)**: Differentiates between "light" and "deep" scrubbing; provides parameters to control timing, frequency, and resource impact.
*   **Operations & QoS (mClock)**: Extensive detail on the `dmClock` algorithm, discussing I/O sharding, cost-based resource allocation, and priority buckets (client vs. recovery vs. scrub).
*   **Cluster Maintenance**: 
    *   **Backfilling**: Managing data rebalancing when OSDs are added or removed.
    *   **Recovery**: Managing data restoration after a crash or peering event.
*   **OSD Maps**: Management of the map cache and epoch deduplication to maintain performance as the cluster scales.

### 3. Technical Keywords
*   **Configuration Sections**: `[osd]`, `[osd.n]`
*   **APIs/Config Options**: `osd_data`, `osd_journal_size`, `osd_max_scrubs`, `osd_op_num_shards`, `osd_mclock_scheduler_client_res`, `osd_recovery_priority`.
*   **Internal Mechanisms**: Heartbeats, Peering, Backfilling, Shallow vs. Deep Scrubbing, mClock/dmClock scheduler.
*   **Storage Terms**: XFS, BlueStore, Filestore, Journals, Placement Groups (PGs).

### 4. Target Audience
*   **Storage Administrators**: Seeking to tune cluster performance or troubleshoot OSD stability.
*   **Systems Architects**: Designing hardware layouts (e.g., SSD for journals/NVMe for mClock scaling).
*   **Developers**: Implementing new OSD features or modifying default behaviors in the Ceph source code.

### 5. Related Concepts
*   **CRUSH Algorithm**: Relates to how data rebalancing (backfilling) is triggered.
*   **Monitors (MONs)**: Relates to OSD-Monitor interaction and heartbeat reporting.
*   **Pools & PGs**: Relates to the logical grouping of data that OSDs manage.
*   **mClock Configuration**: This file is a high-level reference that points to the more specialized `mclock-config-ref`.

---

### When to Update this File (For AI/DevOps triggers)
This file must be updated if any of the following code changes occur:
1.  **New `confval` Definitions**: If a developer adds a new `Option` in the C++ source (typically `common/options/osd.yaml.in` or similar) that affects OSD behavior.
2.  **Default Value Changes**: If the default priority, timeout, or size for OSD operations (e.g., `osd_journal_size`) is modified.
3.  **Deprecation of Features**: If a storage backend (like Filestore) or a specific scheduler is removed or moved to legacy status.
4.  **mClock Refinement**: If the buckets (client, recovery, scrub) or the way weights/reservations are calculated in the `dmClock` implementation are changed.
5.  **Sharding Logic**: Changes to how OSDs shard operations (HDD vs. SSD) require updates to the "Operations" and "Caveats" sections.