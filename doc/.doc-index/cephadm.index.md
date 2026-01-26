# CEPHADM Documentation Index

## Overview
This documentation covers **cephadm**, the management and orchestration utility for Ceph clusters introduced in the Octopus release. It describes the full lifecycle management of a Ceph cluster—including bootstrapping, host management, daemon deployment, service configuration, upgrades, and troubleshooting—all performed within containerized environments using Podman or Docker.

## Files Summary
*   **upgrade.rst**: Details the automated and staggered upgrade processes, monitoring upgrade progress, and resolving common upgrade-related failures.
*   **compatibility.rst**: Outlines supported Podman versions for specific Ceph releases and lists the current stability status of various cephadm features.
*   **certmgr.rst**: Explains the certificate management service (`certmgr`) for handling self-signed and user-provided SSL/TLS certificates across the cluster.
*   **troubleshooting.rst**: Provides procedures for debugging failed daemons, resolving SSH issues, accessing admin sockets, and capturing core dumps for analysis.
*   **client-setup.rst**: Instructs on configuring client hosts with minimal config files and keyrings to interact with the Ceph cluster.
*   **host-management.rst**: Covers commands for adding, removing, draining, and labeling hosts, as well as managing OS-level tuning profiles (sysctl).
*   **adoption.rst**: Provides a step-by-step guide for converting legacy Ceph clusters (deployed via ceph-deploy, Ansible, or DeepSea) to cephadm management.
*   **index.rst**: The root landing page for the cephadm documentation, outlining its purpose and linking to specialized sub-topics.
*   **operations.rst**: Describes daily operational tasks like controlling daemons, managing logs (journald vs. files), and monitoring cluster health checks.
*   **install.rst**: Comprehensive guide for bootstrapping a new cluster, covering requirements, installation methods (curl/distro), and initial network setup.
*   **services/rgw.rst**: Documentation for deploying the RADOS Gateway, including multisite configurations, HTTPS setup, and High Availability via Ingress.
*   **services/snmp-gateway.rst**: Explains how to deploy a gateway to forward Prometheus alerts as SNMP notifications (V2c/V3).
*   **services/monitoring.rst**: Covers the deployment of the monitoring stack (Prometheus, Grafana, Alertmanager) and securing it with TLS/authentication.
*   **services/mds.rst**: Specific instructions for deploying Metadata Server daemons for CephFS volumes.
*   **services/mgmt-gateway.rst**: Details the Nginx-based management gateway that provides a unified, secure entry point for Ceph applications.
*   **services/oauth2-proxy.rst**: Describes integration with external Identity Providers (IDPs) via OIDC for securing the dashboard and monitoring stack.
*   **services/smb.rst**: Covers the experimental deployment of Samba containers for providing SMB access to CephFS.
*   **services/iscsi.rst**: Instructions for deploying iSCSI gateways using service specifications.
*   **services/nfs.rst**: Details the deployment of NFS Ganesha daemons, including HA configurations and HAProxy protocol support.
*   **services/tracing.rst**: Explains how to deploy Jaeger tracing services for performance analysis.
*   **services/mgr.rst**: Covers the deployment and network binding of Manager (MGR) daemons.
*   **services/osd.rst**: Exhaustive guide on OSD deployment, device listing, OSD removal/replacement, and automatic memory tuning.
*   **services/mon.rst**: Instructions for managing Monitor (MON) daemons, subnet designation, and setting CRUSH locations.
*   **services/index.rst**: General service management overview, explaining daemon placement, service specifications, and status monitoring.
*   **services/custom-container.rst**: Documentation for deploying arbitrary third-party containers using cephadm’s container service.

## Code Changes That Would Require Documentation Updates
*   **Orchestrator CLI**: Adding or modifying subcommands under `ceph orch` (e.g., new flags for `apply`, `ls`, or `ps`).
*   **Service Specifications**: Adding new fields to `ServiceSpec` or its subclasses (e.g., `RGWSpec`, `NFSGaneshaSpec`, `IscsiServiceSpec`).
*   **Daemon Types**: Introduction of new daemon types or changes to the deployment logic of existing ones (e.g., adding `nvmeof` or `smb` support).
*   **Upgrade Logic**: Changes to the daemon upgrade order or the introduction of new "staggered" upgrade parameters.
*   **Default Container Images**: Updating the default versions/tags for Ceph, Prometheus, Grafana, or Nginx in `images.py`.
*   **Health Checks**: Implementing new `CEPHADM_` health check codes or modified logic for existing alerts (e.g., `CEPHADM_STRAY_HOST`).
*   **Networking**: Changes to how `public_network` or `cluster_network` are handled during bootstrap or daemon deployment.
*   **Host Management**: Altering the logic for special host labels (e.g., `_admin`, `_no_schedule`) or adding new ones.
*   **Certificate Handling**: Modifications to the `certmgr` module or changes to how SSL fields (`ssl_cert`, `ssl_key`) are processed.
*   **Container Runtimes**: Changes in compatibility or support for Podman/Docker versions.
*   **Internal Templates**: Modifying Jinja2 templates in `mgr/cephadm/templates` (e.g., `haproxy.cfg.j2`, `nginx.conf.j2`).
*   **Device Management**: Updates to how `ceph-volume` or `libstoragemgmt` interact with `cephadm` for OSD provisioning.

## Key Technical Concepts
*   **Commands**: `ceph orch apply`, `ceph orch host add`, `ceph orch ps`, `ceph orch ls`, `ceph orch upgrade`, `ceph bootstrap`, `cephadm shell`.
*   **Configuration Options**: `mgr/cephadm/ssh_identity_key`, `osd_memory_target_autotune`, `secure_monitoring_stack`, `container_image_base`.
*   **Daemon Placement**: `label`, `host_pattern`, `count`, `count_per_host`.
*   **Service Discovery**: `http_sd_config`, `service_discovery_port`.
*   **Security/Auth**: `certmgr`, `oauth2-proxy`, `mgmt-gateway`, `CA signed keys`, `OIDC`.
*   **HA Components**: `keepalived`, `haproxy`, `virtual_ip`.
*   **Monitoring**: `Prometheus`, `Loki`, `Alloy`, `Grafana`, `Alertmanager`, `node-exporter`.

## Related Components
*   **Orchestrator (MGR Module)**: The primary interface for all `ceph orch` commands.
*   **ceph-volume**: Backend used for OSD device scanning and provisioning.
*   **mgr/dashboard**: Web GUI that utilizes cephadm's orchestration capabilities.
*   **mgr/prometheus**: Module providing metrics to the monitoring stack.
*   **Systemd**: Used on host nodes to manage the lifecycle of containerized daemons.
*   **Nginx/HAProxy**: Used as reverse proxies and load balancers within the management gateway and ingress services.