This documentation file, `rados/operations/user-management.rst`, is the definitive guide for managing identities and access control within a Ceph Storage Cluster. It explains the relationship between users, keys, and capabilities (permissions) required to interact with Ceph daemons (MON, OSD, MDS, MGR).

### 1. Primary Purpose
The file documents the **Cephx authentication and authorization system**. It provides instructions for cluster administrators on how to create, modify, and delete users, manage their secret keys via keyrings, and define granular access permissions (capabilities) for various storage resources.

### 2. Key Topics Covered
*   **User Concept**: Definitions of "types" (usually `client`) and IDs (e.g., `client.admin`).
*   **Authorization (Capabilities)**: Syntax and logic for "caps" that govern access to Monitors, OSDs, Managers, and Metadata Servers.
*   **Storage Segregation**: How permissions relate to Pools, Namespaces, and Application Tags.
*   **User Management Operations**: CLI procedures for listing (`ls`), creating (`add`, `get-or-create`), modifying (`caps`), and deleting (`del`) users.
*   **Keyring Management**: Using `ceph-authtool` to create and manage local keyring files and importing/exporting keys between clients and the cluster.
*   **Security Architecture**: Best practices for key storage, key rotation, and the limitations of the Cephx protocol (e.g., lack of in-transit encryption).

### 3. Technical Keywords
*   **APIs/Protocols**: `cephx`, `librados`.
*   **Commands**: `ceph auth`, `ceph-authtool`, `rbd map`, `ceph-volume`, `cephadm`.
*   **Configuration Options**: `keyring`, `CEPH_ARGS`, `--id`, `--name`, `--caps`.
*   **Capability Keywords**: `allow`, `r`, `w`, `x`, `class-read`, `class-write`, `profile`, `pool`, `namespace`.
*   **Profiles**: `profile osd`, `profile rbd`, `profile bootstrap-osd`, `profile simple-rados-client`.

### 4. Target Audience
*   **Storage Administrators**: Responsible for cluster security and user provisioning.
*   **System Integrators**: Developers connecting external applications (like OpenStack, Kubernetes, or custom `librados` apps) to Ceph.
*   **DevOps/SREs**: Automating deployment and key distribution using tools like `cephadm`.

### 5. Related Concepts
*   **Cephx Config Reference**: Deep dive into authentication settings.
*   **RADOS Gateway (RGW)**: While RGW uses Ceph users to talk to the cluster, it has a separate user management system for its own S3/Swift end-users.
*   **CephFS Client Auth**: Specific authorization logic for filesystem access.
*   **Ceph Architecture**: High-level details on how Ceph handles high-availability authentication.

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **New Daemon Types**: If a new daemon (beyond mon, osd, mds, mgr) is added to the architecture.
2.  **Capability Syntax**: If the parser for capabilities is modified to support new filters, operators, or keywords (e.g., new network CIDR restrictions).
3.  **New Profiles**: If new predefined capability profiles (like `profile rbd`) are introduced in the source code.
4.  **CLI Changes**: If the `ceph auth` or `ceph-authtool` command-line interfaces are modified (new flags or deprecated subcommands).
5.  **Keyring Defaults**: If the default search paths or naming conventions for `.keyring` files change.
6.  **Security Protocols**: If Ceph adds support for message encryption in transit or changes the underlying `cephx` handshake.