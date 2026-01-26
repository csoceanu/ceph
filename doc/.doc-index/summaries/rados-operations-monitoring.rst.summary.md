### Documentation Summary: `rados/operations/monitoring.rst`

This documentation serves as the primary operational guide for monitoring the health, status, and performance of a Ceph cluster using the command-line interface.

#### 1. Primary Purpose
The file provides instructions and command references for administrators to verify the operational state of a Ceph cluster. It explains how to interpret cluster health metrics, track data usage, monitor daemon status (OSDs, Monitors, MDS, Managers), and troubleshoot network performance.

#### 2. Key Topics Covered
*   **The `ceph` Tool**: Usage of interactive mode vs. non-interactive mode, and handling non-default configuration/keyring paths.
*   **Cluster Status & Health**: Interpreting `ceph status` and `ceph health` outputs, including how Ceph calculates raw vs. notional data usage.
*   **Event Logging**: Using `ceph -w` to follow the cluster log and `ceph log last` for historical events.
*   **Health Check Management**: Procedures for muting, unmuting, and setting Time-To-Live (TTL) or "sticky" flags on specific health alerts (e.g., `OSD_DOWN`).
*   **Network Performance**: Monitoring heartbeat pings between OSDs and using `dump_osd_network` to identify slow network switches or NICs.
*   **Usage Statistics**: Detailed breakdown of the `ceph df` and `ceph df detail` commands, covering storage classes, pools, and BlueStore-specific metrics (OMAP, compression).
*   **Subsystem Monitoring**: 
    *   **OSDs**: Checking status via `osd tree` and `osd stat`.
    *   **Monitors**: Verifying quorum via `quorum_status` and `mon dump`.
    *   **MDS**: Checking CephFS metadata server states.
    *   **PGs**: Monitoring Placement Group health (active+clean).
*   **Advanced Debugging**: Using the Admin Socket (`ceph daemon`) and `messenger dump` for low-level connection and TCP/IP stats.
*   **Availability Scoring**: Tracking Mean Time Between Failures (MTBF) and Mean Time To Recover (MTTR) per pool.

#### 3. Technical Keywords
*   **Commands**: `ceph status`, `ceph health mute`, `ceph df detail`, `ceph osd tree`, `ceph quorum_status`, `ceph tell`, `ceph daemon`, `dump_osd_network`, `messenger dump`.
*   **APIs/Configuration**: `mon_osd_full_ratio`, `paxos_propose_interval`, `enable_availability_tracking`, `--sticky`, `--ttl`.
*   **Terms**: Heartbeat pings, Quorum, CRUSH device class, OMAP, Notional usage, BlueStore, Admin Socket (.asok), TCP_INFO.

#### 4. Target Audience
*   **Storage Administrators**: For daily maintenance and health verification.
*   **SREs/DevOps Engineers**: For troubleshooting performance bottlenecks and network issues.
*   **Automated Monitoring Developers**: For understanding the output formats of Ceph CLI tools to build wrappers or alerts.

#### 5. Related Concepts
*   **RADOS Operations**: General cluster management.
*   **CephFS**: Specifically regarding MDS status and metadata storage.
*   **CRUSH Maps**: Related to how `osd tree` visualizes data placement.
*   **BlueStore**: Specifically regarding how storage usage and compression are calculated.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **CLI Output Changes**: If the schema of `ceph status`, `ceph df`, or `messenger dump` is modified (e.g., new columns added to the usage table).
2.  **New Health Checks**: If new `HEALTH_ERR` or `HEALTH_WARN` codes are introduced, as they affect the "Muting Health Checks" section.
3.  **Daemon Logic**: If the way Ceph calculates "MTBF" or "MTTR" changes, or if new daemon types are introduced to the `messenger` subsystem.
4.  **Admin Socket**: If new commands are added to the `.asok` interface or if the `ceph daemon` syntax is deprecated.
5.  **BlueStore Metrics**: If changes are made to how raw storage usage, compression ratios, or OMAP data are reported.