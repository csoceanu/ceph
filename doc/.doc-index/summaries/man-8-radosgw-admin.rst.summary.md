This documentation file, `radosgw-admin.rst`, is the manual page for the primary administrative CLI utility used to manage the Ceph Object Gateway (RGW). It serves as the definitive reference for user management, bucket operations, and multi-site configuration.

### 1. Primary Purpose
The file documents the `radosgw-admin` utility, which provides a command-line interface for manual administrative tasks that cannot typically be performed via the S3 or Swift APIs. It is the core tool for managing the backend state of the Ceph Object Gateway.

### 2. Key Topics Covered
*   **User & Subuser Management**: Creation, modification, suspension, and deletion of gateway users and their associated S3/Swift keys.
*   **Bucket Operations**: Managing bucket ownership (`link`/`unlink`/`chown`), statistics, index checking, and sharding/resharding.
*   **Multi-site Administration**: Configuring global clusters using `realms`, `periods`, `zonegroups`, and `zones`.
*   **Resource Control**: Setting and enforcing quotas on users and buckets.
*   **Data Maintenance**: Managing garbage collection (`gc`), lifecycle policies (`lc`), and orphan object searches.
*   **Advanced Features**: Management of STS (Security Token Service) roles, bucket notifications (PubSub), and metadata/data log synchronization.

### 3. Technical Keywords
*   **Core Entities**: `uid`, `subuser`, `tenant`, `bucket`, `bucket-id`, `access-key`, `secret-key`.
*   **Multi-site Terms**: `realm`, `zonegroup`, `zone`, `period`, `epoch`, `master-zone`, `metadata sync`.
*   **Storage Internals**: `RADOS`, `bucket index (bi)`, `placement-id`, `index-pool`, `data-pool`, `reshard`.
*   **Logs**: `mdlog` (metadata log), `bilog` (bucket index log), `datalog`, `usage log`.
*   **Commands**: `user create`, `bucket reshard`, `realm pull`, `quota set`, `caps add`, `orphans find`.
*   **Flags**: `--yes-i-really-mean-it`, `--purge-data`, `--format=json|xml`, `--bypass-gc`.

### 4. Target Audience
*   **Ceph Administrators**: For daily user management and cluster maintenance.
*   **DevOps Engineers**: For scripting automated user provisioning or monitoring.
*   **Support Engineers**: For troubleshooting bucket index inconsistencies or recovering from synchronization failures.

### 5. Related Concepts
*   **Ceph Object Gateway (RGW)**: The service this utility administers.
*   **S3 & Swift APIs**: The protocols used by clients; `radosgw-admin` manages the accounts that use these APIs.
*   **RADOS**: The underlying distributed object store where RGW stores its metadata and data.
*   **Multi-site (Federation)**: The architecture for replicating RGW data across different geographical locations.

---

### Update Triggers for AI Systems
This file must be updated if code changes occur in the following areas:
1.  **CLI Argument Parser**: If new flags (options) are added to `radosgw-admin` or existing ones are deprecated.
2.  **New RGW Features**: If a new subsystem is introduced (e.g., a new notification type or a new storage tiering feature) that requires administrative configuration.
3.  **Command Logic**: If the behavior of a command changes (e.g., `bucket rm` starts requiring a new safety flag, or the output format of `user info` is modified).
4.  **Multi-site Architecture**: If the hierarchy of Realms/Zones changes or new synchronization logs are introduced.
5.  **Quota/Policy Defaults**: If default limits (like the 1000 bucket limit) are changed in the source code.