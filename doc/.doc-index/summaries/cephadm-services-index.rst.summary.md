This documentation file, `cephadm/services/index.rst`, serves as the **central orchestration guide for managing Ceph services and daemons** using the `cephadm` framework. It defines the lifecycle, placement logic, and configuration schemas for all cluster components.

### 1. Primary Purpose
The file provides the high-level interface and conceptual framework for the Ceph Orchestrator (`ceph orch`). it documents how to define, deploy, view, update, and remove services (groups of daemons) and individual daemons across a Ceph cluster.

### 2. Key Topics Covered
*   **Service & Daemon Monitoring**: How to check the status of services (`ls`) and individual process instances (`ps`).
*   **Service Specifications (YAML)**: The declarative syntax used to define service types, IDs, placement, and network settings.
*   **Placement Logic**: Comprehensive rules for where daemons are deployed using explicit hostnames, host labels, or pattern matching (regex/fnmatch).
*   **Advanced Container Configuration**: Mechanisms to pass extra arguments to the container runtime (`extra_container_args`) or the entrypoint process (`extra_entrypoint_args`).
*   **Custom Configuration Management**: Mounting arbitrary files and custom configuration snippets into containers via `custom_configs`.
*   **Management Modes**: Switching between "Managed" (automated reconciliation) and "Unmanaged" (manual deployment) states.

### 3. Technical Keywords
*   **Core Commands**: `ceph orch ls`, `ceph orch ps`, `ceph orch apply`, `ceph orch rm`, `ceph orch redeploy`.
*   **Configuration Options**: `mgr/cephadm/daemon_cache_timeout`, `unmanaged: true/false`.
*   **Specification Keys**: `service_type`, `service_id`, `placement`, `count`, `label`, `host_pattern`, `extra_container_args`, `custom_configs`.
*   **Service Types**: `mon`, `mgr`, `osd`, `rgw`, `mds`, `nfs`, `iscsi`, `monitoring`, `snmp-gateway`, `smb`, `mgmt-gateway`.
*   **Health Warnings**: `CEPHADM_INVALID_CONFIG_OPTION`, `CEPHADM_FAILED_SET_OPTION`.

### 4. Target Audience
*   **Cloud/Storage Administrators**: Responsible for deploying and scaling Ceph clusters.
*   **SREs/DevOps Engineers**: Automating cluster deployments using YAML specifications.
*   **Ceph Developers**: Looking for the standard interface to integrate new service types into the orchestrator.

### 5. Related Concepts
*   **Drive Groups**: Related to OSD service specifications.
*   **Systemd**: Daemons are managed as systemd units on host machines.
*   **Container Engines**: Logic applies to how Ceph interacts with Podman or Docker.
*   **Ceph Manager (MGR) Modules**: This documentation specifically concerns the `cephadm` manager module's behavior.

---

### When to Update This File (AI Guidance)
An AI or developer should update this file if changes occur in the following areas of the Ceph codebase:

1.  **Orchestrator CLI Changes**: If new flags are added to `ceph orch ls`, `ps`, or `apply`.
2.  **Schema Modifications**: If the `ServiceSpec` Python class (in `ceph.deployment.service_spec`) adds new fields (e.g., new networking parameters or resource limiters).
3.  **New Service Integration**: When a new service type (like a new gateway or proxy) is added to the Ceph ecosystem.
4.  **Placement Algorithm Updates**: If the logic for how `cephadm` selects hosts or handles co-location of daemons is modified.
5.  **Container Runtime Integration**: If there are changes in how `cephadm` handles container arguments, environment variables, or volume mounts.
6.  **Default Values**: If internal timeouts (like the `daemon_cache_timeout`) or default management behaviors change.