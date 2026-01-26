This documentation file, `rados/troubleshooting/troubleshooting-mon.rst`, serves as the definitive guide for diagnosing and resolving issues within the **Ceph Monitor (MON)** cluster.

### 1. Primary Purpose
The file provides a systematic approach to troubleshooting Ceph Monitors. Its goal is to help administrators maintain or restore the **Paxos-based quorum** required for cluster operation. It covers everything from basic connectivity checks to advanced disaster recovery of the monitor store.

### 2. Key Topics Covered
*   **Initial Triage**: Verifying `ceph-mon` and `ceph-mgr` daemon status and network reachability (ports 3300/6789).
*   **Quorum Analysis**: Using `mon_status` to identify which monitors are "Leader," "Peon," or out of quorum.
*   **State Identification**: Explaining specific monitor states such as `probing`, `electing`, and `synchronizing`.
*   **Infrastructure Issues**: Addressing clock skews (NTP/Chrony) and firewall (`iptables`) misconfigurations.
*   **Data Recovery**: Methods for injecting a `monmap` and rebuilding a corrupted monitor store using OSD metadata.
*   **Logging and Debugging**: Procedures for increasing debug verbosity (runtime) to provide actionable data to the community.

### 3. Technical Keywords
*   **Daemons/Services**: `ceph-mon`, `ceph-mgr`, `ceph-authtool`, `ceph-monstore-tool`, `ceph-objectstore-tool`.
*   **APIs/Interfaces**: Admin Socket (`.asok`), `ceph tell mon.<id>`, `mon_status`, `quorum_status`.
*   **Configuration**: `mon_clock_drift_allowed`, `debug_mon`, `debug_ms`, `admin_socket`.
*   **Network Ports**: `3300` (msgr2), `6789` (msgr1).
*   **Concepts**: Quorum, Paxos, monmap, Epoch, Rank, Clock Skew, Election Storm.

### 4. Target Audience
*   **Storage Administrators**: Managing production Ceph clusters.
*   **Site Reliability Engineers (SREs)**: Investigating cluster-wide outages or performance degradation.
*   **Support Engineers**: Collecting logs and diagnostic data for bug reporting.

### 5. Related Concepts
*   **High Availability**: How the monitor quorum ensures the cluster remains operational.
*   **Time Synchronization**: Heavy dependency on NTP/Chrony for Paxos consensus.
*   **RADOS Cluster Maps**: The monitors' role as the "source of truth" for OSD and MDS maps.
*   **Containerization**: Note on alternative paths for admin sockets when running in containers (e.g., Rook).

---

### Update Trigger Guide (For AI/Automation)
This file should be updated if code changes occur in the following areas:
*   **CLI Changes**: If the output format of `ceph mon_status` changes or if new flags are added to `ceph-monstore-tool`.
*   **Port Changes**: If the default messenger ports (3300/6789) are altered or supplemented.
*   **State Machine Logic**: If new monitor states are introduced or if the logic for "Ranks" (currently based on IP:Port comparison) is modified.
*   **Default Thresholds**: If the default `mon_clock_drift_allowed` (0.05s) is changed in the source code.
*   **Dependency Changes**: If Ceph transitions away from `asok` files to a different local management interface.
*   **New Components**: If a new daemon type becomes mandatory for monitor health (similar to how `ceph-mgr` was introduced).