This documentation file, `rados/operations/user-management.rst`, serves as the definitive guide for managing identities and access control within a Ceph Storage Cluster. It details the **Cephx** authentication system, user types, and the granular "capability" (caps) system used to authorize actions across different Ceph daemons.

### 1. Primary Purpose
The file documents how to manage **Ceph Client users**, specifically focusing on their **authentication** (proving identity via secret keys) and **authorization** (defining what they can do via capabilities). It provides instructions for cluster administrators to create, modify, and delete users, and explains how clients use keyrings to interact with the cluster securely.

### 2. Key Topics Covered
*   **User Concept**: Definitions of users as system actors or individuals; the `TYPE.ID` nomenclature (e.g., `client.admin`).
*   **Authorization (Capabilities/Caps)**: Detailed syntax for granting permissions to specific daemons:
    *   **Monitors (mon)**: Access to cluster maps and auth operations.
    *   **OSDs (osd)**: Read/write/execute permissions for specific pools, namespaces, or application tags.
    *   **Managers (mgr)**: Permissions for specific modules or command prefixes.
    *   **MDS (mds)**: Access for CephFS clients.
*   **Capability Profiles**: Pre-defined permission sets (e.g., `profile rbd`, `profile bootstrap-osd`, `profile simple-rados-client`).
*   **Logical Partitioning**: Use of **Pools** and **Namespaces** to segregate data and restrict user access.
*   **User Management Operations**: CLI commands to list (`ls`), get (`get`), add (`add`), and delete (`del`) users.
*   **Keyring Management**: Procedures for creating keyring files, importing/exporting keys, and managing local client authentication files.
*   **Security & Limitations**: Warnings regarding plaintext key storage and the lack of in-flight message encryption.

### 3. Technical Keywords
*   **APIs/Tools**: `ceph auth`, `ceph-authtool`, `librados`.
*   **Commands**: `ceph auth get-or-create`, `ceph auth caps`, `ceph auth import`, `ceph auth rotate`.
*   **Configuration/Options**: `--id`, `--user`, `--name`, `-n`, `--keyring`, `CEPH_ARGS` (environment variable).
*   **Daemons**: `mon`, `osd`, `mgr`, `mds`.
*   **Access Specifiers**: `r`, `w`, `x`, `class-read`, `class-write`, `*`, `all`.
*   **Profiles**: `bootstrap-osd`, `bootstrap-mds`, `rbd`, `fs-client`, `role-definer`.

### 4. Target Audience
*   **Ceph Cluster Administrators**: Responsible for cluster security and user provisioning.
*   **DevOps/System Engineers**: Integrating applications with Ceph storage.
*   **Security Auditors**: Understanding the Ceph permissions model and its limitations.

### 5. Related Concepts
*   **Cephx**: The underlying authentication protocol.
*   **CRUSH Map**: Retrievable only by users with `mon 'allow r'` caps.
*   **RADOS Gateway (RGW)**: Acts as a Ceph client itself; has its own internal user management for S3/Swift users.
*   **CephFS**: Uses POSIX semantics but relies on Cephx for client-to-MDS/OSD communication.
*   **Block Devices (RBD)**: Requires specific profiles for image manipulation and blacklisting.

---

### Update Triggers for AI Maintenance
Update this file if code changes occur in the following areas:
1.  **CLI Syntax**: If the `ceph auth` command or its subcommands (add, caps, ls) are modified.
2.  **Capability Logic**: If new access specifiers (beyond `r, w, x, class-read`) or new matchers (e.g., beyond `pool`, `namespace`, `network`) are added to the authorization engine.
3.  **New Profiles**: If the Ceph source code introduces new pre-defined capability profiles (e.g., for new manager modules or daemon types).
4.  **Security Protocols**: If Ceph introduces in-flight encryption or changes the `cephx` handshake.
5.  **Default Paths**: If the default search paths for keyrings (currently `/etc/ceph/`) are changed.