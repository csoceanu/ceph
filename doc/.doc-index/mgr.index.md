# MGR Documentation Index

## Overview
This documentation covers the Ceph Manager Daemon (`ceph-mgr`) and its extensive ecosystem of modules, which provide monitoring, orchestration, and external management interfaces for Ceph clusters. It defines the architecture for extending Ceph functionality through Python-based modules, including the web-based Dashboard, RESTful APIs, and integration with external orchestrators like Rook and Cephadm.

## Files Summary
*   **mgr/telemetry.rst**: Details the telemetry module used for sending anonymous usage and performance data to the Ceph project.
*   **mgr/orchestrator.rst**: Defines the CLI for interacting with external orchestration services (Cephadm, Rook) for host and service management.
*   **mgr/rgw.rst**: Covers the Manager-based RGW module for bootstrapping and managing multi-site RGW realms and zones.
*   **mgr/iostat.rst**: Describes a simple module for real-time reporting of cluster throughput and IOPS.
*   **mgr/telegraf.rst**: Explains how to export Ceph statistics to a Telegraf agent via socket listeners.
*   **mgr/orchestrator_modules.rst**: Provides developer-level documentation for implementing new orchestrator backend plugins.
*   **mgr/modules.rst**: The primary guide for developers on creating, logging, and exposing commands in Python-based manager modules.
*   **mgr/localpool.rst**: Documentation for the localpool module, which automates the creation of localized RADOS pools (e.g., per-rack).
*   **mgr/insights.rst**: Describes the module that aggregates health, crash, and configuration data for the Insights analysis framework.
*   **mgr/rook.rst**: Details the specific integration requirements and configuration for the Rook Kubernetes orchestrator.
*   **mgr/administrator.rst**: A manual for cluster admins on setting up, configuring, and managing the `ceph-mgr` daemon life-cycle.
*   **mgr/smb.rst**: Covers the management of SMB shares and Samba clusters through the `smb` module using imperative and declarative styles.
*   **mgr/dashboard.rst**: An exhaustive guide to the web-based management UI, covering setup, SSL, user roles, and monitoring features.
*   **mgr/nfs.rst**: Explains the management of NFS-Ganesha clusters and exports for CephFS and RGW buckets.
*   **mgr/diskprediction.rst**: Describes the module used for local disk failure prediction based on health metrics.
*   **mgr/crash.rst**: Details the automated collection, storage, and analysis of daemon crash dumps.
*   **mgr/influx.rst**: Covers the continuous collection and export of time-series data to an InfluxDB database.
*   **mgr/prometheus.rst**: Describes the Prometheus exporter implementation, including RBD stats, health checks, and metadata series.
*   **mgr/mds_autoscaler.rst**: Explains the logic for automatically adjusting the number of MDS daemons based on file system settings.
*   **mgr/index.rst**: The master table of contents and introduction for the Manager documentation section.
*   **mgr/progress.rst**: Details the module that provides visibility into long-running background tasks like PG recovery.
*   **mgr/alerts.rst**: Covers the simple SMTP-based alerting module for cluster health notifications.
*   **mgr/cli_api.rst**: Describes the module that exposes the internal Manager Python API via the command line for benchmarking.
*   **mgr/hello.rst**: A "Hello World" template used as a starting point for new module development.
*   **mgr/ceph_api/index.rst**: Documentation for the RESTful API provided by the dashboard, covering auth, versioning, and standards.
*   **mgr/dashboard_plugins/...**: (Multiple files) Covers specific dashboard enhancements like feature toggles, MOTD, and debug modes.

## Code Changes That Would Require Documentation Updates
*   **New Manager Module Creation**: Any addition of a new `.py` module in `pybind/mgr/` requires a corresponding `.rst` file and an entry in `mgr/index.rst`.
*   **CLI Command Modification**: Changes to `@CLICommand` decorators, parameter names, or type annotations in Python modules require updating the "Commands" or "Usage" sections of the module's doc.
*   **Configuration Option Changes**: Modifying the `MODULE_OPTIONS` class attribute (names, types, or default values) in any module.
*   **Orchestrator API Changes**: Updates to the `Orchestrator` base class methods (e.g., `add_host`, `apply_mds`) or `ServiceSpec` structures.
*   **Telemetry Channel/Collection Updates**: Adding or removing data points in `telemetry/module.py` requires updating the channel/collection lists in `telemetry.rst`.
*   **Dashboard REST API Versioning**: Any backward-incompatible change to a REST endpoint in the dashboard backend requires a version bump in `ceph_api/index.rst`.
*   **New Prometheus Metrics**: Adding new performance counters or changing the label structure in `prometheus/module.py`.
*   **NFS/SMB Resource Schema**: Updating the YAML/JSON spec definitions for SMB clusters or NFS exports.
*   **Crash Metadata Format**: Changes to the JSON blob structure generated during a daemon crash.

## Key Technical Concepts
*   **Core Concepts**: Active/Standby Manager, Python Modules, Orchestrator CLI, Service Placement Specs, Multi-site RGW, NFS-Ganesha, SMB/Samba.
*   **Commands**: `ceph mgr module enable/disable`, `ceph orch apply`, `ceph dashboard ac-user-create`, `ceph telemetry on`, `ceph nfs cluster create`.
*   **Configuration Keys**: `mgr/module_name/server_addr`, `mgr/telemetry/interval`, `mgr/prometheus/scrape_interval`, `mgr/dashboard/ssl`.
*   **APIs & Protocols**: RESTful API (v1.0), JWT Authentication, Prometheus Exposition Format, InfluxDB Line Protocol, SAML 2.0/OAuth2, CDLA License.
*   **Developer Classes**: `MgrModule`, `Orchestrator`, `CLICommand`, `Option`, `Responder`, `HandleCommandResult`.

## Related Components
*   **Core Daemons**: `ceph-mon`, `ceph-osd`, `ceph-mds`, `radosgw`.
*   **Orchestration**: `cephadm`, `Rook (Kubernetes)`.
*   **External Monitoring**: `Prometheus`, `Grafana`, `InfluxDB`, `Telegraf`, `Insights`.
*   **External Services**: `NFS-Ganesha`, `Samba (smbd/ctdb)`, `Keepalived`, `HAProxy`.
*   **Libraries**: `CherryPy` (Dashboard), `pyOpenSSL` (SSL/TLS).