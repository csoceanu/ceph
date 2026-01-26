# CEPHADM Documentation Index

## Overview
This documentation area covers **cephadm**, the native management and orchestration tool for Ceph clusters starting from version 15.2.0 (Octopus). It details the full lifecycle management of a cluster, including bootstrapping, host management, daemon deployment via service specifications, automated upgrades, and integrated monitoring and troubleshooting.

## Files Summary
*   **upgrade.rst**: Procedures for automated and staggered Ceph upgrades, monitoring progress, and handling upgrade-specific health alerts.
*   **compatibility.rst**: Support matrices for Podman versions and a status report on the stability of developing features.
*   **certmgr.rst**: Management of the Root CA and SSL/TLS certificates for cluster services (Self-signed vs. User-provided).
*   **troubleshooting.rst**: Guide for debugging containerized daemons, recovering quorum, manual manager deployment, and using gdb/core dumps.
*   **client-setup.rst**: Basic configuration for client hosts, including minimal `ceph.conf` and keyring generation.
*   **host-management.rst**: Operations for adding, removing, draining, and labeling hosts, plus OS-level tuning via sysctl profiles.
*   **adoption.rst**: Workflow for converting legacy clusters (deployed via ceph-ansible, DeepSea, etc.) to cephadm management.
*   **index.rst**: Main landing page and table of contents for the cephadm documentation section.
*   **operations.rst**: Day-to-day management tasks including daemon control (start/stop/redeploy), logging configurations, and custom health checks.
*   **install.rst**: Prerequisites and bootstrap procedures for new clusters, including air-gapped and custom SSH configurations.
*   **services/rgw.rst**: Deployment of RGW daemons, multisite configurations, and high availability via the ingress service.
*   **services/snmp-gateway.rst**: Integration with SNMP management platforms for alert forwarding from Prometheus.
*   **services/monitoring.rst**: Setup of the Prometheus/Grafana/Alertmanager stack, Loki/Alloy logging, and security/template customization.
*   **services/mds.rst**: Deployment of Metadata Servers (MDS) for CephFS volumes.
*   **services/mgmt-gateway.rst**: Configuration of the Nginx-based management gateway for unified, secure access to dashboards.
*   **services/oauth2-proxy.rst**: Integration with external Identity Providers (IDPs) via OIDC for cluster authentication.
*   **services/smb.rst**: Deployment and configuration of Samba containers for SMB share hosting on CephFS.
*   **services/iscsi.rst**: Deployment of iSCSI gateways and target configuration.
*   **services/nfs.rst**: Deployment of NFS Ganesha gateways and high-availability ingress configurations.
*   **services/tracing.rst**: Jaeger tracing backend deployment using Elasticsearch.
*   **services/mgr.rst**: Specifics for the Manager (MGR) service, including co-location and network binding.
*   **services/osd.rst**: Comprehensive OSD lifecycle management, including device scanning, OSDSpecs (DriveGroups), and memory autotuning.
*   **services/mon.rst**: Monitor (MON) deployment, network subnetting, and CRUSH location management.
*   **services/index.rst**: Overview of service status, daemon placement logic (count, label, patterns), and YAML service specifications.
*   **services/custom-container.rst**: Specification format for deploying non-Ceph arbitrary containers with init-containers support.

## Code Changes That Would Require Documentation Updates
*   **CLI Changes**: Adding, renaming, or removing commands under `ceph orch ...` or `cephadm ...`.
*   **Service Specification Schema**: Changes to the YAML structure for any `ServiceSpec` (e.g., adding `extra_container_args`, `networks`, or service-specific fields like `rgw_frontend_port`).
*   **Daemon Deployment Logic**: Modifications to how cephadm selects hosts (placement logic), handles co-location, or orders daemon restarts during upgrades.
*   **Container Images**: Changes to default container images (Prometheus, Grafana, Nginx, etc.) or the logic for pulling/mapping versions.
*   **Dependency Requirements**: Changes in minimum required versions for Python, Podman, Docker, or Linux kernel versions.
*   **Health Checks**: Addition of new `CEPHADM_` health check codes or modifications to existing check logic (e.g., MTU or Linkspeed checks).
*   **Hardware/Storage Handling**: Changes in how `ceph-volume` scans devices or how `libstoragemgmt` interacts with disk LEDs and health status.
*   **Security/Auth Modules**: Modifications to `certmgr` behavior, OIDC/OAuth2 integration flows, or Nginx proxy configurations.
*   **Support for New Services**: Code implementing management for new daemon types (e.g., adding a new `service_type`).
*   **Bootstrap/Install Logic**: Changes to the `cephadm bootstrap` flags, SSH key generation, or initial cluster setup steps.

## Key Technical Concepts
*   **Orchestrator CLI**: `ceph orch` commands (ls, ps, apply, device ls, host add, upgrade).
*   **Service Specification (Spec)**: YAML-based declarations for MDS, RGW, OSD, etc.
*   **Bootstrap**: The initial creation of a cluster from a single node.
*   **Placement Spec**: Logic for deploying daemons based on `hosts`, `labels`, `count`, or `host_pattern`.
*   **DriveGroups / OSDSpec**: Filters for automating OSD creation based on device properties (size, model, type).
*   **Ingress Service**: High availability wrapper for RGW/NFS using Keepalived and HAProxy.
*   **Adoption**: Converting "legacy" (non-containerized) daemons to cephadm management.
*   **Drain/Maintenance Mode**: Safely removing daemons or pausing a host for hardware service.
*   **Certmgr**: Cephadm's internal certificate authority and rotation system.
*   **Staggered Upgrade**: Limiting upgrade scope to specific daemon types, services, or hosts.

## Related Components
*   **ceph-mgr modules**: `cephadm`, `prometheus`, `dashboard`, `nfs`, `smb`.
*   **Container Runtimes**: Podman, Docker.
*   **Storage Tools**: `ceph-volume`, `libstoragemgmt`, LVM2.
*   **Networking**: Keepalived, HAProxy, Nginx.
*   **Monitoring/Observability**: Prometheus, Grafana, Alertmanager, Loki, Alloy, Jaeger, SNMP.
*   **External IDPs**: Dex, Keycloak (via OAuth2-proxy).
*   **Protocols**: S3/Swift (RGW), NFSv4, SMB, iSCSI.