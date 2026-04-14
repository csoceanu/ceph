This documentation file serves as the definitive reference for Ceph **Health Checks**, providing a catalog of warning and error states that a Ceph cluster can report via the `ceph health` command.

### 1. Primary Purpose
The file documents the unique identifiers (health check IDs), meanings, causes, and resolution steps for various health states raised by the **Ceph Monitor (MON)** and **Ceph Manager (MGR)** daemons. It is designed to help operators interpret cluster alerts and troubleshoot underlying infrastructure or configuration issues.

### 2. Key Topics Covered
*   **Monitor Health**: Alerts regarding quorum, clock skew, network partitions (netsplits), disk space (low/critical/database size), and messenger protocol versions (msgr2).
*   **Manager Health**: Status of the MGR daemons and errors/dependencies within Manager modules (e.g., Dashboard, Telemetry).
*   **OSD & BlueStore Health**: Detailed alerts for OSD connectivity, capacity (full/nearfull ratios), backend-specific issues (BlueStore fragmentation, metadata spillover, legacy tracking), and performance warnings (slow ops, stalled reads).
*   **Data Health (PGs & Pools)**: Alerts for data availability, redundancy (degraded/undersized PGs), scrubbing/deep-scrubbing status, and Placement Group (PG) count optimization.
*   **Device Health**: Predictive alerts regarding failing hardware based on S.M.A.R.T. data and life expectancy models.
*   **Security & Auth**: Warnings about insecure global ID reclamation, bad authentication capabilities, and insecure dashboard debug modes.
*   **Advanced Deployment Modes**: Specific checks for **Stretch Mode** (site weights/mon distribution) and **NVMeoF Gateway** status.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph health detail`, `ceph health mute`, `ceph config set`, `ceph osd set-full-ratio`, `ceph-bluestore-tool`, `ceph daemon osd.<id> ops`, `ceph mon enable-msgr2`.
*   **Configuration Options**: `mon_clock_drift_allowed`, `mon_data_avail_warn`, `auth_allow_insecure_global_id_reclaim`, `osd_deep_scrub_interval`, `bdev_stalled_read_warn_threshold`, `mon_pg_warn_min_per_osd`.
*   **Identifiers**: `MON_CLOCK_SKEW`, `OSD_FULL`, `BLUEFS_SPILLOVER`, `PG_AVAILABILITY`, `RECENT_CRASH`, `NVMEOF_GATEWAY_DOWN`.

### 4. Target Audience
*   **Storage Administrators**: For day-to-day monitoring and incident resolution.
*   **DevOps/SREs**: For configuring automated alerting and monitoring integrations (e.g., Prometheus/Grafana).
*   **Support Engineers**: For identifying root causes of cluster instability through health code analysis.

### 5. Related Concepts
*   **RADOS (Reliable Autonomic Distributed Object Store)**: The core system these checks monitor.
*   **CRUSH Hierarchy**: Related to OSD distribution and Stretch Mode health checks.
*   **BlueStore/Filestore**: The underlying storage backends that trigger specific disk-level alerts.
*   **Placement Groups (PGs)**: The logical unit for data health and redundancy checks.

---

### Update Criteria for AI Systems
This file must be updated whenever the Ceph codebase undergoes the following changes:

1.  **New Health Check Addition**: If a developer adds a new `health_check_t` or similar diagnostic alert in the Monitor, Manager, or OSD source code.
2.  **Identifier Rename**: If a health check string (e.g., `MON_DOWN`) is renamed to a more descriptive variable name.
3.  **Threshold Changes**: If default values for warnings (like `mon_data_avail_warn` default percentages) are modified in the configuration schema.
4.  **Feature Deprecation**: If a storage backend (like `Filestore`) or a feature (like a specific auth method) is removed, its associated health checks should be moved or deleted.
5.  **New Daemon/Service Integration**: When a new daemon type is introduced to the Ceph ecosystem (similar to the recent addition of `NVMeoF Gateway` checks).
6.  **CLI/Resolution Updates**: If the recommended command to fix a problem changes (e.g., a new tool replaces an old one for PG repair or BlueStore maintenance).