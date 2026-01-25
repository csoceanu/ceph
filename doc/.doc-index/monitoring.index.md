# MONITORING Documentation Index

## Overview
This documentation provides a comprehensive guide to the Ceph observability stack, detailing how performance counters are transformed into Prometheus-compatible metrics. It serves as a technical reference for monitoring cluster health, performance, and daemon-specific behaviors (OSDs, RGW, MDS, RBD) using PromQL and integrated visualization tools like Grafana.

## Files Summary
*   **monitoring/index.rst**: The primary reference for the Ceph monitoring architecture, providing specific Prometheus metric names, label structures, and PromQL query examples for all major Ceph sub-systems.

## Code Changes That Would Require Documentation Updates
A developer should update this documentation if they perform any of the following:
*   **Metric Schema Changes**: Adding, renaming, or deprecating performance counters in Ceph daemons (OSD, MDS, RGW).
*   **Exporter Modifications**: Changing how `ceph_exporter` or the `mgr/prometheus` module transforms internal counters into Prometheus metrics (e.g., changing a metric from a Gauge to a Counter).
*   **Label Architecture**: Adding or removing labels (e.g., `ceph_daemon`, `pool_id`, `instance_id`) from existing metrics, as this breaks existing PromQL join logic.
*   **Service Port/Endpoint Changes**: Changing default ports for Prometheus, Alertmanager, or the exporter endpoints.
*   **New Daemon/Service Support**: Introducing a new Ceph service (e.g., a new proxy or gateway) that requires its own monitoring section.
*   **RBD Stats Logic**: Changing the opt-in mechanism or configuration keys (like `mgr/prometheus/rbd_stats_pools`) for high-cardinality metrics.
*   **Health Check Logic**: Modifying the behavior of the admin socket or how `ceph_daemon_socket_up` determines a daemon's "liveness."

## Key Technical Concepts
*   **Ceph Monitoring Stack**: Integrated Prometheus, Alertmanager, and Grafana deployment managed by `cephadm`.
*   **Core Exporters**: `ceph_exporter` (node-level) and the Ceph Manager `prometheus` module (cluster-level).
*   **Metric Naming Conventions**: Patterns such as `ceph_osd_op_*`, `ceph_pool_*`, `ceph_rgw_*`, and `ceph_mds_*`.
*   **Admin Socket**: The mechanism used by `ceph_exporter` to harvest native performance counters (`perf_counters`).
*   **PromQL Joins**: The use of metadata metrics (e.g., `ceph_pool_metadata`, `ceph_rgw_metadata`) to join human-readable labels (like pool names) onto raw numeric IDs.
*   **Latency Calculation**: The specific ratio method (`latency_sum` / `latency_count`) used to derive average latency from cumulative counters.
*   **Throughput & IOPS**: Derived metrics using `irate` or `rate` functions over byte and operation counters.

## Related Components
*   **Ceph Manager (MGR)**: Specifically the `prometheus` module.
*   **Cephadm**: Responsible for the deployment and lifecycle of the monitoring containers.
*   **OSD (Object Storage Daemon)**: Primary source of storage performance and capacity metrics.
*   **RGW (RADOS Gateway)**: Source of S3/Swift request and latency metrics.
*   **MDS (Metadata Server)**: Source of CephFS filesystem operation metrics.
*   **RBD (RADOS Block Device)**: Source of block-level image I/O statistics.
*   **Node Exporter**: External Prometheus exporter used in conjunction with Ceph metrics to monitor physical disk health.