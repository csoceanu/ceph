This documentation file describes the **Ceph Manager (mgr) Prometheus module**, which serves as an internal exporter to provide Ceph cluster metrics to Prometheus.

### 1. Primary Purpose
The file documents the installation, configuration, and operational behavior of the `prometheus` manager module. Its main goal is to explain how Ceph performance counters and health data are transformed into a Prometheus-compatible format and exposed via an HTTP endpoint.

### 2. Key Topics Covered
*   **Activation & Configuration**: How to enable the module and modify its network settings (IP, port) and behavior.
*   **Caching & Performance**: Managing the trade-off between metric freshness and Manager load, especially in large clusters (>1000 OSDs).
*   **Health Monitoring**: How Ceph health checks are mapped to discrete Prometheus metrics and the management of health check history.
*   **Feature-Specific Stats**: Instructions for enabling per-image RBD (RADOS Block Device) IO statistics.
*   **Metric Interoperability**: Techniques for correlating Ceph-specific metrics with host-level metrics from the Prometheus `node_exporter` (e.g., mapping a drive to an OSD).
*   **Prometheus Server Setup**: Recommended scrape configurations, including the importance of the `honor_labels` setting.

### 3. Technical Keywords
*   **Daemons/Processes**: `ceph-mgr`, `ceph-exporter`, `node_exporter`, `MgrClient`, `OSD`, `Monitor`.
*   **Configuration Keys**: `mgr/prometheus/server_addr`, `mgr/prometheus/server_port`, `mgr/prometheus/scrape_interval`, `mgr/prometheus/stale_cache_strategy`, `mgr/prometheus/rbd_stats_pools`, `mgr/prometheus/exclude_perf_counters`.
*   **Commands**: `ceph mgr module enable prometheus`, `ceph config set`, `ceph healthcheck history ls/clear`.
*   **Metrics/Labels**: `ceph_health_detail`, `ceph_pool_metadata`, `ceph_osd_metadata`, `ceph_disk_occupation_human`, `honor_labels`.
*   **HTTP Status Codes**: `503` (Service Unavailable), `500` (Internal Error/Standby).

### 4. Target Audience
*   **Storage Administrators**: Who need to monitor Ceph health and performance.
*   **DevOps/SREs**: Who are integrating Ceph into an existing Prometheus/Grafana monitoring stack.
*   **System Architects**: Designing large-scale storage clusters that require performant metric scraping.

### 5. Related Concepts
*   **Prometheus Exposition Format**: The standard text-based format for metrics.
*   **Ceph Performance Counters**: The internal data source for all metrics exposed by this module.
*   **Alertmanager**: Specifically mentioned regarding the use of `ceph_health_detail` for triggering alerts.
*   **PromQL**: The query language used to correlate the module's data with other exporters.

---

### Integration Guide for AI Systems
This file should be updated or referenced when code changes occur in the following areas:

*   **Configuration Schema Changes**: If new configuration options are added to `PyModule.pm` or the Prometheus module's `OPTIONS` list (e.g., new cache strategies or filtering options).
*   **Metric Naming Logic**: If the logic for sanitizing Ceph counter names (translating `.`, `-`, `::` to `_`) is modified.
*   **Performance Scaling**: If the default behavior of the cache or the collection mechanism for large clusters is optimized or changed.
*   **New Metadata Types**: If new metadata metrics (similar to `ceph_pool_metadata`) are introduced to expose more information about Ceph components.
*   **Standard Port Changes**: If the default port `9283` is changed or if the module's interaction with the standby Manager logic is updated.
*   **Deprecations**: If features are moved (like the shift of daemon performance counters to `ceph-exporter`).