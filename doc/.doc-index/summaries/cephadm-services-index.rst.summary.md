This documentation file serves as the primary technical reference for **Service Management** within a Ceph cluster using **cephadm** (the orchestrator). It defines how logical services (groups of daemons) are declared, deployed, monitored, and modified.

### 1. Primary Purpose
The file provides the foundational concepts and operational commands for the Ceph Orchestrator (`ceph orch`). It explains the lifecycle of services and daemons, transitioning from manual command-line execution to a **declarative, YAML-based infrastructure-as-code (IaC) model**.

### 2. Key Topics Covered
*   **Service & Daemon Status**: How to query the orchestrator to see what services are running and the health of individual systemd-based daemons.
*   **Service Specifications (Spec)**: The schema for defining services, including networking, configuration parameters, and persistence.
*   **Placement Specifications**: Logic for determining where daemons live based on explicit hostnames, host labels, or shell-style/regex patterns.
*   **Scalability & Co-location**: Instructions on setting daemon counts and running multiple instances (e.g., RGW/MDS) on a single host.
*   **Customization**: Methods for injecting extra container/entrypoint arguments and mounting custom configuration files into containers.
*   **Managed vs. Unmanaged Modes**: How to toggle between cephadm’s automatic reconciliation loop and manual daemon management.

### 3. Technical Keywords
*   **APIs/CLI**: `ceph orch ls`, `ceph orch ps`, `ceph orch apply`, `ceph orch rm`, `ceph orch redeploy`.
*   **Configuration**: `service_type`, `service_id`, `placement`, `unmanaged`, `extra_container_args`, `custom_configs`.
*   **Components**: `mon`, `mgr`, `osd`, `rgw`, `mds`, `nfs`, `iscsi`, `prometheus`, `node-exporter`.
*   **Variables**: `mgr/cephadm/daemon_cache_timeout`.
*   **Health Checks**: `CEPHADM_INVALID_CONFIG_OPTION`, `CEPHADM_FAILED_SET_OPTION`.

### 4. Target Audience
*   **Storage Administrators**: For day-to-day cluster operations and scaling.
*   **DevOps/SREs**: For automating Ceph deployments using YAML manifests.
*   **Support Engineers**: For troubleshooting daemon placement issues and status mismatches.

### 5. Related Concepts
*   **Systemd**: Daemons are managed as systemd units on host nodes.
*   **Container Runtimes**: Works with Docker or Podman (referenced via `extra_container_args`).
*   **YAML/Declarative State**: The orchestrator functions as a reconciliation loop similar to Kubernetes Controllers.
*   **DriveGroups**: The specific subset of service specs used for OSD (Disk) management.

---

### Update Triggers for AI Maintenance
This file should be updated if any of the following code changes occur:
1.  **Orchestrator CLI Changes**: If new flags are added to `ceph orch` or existing command syntax is deprecated.
2.  **Spec Schema Evolution**: If the `ServiceSpec` Python class (in `ceph.deployment.service_spec`) adds new fields (e.g., new networking options or resource limits).
3.  **New Service Types**: If a new service (like a new gateway or monitoring tool) is added to the Ceph ecosystem.
4.  **Cephadm Logic Changes**: If the algorithm for daemon placement (host selection priority) is modified.
5.  **Default Values**: If the default `daemon_cache_timeout` or health check triggers are changed in the `mgr/cephadm` module.