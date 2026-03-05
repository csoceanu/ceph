This documentation file serves as the definitive guide for managing **SMB (Server Message Block) access to CephFS** via the Ceph Manager (`mgr/smb`) module. It details the lifecycle management of Samba clusters and shares within a Ceph environment.

### 1. Primary Purpose
The file documents the **SMB Manager Module**, which automates the deployment, configuration, and management of Samba services running in containers. It enables CephFS to be exported as SMB shares to Windows, macOS, and Linux clients, supporting both standalone (user-based) and Active Directory-integrated authentication.

### 2. Key Topics Covered
*   **Management Paradigms**: Covers both the **Imperative** (CLI commands like `ceph smb cluster create`) and **Declarative** (YAML/JSON resource specs via `ceph smb apply`) management styles.
*   **Cluster Management**: Creating and removing logical Samba clusters, defining authentication modes (Standalone/AD), and configuring high-availability (CTDB).
*   **Share Management**: Exporting CephFS volumes or subvolumes, configuring paths, and setting permissions (read-only, browseable).
*   **Authentication & Security**: Handling Active Directory joins, local user/group definitions, and TLS credential management for remote control services.
*   **Network Configuration**: Managing binding addresses, "virtual" public IP addresses for failover, and custom port assignments.
*   **Advanced Configuration**: Using "escape hatch" options for direct `smb.conf` modifications.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph smb apply`, `ceph smb cluster`, `ceph smb share`, `ceph smb show`.
*   **Resources Types**: `ceph.smb.cluster`, `ceph.smb.share`, `ceph.smb.join.auth`, `ceph.smb.usersgroups`, `ceph.smb.tls.credential`.
*   **Components**: `samba-container`, `cephadm`, `CTDB` (clustering), `Samba VFS` (classic, new, proxied).
*   **Configuration**: `auth_mode` (user vs. active-directory), `domain_realm`, `placement` (orchestrator specs), `public_addrs`, `subvolume`.
*   **Protocols**: SMB2, SMB3 (SMB1/CIFS is explicitly unsupported).

### 4. Target Audience
*   **Storage Administrators**: Responsible for exposing CephFS to Windows-centric environments.
*   **DevOps Engineers**: Automating storage provisioning using declarative YAML manifests.
*   **Ceph Developers**: Understanding the integration between the `mgr/smb` module and `cephadm`.

### 5. Related Concepts
*   **CephFS**: The underlying file system being shared.
*   **Cephadm / Orchestrator**: The backend required to deploy the actual Samba containers.
*   **NFS Ganesha**: The parallel manager module for NFS exports (the CLI style is intentionally similar).
*   **Active Directory / Kerberos**: External systems for identity management.

### 6. Maintenance: When to Update this File
This file should be updated if any of the following code changes occur:
*   **CLI Changes**: Adding, renaming, or removing subcommands under `ceph smb`.
*   **Resource Schema Changes**: Modifying the JSON/YAML structure of resources (e.g., adding a new field to `ceph.smb.share`).
*   **Protocol Support**: If support for different versions of SMB or new Samba VFS providers (like a new proxy mode) is added.
*   **Orchestration Logic**: Changes in how `placement` or `clustering` (CTDB) are handled by the manager.
*   **Security Features**: New authentication methods or TLS configuration options for the management gRPC service.