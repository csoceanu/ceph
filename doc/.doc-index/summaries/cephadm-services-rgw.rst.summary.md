This documentation provides a technical guide for deploying and managing the **Ceph Object Gateway (RGW)** and its associated **High Availability (Ingress)** service using `cephadm`.

### 1. Primary Purpose
The file serves as the definitive reference for orchestrating RGW services within a Ceph cluster. It outlines how to deploy RGW daemons, configure multisite environments, manage SSL certificates via `certmgr`, and set up high-availability load balancing using HAProxy and Keepalived.

### 2. Key Topics Covered
*   **RGW Deployment**: Command-line and YAML-based deployment of RGW daemons, including "trivial" setups and designated gateway configurations.
*   **Networking & Frontend**: Configuring network bindings and passing specific arguments to the RGW "Beast" frontend.
*   **Multisite Configuration**: Integration with realms, zones, and zonegroups; specifically how to deploy daemons and disable sync traffic for dedicated I/O.
*   **Security (HTTPS/SSL)**: Three methods for managing certificates: `cephadm-signed`, `inline`, and `reference`. Includes support for Wildcard SANs.
*   **Performance & Lifecycle**: QAT (QuickAssist Technology) hardware offloading and "graceful shutdown" (draining client connections) via exit timeouts.
*   **Ingress Service**: Deployment of a High Availability layer using a Virtual IP (VIP), HAProxy, and Keepalived to load balance RGW traffic.

### 3. Technical Keywords
*   **Commands**: `ceph orch apply rgw`, `ceph orch apply ingress`, `radosgw-admin`, `ceph orch host label`.
*   **APIs/Specs**: `RGWSpec`, `service_type: rgw`, `service_type: ingress`.
*   **Configuration Options**: `rgw_frontend_port`, `rgw_frontend_extra_args`, `rgw_exit_timeout_secs`, `rgw_run_sync_thread`, `wildcard_enabled`, `qat`.
*   **Ingress Params**: `virtual_ip`, `backend_service`, `keepalived`, `haproxy`, `vrrp_interface_network`.
*   **SSL Sources**: `inline`, `reference`, `cephadm-signed`.

### 4. Target Audience
*   **Cloud Architects**: Designing scalable object storage solutions.
*   **System Administrators**: Responsible for the deployment, scaling, and maintenance of Ceph clusters.
*   **DevOps/SREs**: Implementing CI/CD for infrastructure and managing high-availability endpoints.

### 5. Related Concepts
*   **Multisite (Realms/Zones)**: The architectural framework for geographic replication.
*   **Cephadm Certificate Manager (certmgr)**: The backend utility managing the lifecycle of service certificates.
*   **Placement Specifications**: Logic used by the Ceph Orchestrator to decide which hosts run which daemons.
*   **OS Networking**: CIDR subnets, Virtual IPs (VIP), and VRRP communication.

---

### Update Triggers for AI Maintenance
This file should be updated if any of the following code-level changes occur:
1.  **Orchestrator Logic**: If the `ceph orch apply rgw` command gains new flags or if the `RGWSpec` Python class is modified.
2.  **RGW Frontend**: Changes to the default "Beast" engine or new supported `rgw_frontends` arguments.
3.  **Security/SSL**: Changes in how `certmgr` handles certificate rotation or the addition of new `certificate_source` types.
4.  **Hardware Offloading**: Updates to QAT support or the introduction of new hardware acceleration types in the spec.
5.  **Ingress Componentry**: If `cephadm` switches from HAProxy/Keepalived to another LB solution (e.g., Nginx or Envoy) or if the VRRP logic changes.
6.  **Default Values**: Any change to the default values of timeouts (like `rgw_exit_timeout_secs`) or ports.