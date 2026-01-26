This documentation file, `troubleshooting-osd.rst`, serves as the primary diagnostic and remediation guide for Ceph **Object Storage Daemons (OSDs)**. It outlines the methodology for identifying, isolating, and fixing common OSD-related failures, performance bottlenecks, and resource constraints.

### 1. Primary Purpose
The file provides a structured approach to troubleshooting OSDs within a RADOS cluster. It moves from preliminary infrastructure checks (Network/Monitors) to specific OSD failure scenarios, performance tuning, and the management of OSD states during maintenance.

### 2. Key Topics Covered
*   **Preliminary Infrastructure Validation**: Ensuring Monitor quorum and network integrity before diving into OSD-specific issues.
*   **Data Collection Tools**: Methods for gathering telemetry via Ceph logs, the Admin Socket, `iostat`, `df`, and `dmesg`.
*   **Maintenance Workflows**: Using `noout` flags to prevent unnecessary data rebalancing during planned downtime.
*   **Startup Failures**: Troubleshooting why a `ceph-osd` daemon fails to initialize (threading limits, connection tracking, kernel versions).
*   **Availability Issues**: Identifying "Down" OSDs and interpreting health alerts.
*   **Capacity Management**: Handling "Full," "Backfill Full," and "Nearfull" states; adjusting fullness ratios.
*   **Performance Bottlenecks**: Diagnosing slow/unresponsive OSDs, blocked requests, and the impact of co-resident processes or hardware issues.
*   **Flapping OSDs**: Understanding and mitigating the "up/down" cycle caused by network instability.
*   **mClock Scheduler**: Specific tuning for HDD-based clusters experiencing slow recovery or I/O.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph health detail`, `ceph osd tree`, `ceph daemon {daemon-name} help`, `ceph osd set noout`, `ceph osd df`, `ceph daemon osd.<id> dump_historic_ops`.
*   **Configuration Options**: `mon_osd_full_ratio`, `mon_osd_nearfull_ratio`, `osd_op_complaint_time`, `osd_op_num_shards_hdd`, `kernel.pid_max`, `nf_conntrack_max`.
*   **Daemons/Components**: `ceph-osd`, `ceph-mgr` (balancer), BlueStore, FileStore (legacy), CRUSH buckets.
*   **Performance Terms**: SyncFS, TCMalloc, suicide timeout, heartbeat failure, `queued_for_pg`, `op_commit`.

### 4. Target Audience
*   **Storage Administrators**: For day-to-day cluster maintenance and incident response.
*   **Site Reliability Engineers (SREs)**: To automate health monitoring and develop playbooks for OSD recovery.
*   **System Architects**: To understand the hardware/network requirements (RAM, NIC bonding, drive ratios) to avoid performance pitfalls.

### 5. Related Concepts
*   **Monitor Quorum**: OSDs cannot function without a healthy monitor map.
*   **CRUSH Algorithm**: Controls how data is rebalanced when OSDs are marked `out`.
*   **Placement Groups (PGs)**: The logical units that become "degraded" or "undersized" when OSDs fail.
*   **Networking (LACP/Bonding)**: Directly relates to preventing "Flapping OSDs."
*   **BlueStore/FileStore**: The underlying storage backends that dictate how data is written to disk.

---

### Maintenance Triggers: When to Update This File
This file should be updated by an AI or developer if code changes occur in the following areas:
1.  **Default Value Changes**: If the default thresholds for `full_ratio` (95%) or `nearfull_ratio` (85%) are modified in the source.
2.  **CLI Deprecations**: If commands like `ceph osd set noout` are superseded by newer `ceph-mgr` modules or different syntax.
3.  **New Backend Logic**: If a new storage backend is introduced or if **FileStore** support is completely removed from the codebase.
4.  **Scheduler Logic**: If the **mClock** scheduler parameters or the way OSD shards are calculated changes.
5.  **Log/Event Evolution**: If the internal states of an operation (e.g., `dispatched`, `reached_pg`) are renamed or if new telemetry events are added to the Messenger layer.