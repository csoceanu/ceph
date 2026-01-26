This documentation file, `troubleshooting-osd.rst`, serves as the primary diagnostic and remediation guide for **Ceph Object Storage Daemons (OSDs)**. It is a critical resource for maintaining cluster health, managing maintenance windows, and resolving performance bottlenecks.

### 1. Primary Purpose
The file provides a systematic approach to identifying, diagnosing, and fixing issues related to OSDs. It covers everything from initial environmental checks (network and monitors) to specific failure scenarios like "flapping" OSDs, full disks, and slow I/O requests.

### 2. Key Topics Covered
*   **Pre-flight Checks**: Verifying monitor quorum and network integrity before diving into OSD-specific issues.
*   **Data Collection Tools**: Instructions for using Ceph logs, the Admin Socket, `iostat`, `df`, and `dmesg`.
*   **Maintenance Workflows**: How to stop OSDs for hardware work without triggering aggressive cluster rebalancing (using `noout` flags).
*   **Startup Failures**: Troubleshooting why a `ceph-osd` process fails to initialize (checking thread counts, `nf_conntrack` limits, and configuration paths).
*   **Capacity Management**: Handling "Nearfull" and "Full" OSDs, including adjusting ratios and using the balancer.
*   **Performance Optimization**: Diagnosing slow requests, identifying bottlenecks (RAM, Co-resident processes, Kernel versions), and debugging via `dump_historic_ops`.
*   **Network Pathologies**: Detailed explanation of "Flapping" OSDs and the impact of public vs. cluster network split.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph health detail`, `ceph osd tree`, `ceph daemon {daemon-name} help`, `ceph osd set noout`, `ceph osd df`, `ceph osd set-group noout`.
*   **Configuration Options**: `mon_osd_full_ratio`, `mon_osd_nearfull_ratio`, `osd_op_complaint_time`, `kernel.pid_max`, `nf_conntrack_max`, `osd_op_num_shards_hdd`.
*   **System Tools**: `systemctl`, `iostat`, `dmesg`, `netstat`, `smartctl`, `sysctl`.
*   **Internal States**: `up`, `down`, `in`, `out`, `degraded`, `queued_for_pg`, `slow requests`.

### 4. Target Audience
*   **Ceph Cluster Administrators**: Responsible for day-to-day operations and emergency troubleshooting.
*   **Storage Engineers**: Designing hardware layouts and tuning performance.
*   **Site Reliability Engineers (SREs)**: Managing automated deployments and maintenance scripts.

### 5. Related Concepts
*   **CRUSH Algorithm**: Specifically how failure domains and rebalancing behave when OSDs are removed.
*   **Monitor (MON) Quorum**: The prerequisite for any OSD troubleshooting.
*   **Placement Groups (PGs)**: How data is redistributed when OSDs fail or fill up.
*   **Storage Backends**: References to **BlueStore** (current standard) and **FileStore** (legacy/deprecated).
*   **mClock Scheduler**: QoS management for I/O operations.

---

### Maintenance Note for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **Default Thresholds**: If the default `full`, `nearfull`, or `backfillfull` ratios are changed in the source.
2.  **Command Syntax**: If `ceph-mgr` or the CLI introduces new ways to handle flags (e.g., changes to `noout` or `noup` behavior).
3.  **Default Sharding**: Changes to OSD threading models or shard defaults (like the `osd_op_num_shards_hdd` logic mentioned for mClock).
4.  **Deprecations**: When legacy support (like FileStore or specific kernel versions) is officially removed from the codebase.
5.  **New Diagnostic Hooks**: If new admin socket commands or performance counters are added to help debug slow requests.