This documentation file, `radosgw/multisite-sync-policy.rst`, provides a comprehensive guide to the **Bucket-Granularity Multisite Sync Policy** introduced in the Ceph **Octopus** release. It describes a sophisticated framework for controlling data replication between specific buckets across different Ceph Object Gateway (RGW) zones.

### 1. Primary Purpose
The file documents the transition from coarse, zonegroup-wide replication to a **fine-grained sync policy engine**. It explains how administrators can define specific data flows and "pipes" to control exactly which data moves between which buckets and zones, allowing for non-symmetrical and complex replication topologies.

### 2. Key Topics Covered
*   **Sync Policy Hierarchy**: How policies are defined at the Zonegroup level (broad permissions) and inherited or refined at the Bucket level (specific implementation).
*   **Policy States**: The three operational statuses for sync groups: `enabled`, `allowed`, and `forbidden`.
*   **Data Flow Definitions**: Concepts of **Symmetrical** (multi-way) and **Directional** (one-way) data movement.
*   **Pipe Configurations**: The mechanism for defining the actual source/destination bucket pairs, including support for wildcards (`*`).
*   **S3 Replication API**: Integration with standard S3 bucket replication, including RGW-specific extensions like the `Zone` array.
*   **Administrative Reference**: A complete CLI command guide for managing groups, flows, and pipes.
*   **Practical Examples**: Ten detailed scenarios ranging from simple mirroring to complex "pull/push" bucket relationships, storage class transitions, and metadata filtering.

### 3. Technical Keywords
*   **Core Concepts**: `zonegroup`, `zone`, `sync policy`, `data-flow`, `pipe`, `wildcard`, `period update`.
*   **Status Flags**: `enabled`, `allowed`, `forbidden`.
*   **CLI Tool**: `radosgw-admin sync` (subcommands: `group`, `flow`, `pipe`, `info`, `get`, `create`, `modify`).
*   **Configuration Parameters**: `--group-id`, `--flow-id`, `--pipe-id`, `--flow-type`, `--source-zones`, `--dest-zones`, `--source-bucket`, `--dest-bucket`.
*   **Filtering & Transformation**: `--prefix`, `--tags-add`, `--storage-class`, `--dest-owner`, `--mode=user`.

### 4. Target Audience
*   **Storage Administrators**: Managing multi-region or multi-site Ceph clusters.
*   **Cloud Architects**: Designing Disaster Recovery (DR) or data distribution strategies.
*   **Developers**: Implementing S3-compatible replication features in applications.

### 5. Related Concepts
*   **Ceph Multisite (RGW)**: The underlying framework for zone and zonegroup replication.
*   **S3 Bucket Replication**: The AWS-standard API that this policy engine supports and extends.
*   **Disaster Recovery**: The primary use case for mirroring entire zones or specific critical buckets.
*   **Data Tiering**: Using the `storage-class` parameter to move synced data to cheaper storage in remote zones.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **RGW Multisite Logic**: Any change to how `radosgw-admin` handles sync commands or how the sync engine interprets `allowed` vs `forbidden` states.
2.  **CLI Command Signature**: If new flags are added to `radosgw-admin sync group/flow/pipe` (e.g., new filtering criteria beyond prefix/tags).
3.  **S3 API Compatibility**: If the RGW implementation of the S3 Replication API adds new capabilities (like intra-zone replication, which the doc currently notes as unsupported).
4.  **Sync Metadata**: If the structure of the JSON output for `sync info` or `sync policy get` is modified.
5.  **Replication Modes**: Changes to `user` mode permissions or the introduction of new security modes.