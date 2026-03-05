This documentation file, `radosgw/multisite.rst`, serves as the definitive guide for configuring and managing **Ceph Object Gateway (RGW) Multi-Site** environments. It covers the architectural hierarchy, deployment procedures, and operational workflows for replicating object data across multiple Ceph clusters.

### 1. Primary Purpose
The file documents the **Multi-Site feature of the RADOS Gateway**, which allows for geographically distributed storage, disaster recovery, and global object namespaces. It explains how to move from a single-zone setup to complex topologies involving multiple zones, zonegroups, and realms.

### 2. Key Topics Covered
*   **Architecture Hierarchy**: Defines **Realms** (containers), **Zonegroups** (formerly regions/namespaces), and **Zones** (individual Ceph clusters).
*   **Replication Modes**: Distinguishes between **Active-Active** (all zones writeable) and **Active-Passive** (read-only secondary zones).
*   **Configuration Workflows**: Step-by-step guides for creating a Master Zone, adding Secondary Zones, and migrating from single-site to multi-site.
*   **Synchronization Mechanics**: Details on metadata and data synchronization, including the "period" system for tracking configuration epochs.
*   **Operational Procedures**: Failover/failback processes, zone promotion to master, and log maintenance (mdlog, bilog, datalog).
*   **JSON Schema**: Definitions for zonegroup and zone configuration objects.

### 3. Technical Keywords
*   **Entities**: `realm`, `zonegroup`, `zone`, `period`, `epoch`.
*   **CLI Tools**: `radosgw-admin`, `ceph orch apply rgw`, `systemctl`.
*   **Commands**: `realm create/pull`, `zonegroup modify/set`, `period update --commit`, `sync status`.
*   **Configuration Options**: `rgw_zone`, `rgw_zonegroup`, `rgw_sync_obj_etag_verify`, `rgw_sync_log_trim_interval`.
*   **Internal Logs**: `mdlog` (metadata), `bilog` (bucket index), `datalog` (data).
*   **Authentication**: System users, `access_key`, `secret_key`.

### 4. Target Audience
*   **Storage Administrators**: Responsible for deploying and maintaining Ceph clusters.
*   **System Architects**: Designing disaster recovery (DR) or Content Delivery Network (CDN) solutions.
*   **DevOps/SREs**: Automating RGW deployments and monitoring synchronization health.

### 5. Related Concepts
*   **S3 API**: Specifically `LocationConstraint` for bucket placement.
*   **RADOS Pools**: The underlying storage pools (e.g., `.rgw.control`, `.rgw.buckets.data`).
*   **Cephadm/Orchestrator**: For containerized deployment of RGW daemons.
*   **Civetweb/Frontends**: The web servers hosting the RGW interface.

---

### Update Triggers for AI Maintenance
An AI system should flag this file for updates if code changes occur in the following areas:
1.  **RGW CLI Modifications**: Any changes to `radosgw-admin` subcommands (e.g., adding new flags to `zone create` or `period update`).
2.  **Sync Engine Changes**: Modifications to the underlying synchronization logic, such as new sync policies or changes to how shards are handled.
3.  **Data Models**: Updates to the JSON structures of Realms, Zonegroups, or Zones in the RGW source code (often found in `rgw_zone.h` or similar).
4.  **Default Behaviors**: Changes to default pool naming conventions, default port assignments, or the transition from active-passive to active-active defaults.
5.  **New Multi-site Features**: Introduction of features like "Cloud Sync" or granular bucket-level replication policies that expand upon the core multi-site framework.