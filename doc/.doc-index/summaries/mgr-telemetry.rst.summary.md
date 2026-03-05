This documentation file details the **Ceph Telemetry Module**, a Manager (`mgr`) component designed to collect and transmit anonymous cluster usage statistics, health metrics, and crash reports to the Ceph development community.

### 1. Primary Purpose
The file serves as the definitive guide for administrators to understand, configure, and manage Ceph’s telemetry system. It explains the opt-in nature of the service, the types of data collected, and how that data is used to improve the software via public dashboards and developer analysis.

### 2. Key Topics Covered
*   **Data Channels**: Breakdown of the five specific categories of data collection:
    *   `basic`: Cluster capacity, daemon counts, versions, and config keys.
    *   `crash`: Stack traces and OS metadata.
    *   **`device`**: SMART metrics (anonymized).
    *   `ident`: Optional user-provided contact info.
    *   `perf`: Performance counters and workload patterns.
*   **Privacy & Anonymization**: Explicit details on what is *not* collected (pool names, object contents, hostnames, serial numbers).
*   **Activation Workflow**: Procedures for opting in, including the mandatory acceptance of the **CDLA-Sharing-1.0** license.
*   **Collections**: Granular sub-components within channels (e.g., `basic_pool_usage`, `perf_memory_metrics`) and how to sync with new data collection points.
*   **Network Configuration**: How to configure reporting intervals and proxy settings for air-gapped or restricted clusters.
*   **Data Inspection**: Commands to preview reports locally before they are sent.

### 3. Technical Keywords
*   **CLI Commands**: `ceph telemetry on`, `ceph telemetry off`, `ceph telemetry enable channel <name>`, `ceph telemetry show`, `ceph telemetry preview`, `ceph telemetry collection ls`.
*   **Configuration Keys**: `mgr/telemetry/interval`, `mgr/telemetry/proxy`, `mgr/telemetry/contact`, `mgr/telemetry/leaderboard`.
*   **Dependencies**: `smartmontools` (v7.0+) for device health reporting.
*   **Endpoints**: `telemetry.ceph.com` (Ingestion), `telemetry-public.ceph.com` (Visualization).
*   **Licensing**: `sharing-1-0` (Community Data License Agreement).

### 4. Target Audience
*   **Storage Administrators**: To understand what data is leaving their network and how to anonymize it.
*   **Security/Compliance Officers**: To verify the privacy guarantees and licensing of shared data.
*   **Ceph Developers**: To ensure the documentation reflects new data collection "collections" added to the source code.

### 5. Related Concepts
*   **Ceph Manager (mgr)**: The daemon that hosts the telemetry module.
*   **SMART Metrics**: The industry-standard drive health data utilized by the `device` channel.
*   **Rook**: Mentioned in specific collections (`basic_rook_v01`) relating to Kubernetes deployments.
*   **Crash Reporting**: Integration with the Ceph crash module to provide stack traces.

---

### When to update this file (for AI/Automation)
This file must be updated if any of the following code changes occur:
1.  **New Channels/Collections**: If a developer adds a new data point to the telemetry report (e.g., adding a `basic_nvme_metadata` collection).
2.  **Config Key Changes**: If the internal `mgr/telemetry/` configuration options are renamed or new ones are added.
3.  **Command Syntax**: If the `ceph telemetry` CLI subcommands are modified.
4.  **License Changes**: If the project transitions to a different data-sharing license.
5.  **External Requirements**: If the minimum version of `smartmontools` or other local dependencies changes.