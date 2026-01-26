This documentation file, `rados/troubleshooting/troubleshooting-mon.rst`, serves as the definitive guide for diagnosing and resolving failures within the **Ceph Monitor (MON)** cluster. It is a critical resource for maintaining cluster availability, as Monitors manage the "source of truth" (the Cluster Map) for the entire Ceph ecosystem.

### 1. Primary Purpose
The file provides a systematic approach to identifying why a Ceph Monitor cluster has lost quorum, why individual monitors are failing, or why the monitor store has become corrupted. It offers both high-level diagnostic steps and low-level recovery procedures to restore the cluster to a `HEALTH_OK` state.

### 2. Key Topics Covered
*   **Initial Triage**: Verifying daemon processes (`ceph-mon`, `ceph-mgr`), network connectivity (ports 3300/6789), and basic cluster responsiveness.
*   **The Admin Socket**: Instructions on interacting with a monitor directly via its `.asok` file when the standard CLI fails.
*   **Monitor States**: Detailed explanations of states like `probing`, `electing`, `synchronizing`, `leader`, and `peon`.
*   **Quorum Troubleshooting**: Analyzing `mon_status` output to identify which specific nodes are out of quorum and why.
*   **Clock Synchronization**: The impact of Paxos-related clock skews and how to mitigate them using NTP/Chrony.
*   **Advanced Recovery**: 
    *   Manual `monmap` injection.
    *   Rebuilding a destroyed Monitor store using metadata harvested from OSDs via `ceph-objectstore-tool`.
*   **Debugging**: Methods for adjusting debug log levels (e.g., `debug_mon`, `debug_ms`) at runtime without restarts.

### 3. Technical Keywords
*   **Commands**: `ceph-mon`, `ceph tell mon.<id> mon_status`, `ceph-conf`, `ceph-admin-daemon`, `ceph mon getmap`, `ceph-monstore-tool`, `ceph-objectstore-tool`.
*   **Configuration Options**: `mon_clock_drift_allowed`, `admin_socket`, `debug_mon`, `debug_ms`.
*   **Ports**: `3300`, `6789`.
*   **Concepts**: Paxos, Quorum, Monmap, Epoch, Rank, Election Storm, Admin Socket (.asok), Clock Skew.

### 4. Target Audience
*   **Storage Administrators**: For day-to-day maintenance and emergency recovery.
*   **Site Reliability Engineers (SREs)**: For automating health checks and troubleshooting network/firewall issues.
*   **Ceph Developers/Support Engineers**: When high-level tools fail and low-level store rebuilding or deep log analysis is required.

### 5. Related Concepts
*   **Paxos Consensus Algorithm**: The underlying logic that monitors use to agree on cluster state.
*   **Cluster Maps (Monmap, OSDMap, etc.)**: The data structures monitors are responsible for distributing.
*   **Ceph Manager (MGR)**: Often co-located with monitors; its availability is frequently checked alongside monitor health.
*   **Network Time Protocol (NTP)**: A prerequisite for monitor stability.

---

### When to Update This Document
This file should be updated if code changes occur in the following areas:
1.  **CLI/Tooling Changes**: If the output format of `mon_status` changes, or if new tools replace `ceph-monstore-tool`.
2.  **Monitor Logic**: Changes to the Paxos implementation, election algorithms, or how "Ranks" are calculated (currently based on IP:Port).
3.  **Default Ports/Paths**: If the default ports (3300/6789) or the default location of the admin socket/logs are altered.
4.  **Dependencies**: If the relationship between `ceph-mon` and `ceph-mgr` changes (e.g., new requirements for quorum).
5.  **Recovery Procedures**: If the process for extracting monitor data from OSDs is simplified or requires different arguments in `ceph-objectstore-tool`.