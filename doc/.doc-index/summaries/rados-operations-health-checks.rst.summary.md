This documentation file serves as the definitive reference for Ceph's cluster-wide **Health Checks**. It defines the specific warning and error codes emitted by the Monitor and Manager daemons to indicate hardware failures, misconfigurations, or performance bottlenecks.

### 1. Primary Purpose
The file documents the automated health monitoring system of a Ceph cluster. It provides a mapping between terse health identifiers (e.g., `MON_DISK_LOW`) and their underlying causes, technical implications, and manual/automated remediation steps.

### 2. Key Topics Covered
The documentation is organized by the functional component responsible for raising the health check:
*   **Monitor (MON) Checks**: Issues with quorum, clock synchronization, disk space on monitor nodes, protocol versions (msgr2), and authentication security (global_id).
*   **Manager (MGR) Checks**: Availability of the manager daemon and errors within specific manager modules or their dependencies.
*   **OSD & CRUSH Checks**: Status of storage daemons (Up/Down/Out), capacity thresholds (Nearfull/Full), BlueStore-specific health (fragmentation, spillover, disk mismatch), and legacy CRUSH settings.
*   **Device Health**: Predictive failure alerts for physical drives and self-healing logic.
*   **Data Health (Pools & PGs)**: Placement Group (PG) states (Availability, Degraded, Undersized), scrubbing/deep-scrubbing status, and PG scaling (too few/too many PGs).
*   **Specialized Modes**: Health logic for **Stretch Mode** (multi-site consistency) and **NVMeoF Gateways**.
*   **Miscellaneous**: Cluster crashes, telemetry updates, and insecure dashboard configurations.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph health detail`, `ceph config set`, `ceph tell`, `ceph osd pool set`, `ceph-bluestore-tool`, `ceph crash archive`.
*   **Configuration Options**: `mon_clock_drift_allowed`, `mon_data_avail_warn`, `osd_deep_scrub_interval`, `pg_autoscale_mode`, `mon_osd_min_in_ratio`.
*   **Core Concepts**: Quorum, CRUSH tunables, BlueStore (DB/WAL/Slow devices), OMAP objects, Backfill, Recovery, Scrubbing, and Placement Groups (PGs).
*   **State Identifiers**: `active+clean`, `peering`, `stale`, `incomplete`, `undersized`, `degraded`.

### 4. Target Audience
*   **Storage Administrators**: For troubleshooting cluster warnings and performing maintenance.
*   **Site Reliability Engineers (SREs)**: For configuring monitoring thresholds and automated alerting.
*   **Developers/Integrators**: For understanding how to handle health status codes in UIs or external orchestration tools.

### 5. Related Concepts
*   **CephFS Health**: MDS-specific messages are documented separately but overlap in logic.
*   **RADOS**: The underlying object store that these health checks primarily monitor.
*   **Monitoring Frameworks**: Prometheus/Grafana integrations often consume these identifiers.
*   **CRUSH Maps**: The topology logic that informs many of the OSD and Stretch Mode checks.

---

### Update Triggers for AI Maintenance
This file must be updated if any of the following code changes occur:
1.  **New Health Identifiers**: If a new `health_check_t` or identifier string is added to the C++ source code (usually in Monitor or Manager classes).
2.  **Logic Changes**: If the default thresholds for warnings (e.g., `full_ratio` or `nearfull_ratio`) are changed in the configuration schema.
3.  **New Components**: If a new daemon type (similar to NVMeoF) is introduced that requires its own health reporting category.
4.  **Deprecations**: If a storage backend (like Filestore) is removed, its associated health checks should be archived or deleted.
5.  **CLI Changes**: If the recommended command to fix a problem (e.g., `ceph mon enable-msgr2`) is replaced by a newer orchestration command (e.g., via `ceph orch`).