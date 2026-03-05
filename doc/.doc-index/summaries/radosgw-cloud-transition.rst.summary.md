This documentation file describes the **Cloud Transition** feature of the Ceph Object Gateway (RGW). It provides a mechanism to move S3 objects from a local Ceph cluster to a remote, S3-compatible cloud provider based on lifecycle policies.

### 1. Primary Purpose
The file documents how to configure and manage the **unidirectional** transition of objects from a Ceph RGW cluster to remote cloud endpoints (like AWS S3 or S3-compliant services). It defines how "Cloud Storage Classes" function as a bridge between local lifecycle management and external cloud storage.

### 2. Key Topics Covered
*   **Storage Class Types**: Introduction of `cloud-s3` (standard) and `cloud-s3-glacier` (archival) tier types.
*   **Tier Configuration**: Parameters for connecting to remote endpoints, including credentials, regions, and path styles.
*   **ACL Mapping**: Methods to map local RGW user permissions to specific destination users in the cloud.
*   **Lifecycle Integration**: How to use standard S3 Lifecycle (LC) XML rules to trigger cloud transitions.
*   **Object Semantics**: Handling of versioned objects, retention of metadata ("head objects"), and naming conventions in the destination bucket.
*   **Management Commands**: CLI instructions using `radosgw-admin` to create, modify, and remove cloud placement targets.

### 3. Technical Keywords
*   **APIs & Protocols**: S3 Lifecycle Management, AWS S3, Glacier.
*   **RGW Components**: `zonegroup`, `placement-id`, `storage-class`, `tier-type`.
*   **Configuration Keys**:
    *   Connection: `access_key`, `secret`, `endpoint`, `region`, `host_style`.
    *   Logic: `target_path`, `target_storage_class`, `retain_head_object`.
    *   Performance: `multipart_sync_threshold`, `multipart_min_part_size`.
*   **CLI Commands**: `radosgw-admin zonegroup placement add/modify/rm`, `period update --commit`.
*   **Metadata Headers**: `x-amz-meta-rgwx-*`, `x-rgw-cloud`, `x-rgw-cloud-keep-attrs`.
*   **Error Codes**: `InvalidObjectState` (returned when attempting to GET a transitioned object).

### 4. Target Audience
*   **Ceph Administrators**: Responsible for configuring multisite, placement targets, and storage tiers.
*   **Storage Architects**: Designing hybrid-cloud workflows or cost-optimization strategies (tiering to cheaper cloud storage).
*   **Developers**: Integrating applications that need to know where data resides and why certain objects return 403 errors.

### 5. Related Concepts
*   **RGW Multisite**: The feature relies on the zonegroup/placement infrastructure used in multisite setups.
*   **S3 Lifecycle Management**: The engine that triggers the movement of data.
*   **Storage Classes**: The abstraction layer used to categorize data placement.
*   **Cloud Restore**: The logical next step (referenced as future work/related link) for bringing data back to the local cluster.

---

### Maintenance Note: When to update this file
This documentation should be updated if any of the following code changes occur:
1.  **New Cloud Providers**: If support is added for non-S3 backends (e.g., Azure, Google Cloud Storage).
2.  **CLI Argument Changes**: If the `radosgw-admin` syntax for `tier-config` or `placement` is modified.
3.  **Metadata Changes**: If the internal RGW metadata headers (`x-amz-meta-rgwx-*`) are renamed or new ones are added.
4.  **Behavioral Logic**: If the handling of versioned objects, delete markers, or the `retain_head_object` logic is altered.
5.  **New Configuration Keys**: If new tuning parameters for multipart uploads or connection timeouts are introduced.