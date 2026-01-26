This documentation file, `rados/operations/monitoring-osd-pg.rst`, serves as the primary operational guide for monitoring the health and status of **Object Storage Daemons (OSDs)** and **Placement Groups (PGs)** in a Ceph cluster.

### 1. Primary Purpose
The file provides administrators with the conceptual framework and practical commands necessary to evaluate the state of data distribution and hardware health. It explains how Ceph maintains high availability through its "up/down" and "in/out" OSD states and defines the lifecycle and health transitions of PGs.

### 2. Key Topics Covered
*   **OSD States**: Explanation of the four-way matrix of OSD status: `up` (reachable), `down` (unreachable), `in` (allocated data), and `out` (not allocated data).
*   **PG Sets**: Distinction between the **Acting Set** (OSDs currently handling requests) and the **Up Set** (OSDs intended to hold data).
*   **Peering Process**: How OSDs reach a consensus on the state of a PG before allowing I/O.
*   **PG Health States**: Detailed definitions of common statuses, including `active`, `clean`, `degraded`, `recovering`, `backfilling`, `remapped`, and `stale`.
*   **Troubleshooting**: Commands to identify "stuck" PGs and how to locate specific objects within the CRUSH hierarchy.

### 3. Technical Keywords
*   **Commands**: `ceph health`, `ceph -s`, `ceph osd stat`, `ceph osd tree`, `ceph pg dump`, `ceph pg map`, `ceph pg stat`, `ceph pg dump_stuck`, `ceph osd map`.
*   **States**: `active+clean`, `active+degraded`, `backfill_toofull`, `peering`, `remapped`, `stale`.
*   **Configuration Options**: `mon_osd_down_out_interval`, `osd_recovery_delay_start`, `osd_max_backfills`, `osd_mclock_override_recovery_settings`, `backfill_full_ratio`.
*   **Concepts**: CRUSH Map, Authoritative History, Epoch, mClock scheduler, Acting Set vs. Up Set.

### 4. Target Audience
*   **Storage Administrators**: For day-to-day monitoring and health checks.
*   **Systems Engineers**: For troubleshooting cluster degradation or performance issues during rebalancing.
*   **SREs/DevOps**: For understanding the implications of hardware failures and recovery timers.

### 5. Related Concepts
*   **CRUSH Algorithm**: The underlying engine determining data placement.
*   **RADOS**: The reliable autonomic distributed object store that manages these OSDs.
*   **Monitoring/Alerting**: Related to the `ceph health` subsystem and integration with external monitoring tools.

---

### Update Triggers (For AI/Developers)
This documentation should be updated if any of the following code changes occur:
1.  **CLI Changes**: If there are changes to the output schema of `ceph osd stat`, `ceph pg dump`, or `ceph osd tree`.
2.  **New PG States**: If new states are added to the Ceph PG state machine (e.g., in `PG.h` or the OSD source code).
3.  **Default Value Changes**: If the default values for recovery/backfill constants (like `mon_osd_down_out_interval`) are altered in the configuration code.
4.  **Scheduler Logic**: If the `mClock` or recovery scheduler logic is changed in a way that affects how `osd_max_backfills` or `osd_recovery_max_active` are prioritized.
5.  **Peering Logic**: If the consensus mechanism for PGs is modified (e.g., changes to how "Authoritative History" is determined).