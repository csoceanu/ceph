This documentation file serves as the manual page for the `ceph` CLI tool, the primary administrative interface for managing a Ceph cluster.

### 1. Primary Purpose
The file documents the **`ceph` administration tool**, a control utility used for manual deployment, maintenance, and real-time management of a Ceph storage cluster. It acts as the definitive reference for the command-line syntax used to interact with the Ceph Monitor (MON) and other cluster daemons.

### 2. Key Topics Covered
*   **Authentication (`auth`)**: Management of cephx keys and entity capabilities (caps).
*   **Configuration (`config`, `config-key`)**: Handling the centralized configuration database, runtime options, and the general-purpose key/value store.
*   **Cluster Health & Status**: Monitoring storage usage (`df`), cluster health (`health`), and overall status (`status`, `report`).
*   **Component Management**:
    *   **OSD (Object Storage Daemon)**: Creating/removing OSDs, managing the CRUSH map, erasure code profiles, and OSD tiers.
    *   **MON (Monitor)**: Managing the monitor map and quorum, including "stretch mode" for disaster recovery.
    *   **MDS (Metadata Server)**: Administrative tasks for CephFS metadata handling.
    *   **MGR (Manager)**: Enabling/disabling modules and viewing manager maps.
*   **Data Structures**: Managing Placement Groups (`pg`) and Ceph File Systems (`fs`).
*   **Direct Daemon Interaction**: Using `tell` or `daemon` to send commands directly to specific running processes or admin sockets.

### 3. Technical Keywords
*   **APIs/Subcommands**: `auth`, `crush`, `osd pool`, `pg`, `mds`, `mon`, `mgr`, `fs`, `tell`, `injectargs`, `daemonperf`.
*   **Configuration Options**: `ceph.conf`, `admin-socket`, `crush_device_class`, `min_size`, `erasure-code-profile`.
*   **Flags/States**: `blocklist`, `scrub`, `deep-scrub`, `reweight`, `in/out`, `up/down`, `primary-affinity`.
*   **Output Formats**: `json`, `json-pretty`, `xml`, `plain`, `yaml`.

### 4. Target Audience
*   **Storage Administrators**: For day-to-day cluster operations and troubleshooting.
*   **DevOps Engineers**: For automating cluster deployments and scaling.
*   **SREs (Site Reliability Engineers)**: For performance tuning and disaster recovery.
*   **Ceph Developers**: For low-level debugging via `pg debug` and `tell` commands.

### 5. Related Concepts
*   **CRUSH Algorithm**: The placement logic for data across the cluster.
*   **Cephx**: The authentication system used by Ceph components.
*   **RocksDB**: The underlying storage for monitor data (referenced via the `compact` command).
*   **TCMalloc**: Memory management (referenced via the `heap` commands).
*   **Erasure Coding & Replication**: Data durability strategies managed via pool settings.

---

### Update Triggers for AI Systems
This file must be updated if code changes occur in the following areas:
1.  **CLI Argument Parser**: Any time a new subcommand or flag is added to `src/ceph.cc` (or the equivalent CLI entry point).
2.  **Monitor Command Map**: If new commands are registered in the Monitor's command handler (`MonCommands.h`), they must be reflected here.
3.  **Deprecation Policy**: When existing commands (like `osd create`) are marked for removal or replaced (by `osd new`).
4.  **New Feature Integration**: If a new daemon type or major feature (e.g., "Stretch Mode") is introduced, the corresponding management subcommands must be documented.
5.  **Output Formatting**: If changes are made to how the CLI handles global options like `--format` or `--watch`.