This documentation file, `rados/operations/control.rst`, serves as a comprehensive reference for the command-line interface (CLI) used to manage and monitor a Ceph cluster.

### 1. Primary Purpose
The file documents the **Control Commands** for the Ceph RADOS cluster. It acts as a manual for the `ceph` utility, explaining how to interact with various cluster subsystems (Monitors, OSDs, PGs, MDS, and Auth) to query status, modify configurations, and manage data placement.

### 2. Key Topics Covered
*   **System Status**: Commands for global cluster health (`status`, `-s`, `-w`).
*   **Monitor (MON) Subsystem**: Managing the quorum, checking monitor status, and dumping monitor maps.
*   **OSD Subsystem**: 
    *   Lifecycle management (creating, removing, marking `in`/`out`/`down`).
    *   Map management (CRUSH maps, OSD maps).
    *   Data balancing (reweighting by utilization, pg-upmap).
    *   Maintenance (scrubbing, repairing, benchmarking).
*   **Placement Group (PG) Subsystem**: Troubleshooting "stuck" PGs (inactive, unclean, stale) and manually fixing data loss.
*   **Authentication (Auth)**: Managing keyrings and capabilities.
*   **Pool Management**: Creating, deleting, renaming, and configuring pool-specific settings (size, `pg_num`).
*   **MDS Subsystem**: Basic status and configuration for the Metadata Server.

### 3. Technical Keywords
*   **CLI Utility**: `ceph`, `ceph tell`, `osdmaptool`.
*   **Commands**: `ceph status`, `ceph pg dump`, `ceph osd crush`, `ceph auth ls`, `ceph osd reweight-by-utilization`, `ceph quorum_status`.
*   **Subsystems**: `mon`, `osd`, `pg`, `mds`, `auth`.
*   **Concepts/States**: `quorum`, `CRUSH map`, `reweight`, `blocklist`, `scrub`, `inactive/unclean/stale/undersized`.
*   **Formats**: `json`, `json-pretty`, `xml`, `plain`.

### 4. Target Audience
*   **Storage Administrators**: For day-to-day cluster operations and troubleshooting.
*   **DevOps/SREs**: For automating cluster monitoring and management via JSON-formatted CLI output.
*   **Testers**: Using commands like `blocklist` and `bench` for failure and performance testing.

### 5. Related Concepts
*   **CRUSH Algorithm**: The underlying logic for data placement modified by `osd crush` commands.
*   **RADOS**: The reliable autonomic distributed object store which these commands control.
*   **Ceph Manager (mgr)**: Specifically the `balancer` module mentioned as a modern alternative to manual reweighting.
*   **Monitor Quorum**: The consensus mechanism that ensures cluster consistency.

---

### Update Triggers for AI Systems
This file should be updated whenever code changes occur in the following areas:

1.  **CLI Argument Parser**: If new flags or subcommands are added to the `ceph` binary (e.g., new `ceph osd ...` or `ceph pg ...` arguments).
2.  **Subsystem State Logic**: If new PG states are introduced beyond "inactive," "unclean," or "stale."
3.  **CRUSH Map Hierarchy**: If the logic for bucket types or device classes (like the mentioned `hdd`, `ssd`, `qlc`) changes.
4.  **Default Values**: If hardcoded defaults—such as the `max_osd` limit (currently 10,000) or the `mon_osd_report_timeout`—are modified in the source code.
5.  **Output Formatting**: If the structure of the JSON output changes for commands like `pg dump` or `osd dump`, as this file explicitly recommends JSON for automation.
6.  **Deprecation of Legacy Features**: If "legacy" features mentioned (like `reweight-by-utilization`) are officially superseded by newer modules (like the `mgr` balancer).