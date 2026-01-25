# CEPHADM Documentation Index

## Overview
This documentation area covers **cephadm**, the native container-based orchestrator for Ceph. It details the full lifecycle management of a Ceph cluster, including bootstrapping, host management, service deployment (daemons), automated upgrades, monitoring integration, and troubleshooting in a containerized environment using Podman or Docker.

## Files Summary
*   **upgrade.rst**: Procedures for automated and staggered upgrades, MDS upgrade safety (fail_fs), and troubleshooting upgrade failures.
*   **compatibility.rst**: Support matrices for Podman versions and the current stability status of various cephadm features.
*   **certmgr.rst**: Management of the Root CA, self-signed and user-provided SSL certificates, and service-specific certificate bindings.
*   **troubleshooting.rst**: Tools for debugging containerized daemons, including log access, SSH key recovery, and using GDB/coredumps within containers.
*   **client-setup.rst**: Basic configuration for client hosts, including minimal `ceph.conf` generation and keyring distribution.
*   **host-management.rst**: CLI commands for adding/removing hosts, managing host labels (like `_admin` or `_no_schedule`), and OS tuning profiles.
*   **adoption.rst**: Workflow for converting existing legacy clusters (deployed via ceph-ansible/deploy) to cephadm management.
*   **index.rst**: Main entry point and table of contents for the cephadm documentation suite.
*   **operations.rst**: Day-2 operations including daemon control (start/stop/redeploy), log destination management, and cluster health checks.
*   **install.rst**: Prerequisites and multi-method bootstrap instructions (distribution packages vs. curl) for new clusters.
*   **services/rgw.rst**: Deployment of Object Gateways, multisite configurations, and high-availability ingress (HAProxy/Keepalived).
*   **services/snmp-gateway.rst**: Integration with Prometheus Alertmanager to forward SNMP notifications (V2c/V3) to management platforms.
*   **services/monitoring.rst**: Setup of the Prometheus/Grafana stack, secure monitoring (TLS/Auth), and centralized logging with Loki/Alloy.
*   **services/mds.rst**: Deployment of Metadata Servers for CephFS via volumes and service specs.
*   **services/mgmt-gateway.rst**: Deployment of the Nginx-based management gateway for unified, secure access to the dashboard and monitoring.
*   **services/oauth2-proxy.rst**: Configuration of external Identity Providers (IDPs) using OIDC for authenticated access to Ceph apps.
*   **services/smb.rst**: Deployment of Samba containers for SMB share access to CephFS, including clustering (CTDB) and AD integration.
*   **services/iscsi.rst**: Deployment and configuration of iSCSI gateways and target parameters.
*   **services/nfs.rst**: NFS Ganesha deployment, high-availability ingress setup, and HAProxy protocol support.
*   **services/tracing.rst**: Deployment of Jaeger tracing services (Agent, Collector, Query) and ElasticSearch integration.
*   **services/mgr.rst**: Management of MGR daemons, network binding, and co-location policies.
*   **services/osd.rst**: Storage device management, OSD creation/removal, drive groups (OSDSpecs), and memory autotuning.
*   **services/mon.rst**: Monitor daemon deployment, public network configuration, and CRUSH location assignment for tiebreakers.
*   **services/index.rst**: Overview of service and daemon status monitoring and the schema for YAML service specifications.
*   **services/custom-container.rst**: Specification format for deploying arbitrary non-Ceph containers, including init-containers and volume mounts.

## Code Changes That Would Require Documentation Updates
*   **CLI Syntax Changes**: Any modification to `ceph orch` subcommands or `cephadm` binary arguments (e.g., adding a flag to `bootstrap` or `upgrade`).
*   **Service Spec Schema**: Adding or removing fields in the `ServiceSpec` (e.g., new attributes in `RGWSpec`, `NFSGaneshaSpec`, or `OSDSpec`).
*   **Daemon Deployment Logic**: Changes to the order of operations in `cephadm` (e.g., changing the upgrade sequence or how `fail_fs` behaves).
*   **New Service Types**: Introduction of new managed services (e.g., a new exporter or gateway type).
*   **Container Runtime Support**: Changes to how `cephadm` interacts with Podman/Docker or updates to the Podman compatibility version matrix.
*   **Internal Configuration Keys**: Changes to `mgr/cephadm/` options (e.g., `autotune_memory_target_ratio` or `container_image_...`).
*   **Certificate/Security Logic**: Modifications to `certmgr` behavior, automated rotation policies, or new `mgmt-gateway` security features.
*   **Host/Label Logic**: Changes to special host labels (e.g., adding a new `_no_...` label) or how `_admin` distributes files.
*   **Health Checks**: Adding, removing, or changing the threshold for `CEPHADM_` health warnings and configuration checks.
*   **Logging/Monitoring Architecture**: Changing default container images (Prometheus, Grafana) or modifying the logging flow (Journald vs. File).

## Key Technical Concepts
*   **Orchestrator CLI**: `ceph orch apply`, `ceph orch ls`, `ceph orch ps`, `ceph orch host`.
*   **Bootstrap**: `cephadm bootstrap`, `--mon-ip`, `--ssh-user`, `--registry-json`.
*   **Service Specs**: Declarative YAML files defining `service_type`, `placement`, `networks`, and `spec`.
*   **Placement Specs**: `host_pattern`, `label`, `count`, `count_per_host`.
*   **Upgrade Mechanisms**: Staggered upgrades, `--ceph-version`, `--image`, `fail_fs`.
*   **Storage Management**: `ceph-volume` integration, Drive Groups (OSDSpec), `all-available-devices`, `zap`.
*   **Networking**: `public_network`, `cluster_network`, `virtual_ip` (VIP), CIDR notation.
*   **Security**: `certmgr`, Root CA, self-signed certs, `oauth2-proxy`, `mgmt-gateway`.
*   **Monitoring Stack**: Prometheus, Grafana, Alertmanager, Node-exporter, Loki, Alloy.
*   **Troubleshooting Tools**: `cephadm shell`, `cephadm logs`, `cephadm enter`, `ceph -W cephadm`.

## Related Components
*   **MGR Modules**: `cephadm`, `orchestrator`, `prometheus`, `dashboard`, `rgw`, `nfs`, `smb`.
*   **Container Runtimes**: Podman, Docker.
*   **System Tools**: Systemd, Journald, SSH, LVM2, Chrony/NTP.
*   **Ceph Daemons**: MON, MGR, OSD, MDS, RGW.
*   **External Gateways**: HAProxy, Keepalived, Nginx, SNMP Gateway, Jaeger.