# CEPHADM Documentation Index

## Overview
This documentation covers **cephadm**, the primary orchestration tool for deploying and managing Ceph clusters using containers. It details the full lifecycle of a Ceph cluster, including bootstrapping, host management, service deployment, automated upgrades, security/certificate management, and troubleshooting in containerized environments.

## Files Summary
*   **upgrade.rst**: Procedures for automated and staggered Ceph version upgrades, including monitoring and failure recovery.
*   **compatibility.rst**: Support matrices for Podman versions and the current stability status of various cephadm features.
*   **certmgr.rst**: Management of self-signed and user-provided SSL certificates via the `certmgr` service.
*   **troubleshooting.rst**: Tools and workflows for debugging containerized daemons, including log access, shell entry, and core dump analysis.
*   **client-setup.rst**: Basic configuration for client hosts to interact with a Ceph cluster (keyrings and minimal configs).
*   **host-management.rst**: CLI commands for adding, removing, labeling, and putting hosts into maintenance mode.
*   **adoption.rst**: Process for converting legacy clusters (ceph-deploy, ceph-ansible) to cephadm management.
*   **index.rst**: High-level introduction and table of contents for the cephadm documentation suite.
*   **operations.rst**: Day-to-day daemon control (stop/start/redeploy), logging configurations, and cluster health check references.
*   **install.rst**: Prerequisites and multi-scenario bootstrap procedures (including air-gapped and custom SSH setups).
*   **services/rgw.rst**: Deployment specs for Rados Gateway, including multisite and high-availability (ingress) configurations.
*   **services/snmp-gateway.rst**: Integration with Prometheus Alertmanager to forward alerts as SNMP notifications.
*   **services/monitoring.rst**: Setup for the Prometheus/Grafana/Alertmanager stack, including security and custom Jinja2 templates.
*   **services/mds.rst**: Deployment instructions for Metadata Servers required for CephFS.
*   **services/mgmt-gateway.rst**: Configuration for the Nginx-based management gateway providing unified access to Ceph apps.
*   **services/oauth2-proxy.rst**: Securing Ceph applications using OIDC-based external Identity Providers.
*   **services/smb.rst**: Management of Samba containers for SMB share access to CephFS.
*   **services/iscsi.rst**: Deployment of iSCSI gateways using service specifications.
*   **services/nfs.rst**: Provisioning NFS Ganesha gateways with high-availability and HAProxy protocol support.
*   **services/tracing.rst**: Deployment of Jaeger and Elasticsearch for distributed tracing.
*   **services/mgr.rst**: Management of Manager daemons, module hosting, and co-location policies.
*   **services/osd.rst**: Storage device management, OSD creation/removal, drive groups, and memory autotuning.
*   **services/mon.rst**: Monitor daemon placement, network configuration, and CRUSH location management.
*   **services/index.rst**: General concepts for Service Specifications, daemon placement logic, and container argument overrides.
*   **services/custom-container.rst**: Specification for deploying arbitrary non-Ceph containers and init-containers.

## Code Changes That Would Require Documentation Updates
*   **Orchestrator CLI**: Adding or modifying subcommands under `ceph orch` (e.g., new flags for `ls`, `ps`, or `apply`).
*   **Service Specifications**: Adding new fields to any `ServiceSpec` class (e.g., `RGWSpec`, `IscsiServiceSpec`, `MgmtGatewaySpec`).
*   **Upgrade Logic**: Changes to the daemon upgrade order or the implementation of "staggered upgrades."
*   **Container Images**: Changes to default image paths (e.g., moving from Docker Hub to Quay.io) or adding support for new monitoring components.
*   **Health Checks**: Implementing new `CEPHADM_` health check codes or modifying the logic of existing ones (e.g., `CEPHADM_STRAY_HOST`).
*   **Host Prerequisites**: Changing required Python versions, container runtimes (Podman/Docker), or system dependencies like LVM2.
*   **Bootstrap Procedure**: Adding new flags to `cephadm bootstrap` or changing how SSH keys are generated and distributed.
*   **Logging/Monitoring Logic**: Changing default log paths, switching from stderr to journald, or adding new monitoring templates (Jinja2).
*   **Placement Algorithm**: Modifying how `count`, `label`, or `host_pattern` interact to select daemon targets.
*   **Hardware/Storage Interaction**: Changes in `ceph-volume` scan logic, `libstoragemgmt` integration, or OSD memory autotuning ratios.
*   **Security/Auth**: Modifications to `certmgr` renewal logic, SSL cipher defaults in `mgmt-gateway`, or OIDC provider support in `oauth2-proxy`.

## Key Technical Concepts
*   **Bootstrapping**: Initializing a single-node cluster to start the orchestration process.
*   **Service Specification (Spec)**: YAML-based declarations of desired service states.
*   **Placement Specification**: Logic for assigning daemons to hosts (labels, patterns, counts).
*   **Adoption**: Moving from "legacy" package-based daemons to "cephadm-style" containerized daemons.
*   **Staggered Upgrade**: Phased component updates to minimize downtime.
*   **Ingress Service**: High-availability stack combining Keepalived (VIP) and HAProxy.
*   **Maintenance Mode**: Safely stopping daemons on a host for hardware service.
*   **DriveGroups**: Declarative mapping of physical disk attributes to OSD configurations.
*   **Centralized Logging**: Integration with Loki and Alloy for log aggregation.
*   **Certmgr**: Root CA functionality within cephadm for SSL lifecycle management.

## Related Components
*   **MGR (Manager)**: The cephadm module runs inside the ceph-mgr process.
*   **ceph-volume**: Tool used by cephadm to provision and manage OSD storage devices.
*   **Podman / Docker**: The underlying container engines managed by cephadm.
*   **Systemd**: Used on the host to manage the lifecycle of containerized daemon units.
*   **Prometheus Stack**: Includes Prometheus, Node Exporter, Alertmanager, and Grafana.
*   **Nginx**: Powers the `mgmt-gateway` for unified application access.
*   **Keepalived / HAProxy**: Used for ingress/high-availability services.
*   **Samba / Ganesha**: External protocol gateways managed as Ceph services.