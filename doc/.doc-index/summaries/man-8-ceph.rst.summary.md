This documentation file, `man/8/ceph.rst`, is the manual page for the **`ceph` administration tool**, the primary CLI utility for managing a Ceph cluster.

### 1. Primary Purpose
The file documents the syntax, subcommands, and options for the `ceph` control utility. It serves as the definitive reference for manual deployment, maintenance, and real-time administration of all core Ceph components (Monitors, OSDs, PGs, MDS, and Managers).

### 2. Key Topics Covered
*   **Authentication (`auth`)**: Management of cephx keys and entity capabilities.
*   **Cluster Configuration (`config`, `config-key`)**: Handling the monitor-based configuration database and general-purpose key/value storage.
*   **Daemon Management (`daemon`, `daemonperf`, `tell`)**: Interacting directly with running daemons via admin sockets or network commands.
*   **Storage Hierarchy (`osd crush`)**: Managing the CRUSH map, buckets, and placement rules.
*   **Data Layout (`osd pool`, `osd tier`)**: Creation and tuning of storage pools and cache tiers.
*   **File System (`fs`, `mds`)**: Administration of CephFS and Metadata Servers.
*   **Cluster Health (`health`, `df`, `status`, `report`)**: Monitoring the operational state, capacity, and performance.
*   **Component Maps (`mon`, `osd`, `mgr`, `pg`)**: Detailed management of individual daemon types and data placement groups.

### 3. Technical Keywords
*   **APIs/Subcommands**: `auth`, `compact`, `config`, `daemon`, `df`, `fs`, `health`, `mds`, `mon`, `mgr`, `osd`, `pg`, `tell`, `injectargs`.
*   **Configuration**: `ceph.conf`, `RocksDB`, `tcmalloc`, `injectargs`, `assimilate-conf`.
*   **Data Structures**: `CRUSH map`, `PG (Placement Group)`, `MonMap`, `OSDMap`, `FSMap`, `MgrMap`.
*   **Security**: `cephx`, `caps`, `keyring`, `dm-crypt`, `blocklist`.
*   **CLI Flags**: `--format`, `--watch`, `--connect-timeout`, `--yes-i-really-mean-it` (safety override).

### 4. Target Audience
*   **System Administrators**: Responsible for day-to-day cluster operations and troubleshooting.
*   **DevOps Engineers**: Automating Ceph deployment and scaling.
*   **Storage Engineers**: Tuning performance and data placement strategies.
*   **Developers**: Testing cluster features or debugging daemon-specific behavior.

### 5. Related Concepts
*   **Daemons**: Closely related to `ceph-mon`, `ceph-osd`, `ceph-mds`, and `ceph-mgr` binaries.
*   **Storage Orchestrators**: Works alongside `cephadm` or `Rook`, though this tool focuses on the internal Ceph state rather than host-level orchestration.
*   **Monitoring**: Feeds data into the Ceph Dashboard and Prometheus/Grafana stacks.

---

### Update Triggers for AI Systems
This file must be updated whenever the following code changes occur:

1.  **New CLI Subcommands**: If a new module is added to the `ceph` CLI (e.g., a new top-level command like `orch` or a new subcommand under `osd`).
2.  **Configuration Parameter Changes**: If new configuration options are added to the monitor's config database that require specific `ceph config` syntax explanations.
3.  **Command Deprecation**: When a command is phased out (e.g., the transition from `osd create` to `osd new`).
4.  **Feature Flags**: Addition of new cluster-wide flags (e.g., new OSD maintenance flags like `noout` or `noscrub`).
5.  **Output Format Updates**: Changes to supported `--format` types or the structure of JSON/XML outputs.
6.  **Safety Logic**: Changes to the requirements for "danger" flags (e.g., `--yes-i-really-mean-it`) for specific destructive operations.