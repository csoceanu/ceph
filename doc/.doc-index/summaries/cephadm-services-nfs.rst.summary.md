This documentation provides a technical guide for deploying and managing **NFS Ganesha** services within a Ceph cluster using **cephadm**. It focuses on advanced orchestration beyond simple CLI commands, detailng service specifications, security, and high-availability configurations.

### 1. Primary Purpose
The file serves as the definitive reference for managing the lifecycle of the **NFS Ganesha service** via the Ceph Orchestrator (`cephadm`). It explains how to define, deploy, and secure NFS gateways and how to ensure their availability through ingress controllers.

### 2. Key Topics Covered
*   **Deployment Methods**: Using `ceph orch apply nfs` for quick setup and YAML-based service specifications for advanced configurations.
*   **Networking**: Binding NFS services to specific IP addresses, subnets, or custom ports.
*   **TLS/SSL Encryption**: Detailed configuration of transport-layer security, including certificate sources (inline, reference, or cephadm-signed) and kTLS (kernel TLS) performance optimizations.
*   **High Availability (HA)**: Implementing an `ingress` service using a Virtual IP (VIP), HAProxy, and Keepalived.
*   **Alternative HA Modes**: 
    *   VIP-only mode (Keepalived without HAProxy).
    *   HAProxy Protocol support (v5.0+) for preserving client IP addresses in exports.
*   **Monitoring**: Customizing monitoring ports and IP bindings for health checks.

### 3. Technical Keywords
*   **Service Types**: `nfs`, `ingress`.
*   **Components**: `nfs-ganesha`, `haproxy`, `keepalived`.
*   **Configuration Keys**: `virtual_ip`, `backend_service`, `enable_haproxy_protocol`, `keepalive_only`, `tls_ktls`.
*   **SSL Params**: `certificate_source`, `ssl_cert`, `ssl_key`, `ssl_ca_cert`, `tls_min_version`.
*   **APIs/Commands**: `ceph orch apply -i`, `ceph nfs export`, `ceph config-key get`.
*   **Protocols**: NFSv4 (only supported protocol).

### 4. Target Audience
*   **Storage Administrators**: Responsible for exposing CephFS or RGW via NFS.
*   **Systems Architects**: Designing highly available storage access layers.
*   **DevOps Engineers**: Automating Ceph infrastructure via YAML manifests and the Orchestrator CLI.

### 5. Related Concepts
*   **CephFS/RGW**: The underlying data sources that NFS exports usually point to.
*   **MGR NFS Module**: The simplified management layer for NFS clusters.
*   **Orchestrator (cephadm)**: The parent system managing the containers and placement of these daemons.
*   **Monitoring Stack**: Prometheus/Grafana (implied by the monitoring port configurations).

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
*   **CLI Changes**: Alterations to `ceph orch apply nfs` arguments.
*   **Schema Changes**: Additions or removals of fields in the `nfs` or `ingress` service specifications (e.g., new SSL parameters or networking options).
*   **Dependency Updates**: Upgrading the minimum supported version of NFS-Ganesha (e.g., if a feature requires v6.0+).
*   **Protocol Support**: If Ceph adds support for NFSv3 or updates TLS version defaults.
*   **Infrastructure Logic**: Changes in how `cephadm` handles Virtual IPs, Keepalived, or HAProxy integration.