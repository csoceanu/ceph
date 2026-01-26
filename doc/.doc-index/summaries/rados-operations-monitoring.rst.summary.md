This documentation file, `rados/operations/monitoring.rst`, serves as the primary operational guide for observing the health, capacity, and performance of a Ceph cluster.

### 1. Primary Purpose
The file provides instructions on how to use the Ceph CLI and admin sockets to monitor a running cluster. It explains how to interpret cluster status, track data usage across different storage types, and diagnose network or daemon-level issues.

### 2. Key Topics Covered
*   **CLI Usage Modes**: Explains Interactive mode vs. standard CLI, and how to specify non-default configuration/keyring paths.
*   **Cluster Health & Status**: Interpreting `ceph status` and the lifecycle of health checks (from `HEALTH_OK` to `HEALTH_WARN` and recovery).
*   **Data Usage Analysis**: Detailed breakdown of "Raw Storage" vs. "Pool Usage," including how Ceph accounts for replication, erasure coding, and compression.
*   **Daemon Monitoring**: Specific commands for checking the status of OSDs (Object Storage Daemons), Monitors (Quorum status), and MDS (Metadata Servers for CephFS).
*   **Log Management**: Watching the real-time cluster log and retrieving historical entries.
*   **Advanced Diagnostics**: 
    *   **Health Muting**: How to temporarily silence specific health alerts (sticky vs. non-sticky).
    *   **Network Performance**: Monitoring heartbeat pings between OSDs to find network bottlenecks.
    *   **Admin Sockets**: Direct daemon-to-host querying for runtime configuration and "messenger" (network) snapshots.
    *   **Availability Tracking**: Calculating MTBF (Mean Time Between Failures) and MTTR (Mean Time To Recover) at the pool level.

### 3. Technical Keywords
*   **Commands**: `ceph status` (`-s`), `ceph health detail`, `ceph -w`, `ceph df`, `ceph osd tree`, `ceph quorum_status`, `ceph daemon`, `ceph tell`, `ceph health mute`.
*   **APIs/Options**: `dump_osd_network`, `messenger dump`, `pool availability-status`.
*   **Config Options**: `mon_osd_full_ratio`, `mon_pool_availability_update_interval`, `enable_availability_tracking`.
*   **Terms**: BlueStore, OMAP, Quorum, Heartbeats, CRUSH device class, Erasure Coding, Notional usage.

### 4. Target Audience
*   **Storage Administrators**: For daily maintenance and health checking.
*   **DevOps/SREs**: For automating monitoring scripts or integrating with external alerting systems.
*   **Troubleshooting Engineers**: For diagnosing performance degradations (slow pings) or daemon failures.

### 5. Related Concepts
*   **CRUSH Maps**: Relates to how `osd tree` displays the physical hierarchy.
*   **RADOS**: The underlying object store being monitored.
*   **CephFS/RGW**: Mentioned in the context of pool usage (Data vs. OMAP).
*   **Paxos**: Mentioned regarding the update interval for availability tracking.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **CLI Output Changes**: If the JSON or text output schema for `ceph status`, `ceph df`, or `ceph osd tree` is modified.
2.  **New Health Codes**: If new health check identifiers (like `OSD_DOWN`) are added to the source.
3.  **Messenger Protocol**: If the `messenger dump` or `async_connection` data structures are changed.
4.  **Usage Logic**: If the way BlueStore or RADOS calculates "Raw Used," "Stored," or "OMAP" usage changes.
5.  **New Monitoring Features**: If new sub-commands are added to `ceph health` (e.g., new mute behaviors) or new availability metrics are introduced.
6.  **Socket Interaction**: If the default path for `.asok` files or the mechanism for `ceph tell` vs. `ceph daemon` is altered.