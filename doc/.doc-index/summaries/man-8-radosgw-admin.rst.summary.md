This documentation file, `man/8/radosgw-admin.rst`, is the definitive manual page for the **Ceph Object Gateway (RGW) administrative CLI**. It details the command-line interface used to manage the lifecycle of users, buckets, quotas, and multi-site configurations within a Ceph cluster.

### 1. Primary Purpose
The file serves as a comprehensive reference for the `radosgw-admin` utility. Its primary goal is to document every administrative command and option available to manage the RADOS REST Gateway (RGW), moving beyond standard S3/Swift API capabilities into low-level cluster administration, repair, and multi-site orchestration.

### 2. Key Topics Covered
*   **User & Identity Management**: Creating, modifying, suspending, and removing users, subusers (Swift), and access keys.
*   **Bucket Operations**: Management of bucket ownership (link/unlink/chown), resharding, index checking, and statistics.
*   **Multi-Site Configuration**: Documentation for Realms, Periods, Zonegroups, and Zones, which define how data replicates across different clusters.
*   **Resource Control**: Setting and enforcing quotas (object count and size) at both the user and bucket levels.
*   **Maintenance & Repair**: Garbage collection (gc), lifecycle processing (lc), orphan object identification, and bucket index (bi) manipulation.
*   **Logging & Usage**: Tools for tracking system usage, trimming logs, and viewing audit trails.
*   **Security & IAM**: Role management and policy attachment for compatibility with STS (Security Token Service).
*   **PubSub/Notifications**: Managing topics and subscriptions for bucket event notifications.

### 3. Technical Keywords
*   **Core Entities**: `uid`, `subuser`, `bucket`, `tenant`, `realm`, `zonegroup`, `zone`, `period`.
*   **Multi-Site Logs**: `mdlog` (metadata log), `bilog` (bucket index log), `datalog`.
*   **Commands**: `user create`, `bucket reshard`, `caps add`, `quota set`, `realm pull`, `metadata sync status`.
*   **Configuration/APIs**: `S3`, `Swift`, `STS`, `REST`, `RADOS`, `JSON/XML`.
*   **Safety Flags**: `--yes-i-really-mean-it`, `--purge-data`, `--fix`.
*   **Storage Internals**: `placement-id`, `index-pool`, `data-pool`, `shard-id`, `manifest`.

### 4. Target Audience
*   **Ceph Administrators**: Those responsible for day-to-day operations, user onboarding, and cluster health.
*   **Cloud Engineers**: Architects designing multi-region/multi-site object storage architectures.
*   **Support Engineers**: Technical staff performing emergency repairs on corrupted bucket indexes or investigating leaked objects.
*   **Automation Developers**: Developers writing scripts or Ansible/Terraform providers to automate RGW resource provisioning.

### 5. Related Concepts
*   **Ceph RADOS**: The underlying distributed object store that `radosgw-admin` interacts with.
*   **S3/Swift APIs**: While `radosgw-admin` is for administration, it manages the entities that use these APIs.
*   **Multi-Site Replication**: The logic governing how data is synchronized across geographically dispersed Ceph clusters.
*   **STS (Security Token Service)**: The AWS-compatible service for temporary credentials and role assumption.

---

### Maintenance Note: When to Update This File
This file should be updated whenever code changes occur in the RGW component of Ceph, specifically:
1.  **CLI Changes**: Adding a new command, renaming a command, or deprecating a utility (e.g., the transition from `orphans find` to `rgw-orphan-list`).
2.  **Feature Additions**: New functionality such as new compression algorithms for placement targets or new sync mechanisms.
3.  **Schema/Metadata Changes**: Changes in how users, buckets, or periods are structured that require new flags (e.g., adding `--tenant` support to more commands).
4.  **Policy/Security Updates**: New IAM-related roles or capability types (caps) added to the RGW engine.
5.  **Default Value Shifts**: Changes to hardcoded defaults, such as the `max-buckets` limit or `max-concurrent-ios`.