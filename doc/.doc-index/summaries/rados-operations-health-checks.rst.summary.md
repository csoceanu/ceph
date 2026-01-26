### 1. Primary Purpose
The file `rados/operations/health-checks.rst` serves as the authoritative reference catalog for **Ceph Health Checks**. It documents the human-readable identifiers (codes) triggered by the Ceph Monitor and Manager daemons when the cluster enters an abnormal state. Each entry explains the meaning of a specific health warning/error, its potential root causes, and the administrative commands required to troubleshoot or resolve the issue.

---

### 2. Key Topics Covered
The documentation is organized by the component or subsystem responsible for raising the health check:
*   **Monitor (MON):** Versions, clock synchronization, disk space/database size, network partitions (netsplits), and security protocols (global_id reclaim).
*   **Manager (MGR):** Daemon availability, module dependencies, and unhandled module exceptions.
*   **OSDs:** Daemon status, storage utilization (nearfull/full), CRUSH map inconsistencies, and backfill/recovery states.
*   **BlueStore & BlueFS:** Metadata spillover, disk fragmentation, slow operations, stalled reads, and legacy usage tracking.
*   **Device Health:** Predictive failure alerts and lifecycle management for physical drives.
*   **Data Health (Pools & PGs):** Data availability, redundancy (degraded/undersized), scrubbing/deep-scrubbing status, large Omap objects, and PG autoscaling.
*   **Stretch Mode:** Specific checks for multi-site/stretch cluster configurations and CRUSH bucket weights.
*   **NVMeoF Gateway:** High availability and connectivity status for NVMe-over-Fabrics gateways.
*   **Miscellaneous:** Daemon crashes, telemetry changes, and dashboard security settings.

---

### 3. Technical Keywords
*   **APIs/Commands:** `ceph health detail`, `ceph config set`, `ceph tell`, `ceph daemon`, `ceph-bluestore-tool`, `ceph osd pool set`, `ceph crash archive`.
*   **Configuration Options:** `mon_clock_drift_allowed`, `mon_data_avail_warn`, `osd_deep_scrub_interval`, `bdev_stalled_read_warn_threshold`, `auth_allow_insecure_global_id_reclaim`.
*   **Key Identifiers:** `MON_CLOCK_SKEW`, `OSD_FULL`, `PG_DEGRADED`, `BLUEFS_SPILLOVER`, `LARGE_OMAP_OBJECTS`, `RECENT_CRASH`.
*   **Subsystems:** RocksDB, BlueStore, BlueFS, CRUSH, RADOS, NVMeoF.

---

### 4. Target Audience
*   **Storage Administrators:** To diagnose and fix cluster warnings during daily operations.
*   **SREs/Monitoring Engineers:** To configure alerting thresholds and map Ceph health codes to external monitoring tools (e.g., Prometheus/Grafana).
*   **System Integrators:** To ensure custom deployments meet security and hardware synchronization requirements.

---

### 5. Related Concepts
*   **RADOS:** The underlying object store that these checks monitor.
*   **CRUSH Algorithm:** Relevant for data placement and failure domain checks.
*   **CephFS/MDS:** While mentioned as having their own checks, they interact with the RADOS health system.
*   **Telemetry:** The system used to report these health metrics back to the Ceph project.
*   **Erasure Coding:** Specifically related to techniques like `blaum_roth`.

---

### 6. Maintenance Guide for AI (When to update)
This file must be updated whenever the Ceph source code undergoes the following changes:
*   **New Health Code Addition:** If a new `health_check_t` or similar identifier is added to the Monitor, Manager, or OSD source code (typically in C++ headers or Manager modules).
*   **Deprecated Features:** If a subsystem (like `Filestore`) is removed or a warning is retired.
*   **Threshold Logic Changes:** If the default values or the behavior of warning thresholds (e.g., `mon_data_avail_warn`) are modified in the global config.
*   **New Management Commands:** If the recommended CLI method for resolving a specific health check changes (e.g., moving from `ceph-objectstore-tool` to a new `ceph daemon` command).
*   **Security Protocol Updates:** Changes to authentication or global ID reclamation logic that trigger new warnings.