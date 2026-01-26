This documentation file, `rados/configuration/mon-config-ref.rst`, serves as the definitive reference for configuring and understanding **Ceph Monitors (MONs)**. It bridges the gap between high-level architectural concepts (like Paxos consensus) and low-level configuration parameters.

### 1. Primary Purpose
The file documents the **role, architecture, and configuration options** of the Ceph Monitor daemon. It explains how monitors maintain cluster state, achieve high availability through quorums, and provides a comprehensive list of configuration "tunables" that affect cluster health, storage limits, and synchronization.

### 2. Key Topics Covered
*   **Monitor Architecture**: Detailed explanation of the Cluster Map (Monitor, OSD, PG, and MDS maps) and the use of the **Paxos algorithm** for strong consistency.
*   **High Availability & Quorum**: The requirement for multiple monitors and how they establish a majority consensus.
*   **Consistency & Discovery**: Why monitors use the `monmap` rather than `ceph.conf` for discovery to avoid configuration errors.
*   **Bootstrapping**: Essential requirements for initialization, including `fsid`, Monitor IDs, and secret keys.
*   **Data Management**: Use of RocksDB for key/value storage and the importance of separating monitor data from OSD workloads.
*   **Storage Capacity Planning**: Managing "Full" and "Nearfull" ratios to prevent cluster lockups.
*   **Synchronization**: Roles of Leader, Provider, and Requester during monitor state syncing.
*   **Clock Synchronization**: The critical role of NTP/PTP in preventing Paxos anomalies and clock skew warnings.
*   **Guardrails**: Pool deletion protections and safety flags.

### 3. Technical Keywords
*   **Core Concepts**: Paxos, Quorum, Cluster Map (Epochs), `fsid`, `monmap`, Mon Host, Mon ID.
*   **Storage/Backend**: RocksDB, Key/Value Store, ACID transactions, `mmap()`.
*   **CLI/Commands**: `ceph osd set-full-ratio`, `ceph osd set-nearfull-ratio`, `cephadm`.
*   **Key Configuration Options**:
    *   `mon_initial_members`
    *   `mon_osd_full_ratio`, `mon_osd_nearfull_ratio`, `mon_osd_backfillfull_ratio`
    *   `mon_data`, `mon_memory_target`, `mon_memory_autotune`
    *   `mon_clock_drift_allowed`, `mon_tick_interval`
    *   `mon_allow_pool_delete`
*   **Network/Time**: DNS SRV records, NTP/PTP, `mon_addr`.

### 4. Target Audience
*   **Ceph Administrators**: Responsible for cluster deployment, capacity planning, and troubleshooting.
*   **Systems Architects**: Designing high-availability storage solutions.
*   **DevOps/Site Reliability Engineers (SREs)**: Automating Ceph deployments and monitoring cluster health.

### 5. Related Concepts
*   **CRUSH Algorithm**: Used by clients (via the cluster map) to calculate data locations.
*   **RADOS**: The underlying object store that monitors manage.
*   **Ceph OSD & MDS**: Daemons that rely on the maps maintained by the Monitors.
*   **Authentication (cephx)**: Monitors serve as the primary authentication authority.

---

### Update Triggers for AI Systems
This file must be updated if any of the following code-level changes occur:
1.  **New Configuration Constants**: If new `mon_` or `paxos_` prefixed variables are added to the Ceph source (typically in `common/options/mon.yaml.in` or similar).
2.  **Default Value Changes**: If the default thresholds for storage (e.g., `.95` full ratio) or timeouts (e.g., election timeouts) are modified in the code.
3.  **Architectural Shifts**: If the backend store changes from RocksDB, or if the consensus mechanism is updated.
4.  **CLI Command Deprecation**: If the syntax for setting ratios or managing monitor maps changes in the `ceph` CLI tool.
5.  **New Guardrails**: Addition of new safety flags regarding pool management or cluster health reporting.