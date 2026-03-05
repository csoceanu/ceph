This documentation file, `radosgw/config-ref.rst`, serves as the definitive configuration reference for the **Ceph Object Gateway (RGW)**. It provides a structured inventory of the parameters used to tune, secure, and operate the S3 and Swift-compatible interface of a Ceph storage cluster.

### 1. Primary Purpose
The file documents the available configuration options for the RGW service. It explains how to apply these settings within the `ceph.conf` file (typically under `[client.radosgw.{instance-name}]`) and provides guidance on performance tuning for specific subsystems like Garbage Collection and Lifecycle Management.

### 2. Key Topics Covered
*   **Core Gateway Runtime**: General settings for frontends, timeouts, thread pools, and object striping.
*   **Bucket Lifecycle (LC)**: Configuration for parallel processing of object expiration and transition rules (introduced/improved in the Nautilus release).
*   **Garbage Collection (GC)**: Tuning parameters for purging deleted/overwritten data from the underlying RADOS cluster, specifically for delete-heavy workloads.
*   **Multisite Operations**: Parameters for realms, zones, and sync logs (metadata and data sync) used in geo-replication.
*   **Authentication & Interoperability**: Integration with S3 (LDAP), Swift (ACLs, multi-tenancy), and OpenStack Keystone.
*   **Security & Encryption**: Support for Server-Side Encryption (SSE) using Barbican, HashiCorp Vault, and SSE-S3 backends.
*   **QoS & Scheduling**: Managing request concurrency and experimental mClock-based rate limiting (Beast vs. Civetweb frontends).
*   **Caching & Notifications**: D4N (Redis-backed caching), Cloud Restore (async object restoration), and Topic Persistency for bucket notifications.

### 3. Technical Keywords
*   **Frontends**: `beast`, `civetweb`, `rgw_frontends`.
*   **APIs/Protocols**: S3, Swift, LDAP, Keystone, Barbican, Redis.
*   **Components/Daemons**: `radosgw`, `radosgw-admin`.
*   **Logic/Flows**: `rgw_lc_max_worker`, `rgw_gc_max_concurrent_io`, `rgw_dmclock_data_res`.
*   **Storage Internals**: `rgw_obj_stripe_size`, `rgw_max_chunk_size`, `instance-name`.
*   **Configuration Macro**: `.. confval::` (indicates a unique configuration variable).

### 4. Target Audience
*   **Storage Administrators**: To tune performance and manage cluster resources.
*   **Cloud Architects**: To design multisite or encrypted object storage solutions.
*   **DevOps/SREs**: To troubleshoot timeouts, logging, and connectivity between RGW and external auth providers (Keystone/Vault).

### 5. Related Concepts
*   **RADOS**: The underlying reliable autonomic distributed object store that RGW interacts with.
*   **Multisite/Federation**: The architectural pattern for replicating data across different geographical zones.
*   **D4N (Data Delivery Network for Nations)**: An advanced caching layer for RGW.
*   **Bucket Notifications**: The event-driven system for triggering external actions on object changes.

---

### AI Update Triggers (When to update this file)
An AI system should flag this file for updates whenever code changes occur in the following areas:
1.  **New Configuration Definitions**: If a new `rgw_*` variable is added to the RGW source code (usually in `common/options/rgw.yaml.in` or similar).
2.  **Default Value Changes**: If the default behavior of a lifecycle, GC, or sync thread is modified.
3.  **Feature Deprecation**: If older frontends (like `civetweb`) or specific auth methods are removed.
4.  **New Subsystems**: If a new feature involving background threads (similar to Cloud Restore or D4N) is implemented.
5.  **Versioning**: When a new Ceph named release (e.g., Quincy, Reef) introduces changes to the recommended tuning values for S3/Swift compatibility.