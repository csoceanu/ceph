This documentation file, `rados/operations/control.rst`, serves as a comprehensive reference for the **Ceph CLI (Command Line Interface)**. It outlines the administrative commands used to monitor, configure, and manage a Ceph Storage Cluster.

### 1. Primary Purpose
The file documents the syntax and usage of the `ceph` utility. It acts as a manual for operators to interact with various Ceph subsystems (Monitors, OSDs, PGs, MDS, and Auth) to maintain cluster health, manage data distribution, and troubleshoot issues.

### 2. Key Topics Covered
*   **Cluster Health & Status**: Monitoring the global state, quorum status, and real-time event logs.
*   **Authentication (ceph auth)**: Managing keyrings and capabilities for cluster security.
*   **Placement Groups (ceph pg)**: Diagnostic tools for PGs, including identifying "stuck" states (inactive, unclean, stale, undersized) and manual object recovery.
*   **OSD Management (ceph osd)**: 
    *   Lifecycle management (create, remove, up/down, in/out).
    *   Data distribution via CRUSH (setting weights, moving buckets, device classes).
    *   Storage pool operations (create, delete, rename, and parameter tuning).
    *   Maintenance tasks (scrubbing, repairing, and throughput benchmarking).
*   **Metadata Server (ceph mds)**: Basic status checks and runtime configuration overrides.
*   **Monitor Subsystem (ceph mon)**: Quorum tracking and monitor map (monmap) extraction.

### 3. Technical Keywords
*   **Utility**: `ceph`, `osdmaptool`, `jq`.
*   **Core Commands**: `ceph status`, `ceph tell`, `ceph osd df`, `ceph pg dump`, `ceph osd crush`.
*   **States/Flags**: `inactive`, `unclean`, `stale`, `degraded`, `pause`, `blocklist`.
*   **Configuration/Maps**: `monmap`, `osdmap`, `crush map`, `mon_osd_report_timeout`.
*   **Data Structures**: `Pools`, `Placement Groups (PGs)`, `CRUSH buckets`, `Quorum`.

### 4. Target Audience
*   **Storage Administrators**: For daily operations and cluster maintenance.
*   **SREs/DevOps Engineers**: For developing monitoring scripts and automation tools (specifically referencing the JSON output sections).
*   **Support Engineers**: For troubleshooting data availability or performance bottlenecks.

### 5. Related Concepts
*   **RADOS**: The underlying autonomous storage layer these commands control.
*   **CRUSH Algorithm**: The mechanism for data placement managed via `osd crush` commands.
*   **Ceph Manager (ceph-mgr)**: Mentioned as the modern alternative for balancing (via the `balancer` module).
*   **CephX**: The authentication protocol managed via `ceph auth`.

---

### AI Update Triggers (When to update this file)
An AI system should flag this file for updates if code changes occur in the following areas:

1.  **CLI Command Registration**: If new subcommands are added to `src/mon/MonCommands.h` or if existing command arguments/syntax are deprecated.
2.  **Subsystem State Logic**: If new Placement Group states are introduced or existing ones (like `stale` or `undersized`) are redefined.
3.  **Default Value Changes**: If hardcoded defaults mentioned in the docs change (e.g., the `max_osd` default of 10,000 or the `pg dump_stuck` threshold of 300s).
4.  **JSON Schema Changes**: Since the doc explicitly recommends JSON for automation, any change to the structure of `ceph osd dump --format json` or `ceph pg dump` should be reflected.
5.  **CRUSH Evolution**: If new bucket types or device classes (beyond `hdd`, `ssd`, `qlc`) are added to the core CRUSH implementation.
6.  **Deprecation of Legacy Features**: As noted with the `balancer` module replacing manual reweighting, any shift from manual CLI control to automated manager modules requires updating the "Note" or "Warning" sections.