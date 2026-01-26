This documentation provides a technical guide for deploying and managing **NFS Ganesha** services within a Ceph cluster using **cephadm**. It focuses on manual service orchestration, high-availability configurations, and security via TLS.

### 1. Primary Purpose
The file serves as the authoritative reference for managing NFS services through the **Ceph Orchestrator (cephadm)**. While it points to simpler management methods (MGR-based), its specific purpose is to document **unusual or advanced configurations**, including manual YAML-based service specifications, networking overrides, and High Availability (HA) setups.

### 2. Key Topics Covered
*   **Deployment Methods**: Using the CLI (`ceph orch apply nfs`) and YAML-based service specifications.
*   **Networking Configuration**: Binding NFS and monitoring services to specific ports, IP addresses, or subnets.
*   **TLS/SSL Encryption**: Configuring secure transport using internal CAs, inline PEM certificates, or external certificate managers.
*   **High Availability (HA)**: 
    *   Deploying an **Ingress service** (HAProxy + Keepalived) to provide a Virtual IP (VIP).
    *   **Keepalived-only mode**: Using a VIP without an HAProxy load balancer.
    *   **HAProxy Protocol support**: Enabling client IP visibility for exports (requires Ganesha v5.0+).

### 3. Technical Keywords
*   **APIs/Services**: `service_type: nfs`, `service_type: ingress`, `NFS Ganesha`, `HAProxy`, `Keepalived`.
*   **Configuration Options**: 
    *   `port`, `virtual_ip`, `backend_service`.
    *   `ssl`, `certificate_source` (`cephadm-signed`, `inline`, `reference`).
    *   `tls_ktls` (Kernel TLS), `tls_min_version`, `tls_ciphers`.
    *   `enable_haproxy_protocol`, `keepalive_only`.
*   **Commands**: 
    *   `ceph orch apply nfs <svc_id>`
    *   `ceph orch apply -i <file.yaml>`
    *   `ceph config-key get mgr/cephadm/ingress...`

### 4. Target Audience
*   **Ceph Cluster Administrators**: Those needing to expose Ceph storage (typically CephFS) via NFSv4.
*   **System Architects**: Designing high-availability storage gateways.
*   **DevOps/SREs**: Automating storage deployments via YAML manifests.

### 5. Related Concepts
*   **CephFS**: The primary backend filesystem usually exported via these NFS gateways.
*   **MGR NFS Module**: The simplified high-level interface for managing NFS exports.
*   **Ceph Orchestrator**: The underlying framework (`cephadm`) that manages the lifecycle of these containers.
*   **Ingress Service**: A related Ceph service type used to provide load balancing for multiple Ceph daemons.

---

### Triggering Updates: When to Edit This File
This file should be updated if code changes occur in the following areas:
1.  **Orchestrator Logic**: Changes to how `cephadm` parses `service_type: nfs` or `service_type: ingress` specifications.
2.  **Ganesha Integration**: If a new version of NFS-Ganesha introduces required parameters or deprecates current ones (e.g., changes to TLS handling).
3.  **Networking Changes**: Updates to how Ceph handles VIPs, network isolation, or backend port mapping.
4.  **Security/TLS**: Addition of new certificate sources, cipher suites, or encryption protocols (like updates to kTLS support).
5.  **CLI Changes**: Any modifications to the `ceph orch apply nfs` command arguments.