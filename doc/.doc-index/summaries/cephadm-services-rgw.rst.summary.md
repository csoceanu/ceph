This documentation provides a technical guide for deploying and managing the **Ceph Object Gateway (RGW)** and its associated **High Availability (Ingress)** service using `cephadm`.

### 1. Primary Purpose
The file serves as the definitive operational guide for RGW lifecycle management within a Ceph cluster managed by `cephadm`. It explains how to deploy RGW daemons, configure multisite environments, secure traffic via SSL/TLS, and establish high-availability load balancing.

### 2. Key Topics Covered
*   **Deployment Strategies**: Instructions for trivial (single-cluster) setups and advanced "designated gateway" deployments using host labels.
*   **Service Configuration**: Using YAML specifications to define networks, ports, and frontend arguments (e.g., Beast frontend settings).
*   **Multisite Operations**: Deploying RGWs within specific realms and zones, including how to disable sync traffic for dedicated I/O daemons.
*   **Security & SSL/TLS**: Comprehensive management of HTTPS certificates via `certmgr`, supporting inline, referenced, or `cephadm`-signed certificates, including wildcard SANs.
*   **Performance & Hardware Offload**: Enabling Intel QAT (QuickAssist Technology) for hardware-accelerated compression.
*   **Operational Lifecycle**: Managing "graceful shutdown" by draining client connections using `rgw_exit_timeout_secs`.
*   **Ingress (High Availability)**: Deploying a combination of **HAProxy** and **Keepalived** to provide a virtual floating IP (VIP) and load balancing across RGW daemons.

### 3. Technical Keywords
*   **Commands**: `ceph orch apply rgw`, `ceph orch host label add`, `radosgw-admin`, `ceph orch apply ingress`.
*   **APIs/Specs**: `RGWSpec`, `service_type: rgw`, `service_type: ingress`.
*   **Configuration Options**: `rgw_frontend_port`, `rgw_frontend_extra_args`, `rgw_realm`, `rgw_zone`, `rgw_run_sync_thread`, `rgw_exit_timeout_secs`.
*   **Networking/HA**: `virtual_ip`, `keepalived`, `haproxy`, `vrrp_interface_network`, `beast` (frontend type).
*   **Hardware**: `qat` (compression offload).

### 4. Target Audience
*   **Storage Administrators**: Responsible for deploying and scaling object storage.
*   **DevOps/SREs**: Implementing high availability and automated infrastructure-as-code deployments.
*   **Security Engineers**: Configuring SSL/TLS termination and certificate rotation for storage endpoints.

### 5. Related Concepts
*   **Multisite Replication**: Relates to Ceph’s global object namespaces (Realms/Zones).
*   **Ceph Orchestrator (`ceph orch`)**: The underlying framework for all commands in this file.
*   **S3/Swift API**: The protocols served by the gateways documented here.
*   **Certificate Manager (`certmgr`)**: The internal Ceph service handling the SSL logic.

---

### Update Triggers for AI Systems
This file should be updated if any of the following code changes occur:
1.  **Schema Changes in `RGWSpec` or `IngressSpec`**: If new fields are added to the Python classes in `ceph.deployment.service_spec`.
2.  **CLI Command Evolution**: If the `ceph orch apply rgw` syntax or arguments change.
3.  **New Frontend Features**: If the `radosgw` binary adds new supported frontend types or critical performance flags.
4.  **Ingress Logic Updates**: If `cephadm` changes how it orchestrates `haproxy` or `keepalived` (e.g., shifting from unicast to multicast by default).
5.  **Hardware Support**: Expansion of the `qat` block or addition of other hardware acceleration types (e.g., Zlib offload).