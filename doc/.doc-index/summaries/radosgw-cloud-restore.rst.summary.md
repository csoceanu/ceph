This documentation file, `radosgw/cloud-restore.rst`, provides a technical overview and configuration guide for the **Cloud Restore** feature in Ceph’s RADOS Gateway (RGW). 

It specifically addresses the workflow and mechanics of pulling objects back into a local RGW deployment after they have been moved to external S3-compatible cloud providers via the "Cloud Transition" (storage tiering) feature.

### 1. Primary Purpose
The file documents how to retrieve and restore data that has been tiered to a remote cloud endpoint. It explains how to configure RGW to support these retrievals and how users can initiate restoration using standard S3 APIs.

### 2. Key Topics Covered
*   **Restoration Methods**: Detailed explanation of the `S3 RestoreObject` API (for explicit restores) and the "Read-through" mechanism via `S3 GetObject`.
*   **Tier Configuration**: Parameters required in the `tier-config` to facilitate restores, such as credentials, endpoints, and storage class mapping.
*   **Glacier-Specific Logic**: Procedures for objects archived in high-latency cloud tiers (Glacier/Tape), including retrieval tiers like `Expedited` or `Standard`.
*   **Object Lifecycle & Properties**: How restored objects behave regarding persistence (temporary vs. permanent), metadata (`mtime`), versioning, and replication across Ceph zones.
*   **Verification**: How to use `S3 HeadObject` or `radosgw-admin` to check the status of a restoration process.

### 3. Technical Keywords
*   **APIs**: `RestoreObject`, `GetObject`, `HeadObject`.
*   **Configuration Options**: `retain_head_object`, `allow_read_through`, `restore_storage_class`, `read_through_restore_days`, `glacier_restore_days`, `glacier_restore_tier_type`.
*   **Commands**: `radosgw-admin zonegroup placement modify`, `aws s3api restore-object`, `radosgw-admin object stat`.
*   **Status Headers**: `x-amz-restore`, `x-amz-storage-class`.
*   **Storage Classes**: `STANDARD`, `CLOUDTIER`, `cloud-s3-glacier`.

### 4. Target Audience
*   **Ceph Storage Administrators**: Who need to configure zonegroup placements and storage tiers to allow users to access archived data.
*   **Cloud Architects**: Designing hybrid storage solutions involving Ceph and public/private S3 clouds.
*   **Developers/Users**: Utilizing the S3 API to manage object lifecycles and data retrieval from cold storage.

### 5. Related Concepts
*   **Cloud Transition (Tiering)**: The prerequisite process of moving data to the cloud.
*   **RGW Multisite/Replication**: The document notes that temporary restores are local to a zone, while permanent restores participate in standard replication.
*   **Lifecycle (LC) Policies**: Rules governing when restored objects expire or are deleted.
*   **S3 Glacier Retrieval Tiers**: Integration with remote cloud provider retrieval speeds and costs.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
*   **RGW Tiering Logic**: Any changes to how `tier-config` JSON structures are parsed or validated.
*   **S3 API Implementation**: If new parameters are added to `RestoreObject` or if the behavior of `GetObject` (read-through) is modified.
*   **Glacier Integration**: If additional retrieval tiers are supported or if the `cloud-s3-glacier` tier-type logic changes.
*   **Placement Strategy**: Changes to `radosgw-admin` commands related to zonegroup placement and storage class management.
*   **Metadata Handling**: If the way `x-amz-restore` headers or object `mtime` are handled during the download-from-cloud phase is altered.