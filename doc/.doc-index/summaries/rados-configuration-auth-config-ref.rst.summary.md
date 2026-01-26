This documentation file serves as the definitive configuration reference for **CephX**, the proprietary authentication protocol used by the Ceph distributed storage system.

### 1. Primary Purpose
The file provides technical instructions and configuration specifications for enabling, disabling, and managing CephX authentication. It details how Ceph ensures secure communication between clients and cluster daemons (Monitors, OSDs, MDS, and MGRs) and establishes the requirements for cryptographic key management.

### 2. Key Topics Covered
*   **Deployment Scenarios**: Distinguishes between automated deployments (via `cephadm`) and manual/third-party deployments (Ansible, Chef, etc.).
*   **Enablement Procedures**: Step-by-step commands for generating keys for various daemon types (`mon`, `mgr`, `osd`, `mds`) and the `client.admin` user.
*   **Key Management**: Instructions on keyring search paths, file locations, and manual key distribution using `scp`.
*   **Security Policies**: Configuration of requirements for cluster-to-cluster, service-to-client, and client-to-service authentication.
*   **Message Signatures**: Configuration for preventing man-in-the-middle attacks through message signing (not to be confused with full encryption).
*   **Daemon Capabilities**: Standard permission profiles (capabilities) required for each Ceph daemon type.

### 3. Technical Keywords
*   **Protocols/Services**: `CephX`, `RADOS`, `ceph-mon`, `ceph-osd`, `ceph-mgr`, `ceph-mds`.
*   **APIs/Commands**: `ceph auth get-or-create`, `ceph-authtool`, `ceph auth get-or-create-key`.
*   **Configuration Options**: 
    *   `auth_cluster_required`, `auth_service_required`, `auth_client_required` (Options: `cephx`, `none`).
    *   `keyring`, `keyfile`, `key`.
    *   `cephx_require_signatures`, `cephx_cluster_require_signatures`, `cephx_sign_messages`.
    *   `auth_service_ticket_ttl`.
*   **Paths**: `/etc/ceph/`, `/var/lib/ceph/`, `$mon_data/keyring`.

### 4. Target Audience
*   **System Administrators**: Responsible for cluster security and manual deployment.
*   **DevOps Engineers**: Integrating Ceph with automation tools like Ansible or Puppet.
*   **Security Auditors**: Assessing the authentication and tampering protection of the storage cluster.

### 5. Related Concepts
*   **User Management**: Relates to how individual users and permissions are handled once authentication is enabled.
*   **High Availability (HA)**: Relates to the architecture of authenticating against a distributed monitor cluster.
*   **Bootstrap**: The initial process of bringing a cluster from a raw state to a functional, authenticated state.

---

### Triggering Updates: When to revise this file
This documentation should be updated if any of the following code-level changes occur:
1.  **Defaults Change**: If the default value of `auth_*` settings or `cephx_require_signatures` shifts (e.g., moving toward more restrictive security defaults).
2.  **CLI Syntax**: If the `ceph auth` or `ceph-authtool` command arguments change or are deprecated.
3.  **Path Refactoring**: If the default search paths for keyrings (currently `/etc/ceph/`) or the internal daemon data directory structures change.
4.  **New Daemon Types**: If a new core daemon is introduced (similar to when `mgr` was added), it must be documented here with its specific capabilities and keyring requirements.
5.  **Signature Support**: If the protocol evolves to support full in-flight encryption or new cryptographic signing methods.