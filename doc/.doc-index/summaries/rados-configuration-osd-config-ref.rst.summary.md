This documentation file, `osd-config-ref.rst`, serves as the primary technical reference for configuring **Ceph Object Storage Daemons (OSDs)**. It acts as a comprehensive manual for tuning OSD performance, reliability, and resource allocation.

### 1. Primary Purpose
The file documents the available configuration options for Ceph OSDs. It explains how OSDs are identified, how they interact with the rest of the cluster, and provides a reference for parameters that control hardware utilization, data integrity (scrubbing), and I/O prioritization.

### 2. Key Topics Covered
*   **General Configuration**: OSD naming conventions (e.g., `osd.0`), configuration inheritance via `[osd]` global sections versus daemon-specific sections, and data/journal paths.
*   **Backend Storage (Filestore vs. BlueStore)**: Guidance on legacy Filestore settings (journaling, XFS mount options) while emphasizing that BlueStore is the modern default.
*   **Data Integrity (Scrubbing)**: Detailed explanation of "light" vs. "deep" scrubbing and the parameters used to control their frequency and performance impact.
*   **Quality of Service (mClock)**: A deep dive into the **dmClock algorithm**, explaining how Ceph manages I/O priorities (client ops, recovery, scrubbing) using reservation, limitation, and weight tags.
*   **Resource Management**: Sharding logic for OSD operations, threading models for different storage media (HDD vs. SSD), and backfilling/recovery speed controls.
*   **Cluster Dynamics**: Management of OSD Maps, peering processes, and rebalancing (backfilling) behaviors when the cluster topology changes.

### 3. Technical Keywords
*   **Core Entities**: `osd`, `mon`, `placement group (PG)`, `crush`, `journal`.
*   **Storage Backends**: `Filestore`, `BlueStore`, `XFS`.
*   **mClock/dmClock Parameters**: `osd_mclock_scheduler`, `reservation`, `limitation`, `weight`, `osd_op_queue`.
*   **Scrubbing Options**: `osd_max_scrubs`, `osd_deep_scrub_interval`, `osd_scrub_auto_repair`.
*   **Performance Tuning**: `osd_op_num_shards`, `osd_recovery_max_active`, `osd_op_thread_timeout`, `bluestore_throttle_bytes`.
*   **Configuration Files**: `ceph.conf`, Central Config Store.

### 4. Target Audience
*   **Storage Administrators**: Who need to tune cluster performance or troubleshoot OSD stability.
*   **System Architects**: Designing hardware layouts (e.g., colocating journals on NVMe vs. HDDs).
*   **Ceph Developers**: Reference for default behavior and existing configuration hooks when adding new OSD features.

### 5. Related Concepts
*   **CRUSH Algorithm**: Determines how backfilling and recovery are triggered based on map changes.
*   **Monitor (MON) Interaction**: OSDs report heartbeats to Monitors; configuration here defines those intervals.
*   **Pool & PG Management**: This file references external documentation for specific data placement and pool-level tuning.
*   **Quality of Service (QoS)**: Heavily linked to the `mclock-config-ref` for advanced traffic shaping.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **New Config Options**: If a new `confval` is added to the OSD daemon source code.
2.  **Default Value Changes**: If the default value for threads, shards, or scrubbing intervals is modified in the C++ headers.
3.  **Feature Deprecation**: If Filestore-specific settings are removed or if a new storage backend is introduced.
4.  **mClock Refinement**: If the dmClock implementation changes how it handles "cost," "shards," or "sequencer" logic.
5.  **Priority Handling**: If the internal prioritization of operations (e.g., snap trim, recovery, client I/O) is restructured.