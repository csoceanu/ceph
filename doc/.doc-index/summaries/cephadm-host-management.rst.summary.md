This documentation file, `cephadm/host-management.rst`, serves as the definitive guide for managing the lifecycle of physical or virtual nodes within a Ceph cluster managed by **cephadm**.

### 1. Primary Purpose
The file documents how to interact with the **Ceph Orchestrator (ceph-mgr)** to perform host-level operations. It ensures that the Ceph cluster knows which machines are available for deploying daemons, how to authenticate with them via SSH, and how to tune their underlying operating systems.

### 2. Key Topics Covered
*   **Host Lifecycle**: Commands for listing, adding (including bulk addition via YAML), and removing hosts.
*   **Drain & Maintenance**: Procedures for safely evacuating daemons from a host for hardware service without risking data loss.
*   **Labeling System**: Using labels to control daemon placement and identifying "special" labels that change cephadm's behavior (e.g., `_no_schedule`).
*   **Hardware Management**: Rescanning storage adapters (HBAs) and setting initial CRUSH locations for proper data distribution.
*   **OS Tuning Profiles**: Managing `sysctl` settings across multiple hosts using centralized YAML specifications.
*   **Security & Connectivity**: Managing SSH keys (including CA-signed certs), custom SSH configurations, and non-root user access.
*   **Naming Conventions**: Strict requirements regarding Hostnames vs. Fully Qualified Domain Names (FQDN).

### 3. Technical Keywords
*   **CLI Commands**: `ceph orch host`, `ceph orch apply -i`, `ceph orch device rescan`, `ceph orch tuned-profile`.
*   **Configuration Flags**: `--drain`, `--zap-osd-devices`, `--rm-crush-entry`, `--force`, `--offline`.
*   **Special Labels**: `_admin`, `_no_schedule`, `_no_conf_keyring`, `_no_autotune_memory`.
*   **APIs/Config Keys**: `mgr/cephadm/ssh_identity_key`, `mgr/cephadm/ssh_config_file`, `osd_memory_target_autotune`.
*   **Files/Paths**: `/etc/ceph/`, `/etc/sysctl.d/`, `/var/lib/ceph/<fsid>/config`.

### 4. Target Audience
*   **Storage Administrators**: Responsible for cluster expansion and maintenance.
*   **Site Reliability Engineers (SREs)**: Automating cluster deployments using YAML specs.
*   **Systems Architects**: Designing CRUSH hierarchies and hardware placement strategies.

### 5. Related Concepts
*   **Cephadm Orchestrator**: The backend engine driving these changes.
*   **CRUSH Maps**: The algorithm used for data placement, which is influenced by the host "location" attribute.
*   **OSD Management**: Removing a host is inextricably linked to the removal of OSDs.
*   **Service Specifications**: Host management provides the foundation for "Service Specs" (Placement blocks).

---

### When to update this file
This file must be updated if code changes occur in the following areas:
1.  **Orchestrator CLI**: If new flags are added to `ceph orch host` or if the output format of `host ls --detail` changes.
2.  **Cephadm Logic**: If new "Special Labels" (starting with `_`) are introduced to control daemon behavior.
3.  **Authentication**: If the SSH key management workflow changes or if support for new authentication methods (beyond CA-signed keys) is added.
4.  **Hardware Interaction**: If the logic for scanning devices or interacting with `sysctl` via "Tuned Profiles" is modified.
5.  **Placement Logic**: If the way `location` attributes map to CRUSH buckets is altered.