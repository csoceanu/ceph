This documentation file describes the **Ceph Manager (mgr) NFS module**, which provides a centralized interface for managing NFS Ganesha clusters and exporting Ceph File System (CephFS) namespaces and RADOS Gateway (RGW) buckets via the NFS protocol.

### 1. Primary Purpose
The file documents the administration and configuration of NFS services within a Ceph cluster. It explains how to deploy NFS Ganesha gateways, manage high-availability (HA) ingress controllers, and define export rules for Ceph backends (CephFS/RGW) using either the Ceph CLI or the dashboard.

### 2. Key Topics Covered
*   **Cluster Lifecycle**: Creating, listing, updating, and deleting NFS Ganesha clusters.
*   **Orchestration Integration**: Automated deployment via `cephadm` or `rook`.
*   **High Availability (Ingress)**: Implementation of virtual IPs using `keepalived` and load balancing via `haproxy`.
*   **Backend Exports**:
    *   **CephFS**: Mapping CephFS paths to NFS pseudo-paths.
    *   **RGW**: Exporting specific RGW buckets or all buckets belonging to a specific RGW user.
*   **Configuration Management**: Setting custom Ganesha configuration blocks and managing RADOS-stored config objects.
*   **Client Access**: Mounting exports and configuring security types (Kerberos, system auth).
*   **Manual Deployment**: Requirements and limitations for non-orchestrated environments.

### 3. Technical Keywords
*   **Daemons/Services**: `nfs-ganesha`, `nfs-ganesha-ceph`, `haproxy`, `keepalived`, `mgr/nfs`.
*   **CLI Commands**: `ceph nfs cluster create`, `ceph nfs export create cephfs/rgw`, `ceph orch apply -i <spec>.yaml`, `ceph nfs cluster config set`.
*   **Configuration Params**: `pseudo-path`, `cluster_id`, `ingress-mode` (`haproxy-protocol`, `keepalive-only`), `squash` (`no_root_squash`), `sectype` (`krb5p`, `sys`).
*   **Storage Components**: `RADOS`, `FSAL` (File System Abstraction Layer), `.nfs` (pool), `RGW buckets`, `CephFS subvolumes`.

### 4. Target Audience
*   **Storage Administrators**: Seeking to provide standard NFS access to Ceph storage.
*   **Cloud Architects**: Designing highly available storage gateways for Kubernetes (Rook) or bare-metal (Cephadm).
*   **Systems Engineers**: Troubleshooting connectivity or performance issues between NFS clients and Ceph.

### 5. Related Concepts
*   **Ceph Orchestrator (`cephadm`/`rook`)**: Essential for the "cluster create" functionality.
*   **CephFS & RGW**: The underlying storage providers being exported.
*   **NFS-Ganesha**: The third-party user-space NFS server utilized by Ceph.
*   **High Availability**: The "Ingress" feature relates directly to networking and load-balancing patterns.

---

### When to Update This File
This documentation should be updated if any of the following code changes occur:
1.  **CLI/API Changes**: Modifications to the `ceph nfs` command-line arguments or the underlying Manager module API.
2.  **Orchestrator Logic**: Changes in how `cephadm` or `rook` deploy NFS containers or ingress controllers.
3.  **New Export Features**: Adding support for new security types, squashing options, or support for exporting additional Ceph resources.
4.  **Backend Integration**: Updates to the `FSAL` (File System Abstraction Layer) requirements for CephFS or RGW.
5.  **Default Changes**: Changes to default ports, default ingress modes (e.g., switching from `haproxy-standard` to another), or mandatory Manager modules.