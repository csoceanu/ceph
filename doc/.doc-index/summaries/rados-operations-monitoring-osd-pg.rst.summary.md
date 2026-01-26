This documentation file, `rados/operations/monitoring-osd-pg.rst`, serves as a foundational guide for observing and diagnosing the health of **Object Storage Daemons (OSDs)** and **Placement Groups (PGs)** in a Ceph cluster. It explains the lifecycle, state transitions, and discovery mechanisms used to maintain high availability.

### 1. Primary Purpose
The file documents how to monitor the health, status, and data distribution of a Ceph cluster. It defines the operational states of OSDs and PGs and provides the command-line tools necessary to identify faults, track data migration, and locate specific objects within the CRUSH hierarchy.

### 2. Key Topics Covered
*   **OSD Status Definitions**: Explains the matrix of OSD states: `up`/`down` (reachability) and `in`/`out` (participation in the CRUSH map).
*   **The Peering Process**: Describes how OSDs reach a consensus on the state of PGs before allowing I/O.
*   **PG Sets**: Differentiates between the **Up Set** (where data should be) and the **Acting Set** (the OSDs currently servicing requests).
*   **PG States Deep-Dive**: Detailed explanations of common and transitional states including `active`, `clean`, `degraded`, `recovering`, `backfilling`, `remapped`, and `stale`.
*   **Troubleshooting Stuck PGs**: Categorizes problematic PGs into `unclean`, `inactive`, and `stale` for targeted intervention.
*   **Object Mapping**: Procedures for tracing a specific object from a pool name and object ID to its physical location on OSDs.

### 3. Technical Keywords
*   **Commands**: `ceph health`, `ceph -s`, `ceph osd stat`, `ceph osd tree`, `ceph pg dump`, `ceph pg map`, `ceph pg stat`, `ceph pg dump_stuck`, `ceph osd map`, `rados put/ls/rm`.
*   **Configuration Options**: `mon_osd_down_out_interval`, `osd_recovery_delay_start`, `osd_recovery_max_active`, `osd_max_backfills`, `backfill_full_ratio`, `mon_osd_report_timeout`.
*   **Core Concepts**: CRUSH Map, Acting Set vs. Up Set, Peering, Authoritative History, Backfilling, Epoch.
*   **System Components**: `ceph-osd`, `ceph-mon`, `systemctl`.

### 4. Target Audience
*   **Storage Administrators**: For daily monitoring and health checks.
*   **SREs / Operators**: For troubleshooting "HEALTH_WARN" or "HEALTH_ERR" states during hardware failures or cluster expansions.
*   **Developers**: Understanding the state machine of PGs and how OSDs interact.

### 5. Related Concepts
*   **CRUSH Algorithm**: The underlying logic for data placement.
*   **Fault Domains**: Physical groupings (racks, hosts) used by CRUSH to ensure redundancy.
*   **mClock Scheduler**: Mentioned in the context of managing recovery and backfill priority.
*   **Data Scrubbing**: Related to the `clean` and `active` states where Ceph verifies data integrity.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **CLI Output Changes**: If the output format of `ceph osd stat` or `ceph pg dump` is modified.
2.  **New PG/OSD States**: If the RADOS state machine introduces new lifecycle states (e.g., new sub-states for `scrubbing` or `snaptrimming`).
3.  **Default Value Changes**: If the default intervals for `mon_osd_down_out_interval` or recovery settings are changed in the source code.
4.  **mClock Logic**: If the interaction between mClock and backfill/recovery settings is altered (e.g., new override flags).
5.  **New Monitoring Tools**: If a new primary command is introduced to replace `ceph pg dump` for identifying stuck PGs.