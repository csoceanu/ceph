This documentation provides a comprehensive guide to managing physical or virtual hosts within a Ceph cluster using **cephadm** and the **orchestrator CLI (`ceph orch`)**. It covers the entire lifecycle of a host from addition and labeling to maintenance, tuning, and removal.

### 1. Primary Purpose
The file documents the operational procedures for managing cluster nodes. It serves as the authoritative reference for cluster administrators to scale their infrastructure, perform hardware maintenance, and ensure secure communication between the Ceph manager and cluster hosts.

### 2. Key Topics Covered
*   **Host Lifecycle**: Commands for adding hosts, listing cluster members, and safely removing hosts (including "draining" daemons).
*   **Filtering & Visibility**: Using regex patterns, labels, and status flags to query the host inventory.
*   **Maintenance Operations**: Procedures for entering/exiting maintenance mode to stop/start daemons safely and rescanning hardware for new devices.
*   **Host Labeling**: Using labels (including special internal labels starting with `_`) to control daemon placement and configuration distribution.
*   **Configuration Management**: Managing OS-level `sysctl` tuning profiles and CRUSH map locations via YAML specs.
*   **Security & Connectivity**: Managing SSH keys, custom SSH users (non-root), and CA-signed certificates for secure inter-node communication.

### 3. Technical Keywords
*   **CLI Commands**: `ceph orch host add|rm|ls`, `ceph orch host drain`, `ceph orch host label`, `ceph orch host maintenance`, `ceph orch tuned-profile`.
*   **Configuration Flags**: `--format yaml`, `--detail`, `--host-pattern`, `--labels`, `--rm-crush-entry`, `--zap-osd-devices`, `--force`.
*   **Special Labels**: `_admin`, `_no_schedule`, `_no_conf_keyring`, `_no_autotune_memory`.
*   **SSH Config**: `ssh-copy-id`, `ssh_identity_key`, `set-ssh-config`, `set-user`.
*   **Infrastructure Terms**: CRUSH map, FQDN (Fully Qualified Domain Name), sysctl, HBA (Host Bus Adapter), YAML spec.

### 4. Target Audience
*   **Storage Administrators**: Responsible for day-to-day cluster operations and scaling.
*   **Systems Engineers**: Focused on OS tuning, hardware maintenance, and security hardening.
*   **DevOps/SREs**: Automating cluster deployments using YAML specifications and orchestrator APIs.

### 5. Related Concepts
*   **Cephadm Orchestrator**: The underlying framework for managing Ceph services as containers.
*   **Service Specifications**: Related to how services are placed on the hosts managed here.
*   **OSD Removal**: Draining a host specifically triggers the OSD removal workflow.
*   **CRUSH Hierarchy**: Host management directly impacts how Ceph perceives the physical data distribution map.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **Orchestrator CLI Syntax**: Any changes to the `ceph orch host` or `ceph orch tuned-profile` command sub-structures.
2.  **Special Label Logic**: If new internal labels (starting with `_`) are added to the `cephadm` python module to control daemon behavior.
3.  **SSH Requirements**: Changes in how `cephadm` connects to hosts or updates to supported SSH authentication methods (e.g., new certificate types).
4.  **Hardware Interaction**: New methods for device discovery or changes to the `rescan` logic.
5.  **Host Requirements**: Any changes to the base OS dependencies required for a host to be "added" successfully.