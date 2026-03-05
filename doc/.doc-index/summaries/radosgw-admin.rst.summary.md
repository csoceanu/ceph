This documentation file, `radosgw/admin.rst`, serves as the **primary administrative manual** for managing the Ceph Object Gateway (RGW) after initial deployment. It focuses on the CLI-based management of users, permissions, resource constraints, and operational auditing.

### 1. Primary Purpose
The file provides instructions and command-line references for the `radosgw-admin` utility. Its goal is to enable storage administrators to manage the lifecycle of S3 and Swift users, enforce multi-tenancy through quotas and rate limits, and monitor system utilization.

### 2. Key Topics Covered
*   **User & Subuser Management**: Creation and modification of S3 users and Swift subusers, including key generation (access/secret) and account associations.
*   **Access Control & Capabilities**: Assigning administrative "caps" (e.g., `users=read`, `buckets=*`) and global flags like `--admin` or `--system`.
*   **Quota Management**: Setting limits on object counts and storage size at both the user level (aggregated across buckets) and individual bucket level.
*   **Rate Limiting**: Throttling REST operations (Read/Write/List/Delete) and bandwidth (Bytes) per RGW instance to prevent noisy-neighbor scenarios.
*   **Usage Tracking & Auditing**: Enabling and managing usage logs to track user activity, including instructions for trimming logs to save space.

### 3. Technical Keywords
*   **Tool**: `radosgw-admin`
*   **User Entities**: `uid`, `subuser`, `access_key`, `secret_key`, `account`.
*   **Commands**: `user create`, `subuser create`, `caps add`, `quota set`, `ratelimit set`, `usage show`, `usage trim`.
*   **Configuration Options**: `rgw_enable_usage_log`, `rgw_bucket_quota_ttl`, `rgw_ratelimit_interval`, `rgw_usage_log_key_transition`.
*   **API Types**: S3 (User), Swift (Subuser), Admin Ops API.
*   **Quota/Rate Scopes**: `user`, `bucket`, `anonymous`, `global`.

### 4. Target Audience
*   **Storage Administrators**: Responsible for day-to-day Ceph operations.
*   **System Integrators**: Developers building portals that wrap `radosgw-admin` commands.
*   **Support Engineers**: Troubleshooting permission issues or resource exhaustion.

### 5. Related Concepts
*   **Multisite Configuration**: Relates to global quotas and rate limits that require `period update --commit`.
*   **S3 & Swift APIs**: The protocols used by the end-users being managed.
*   **Admin Ops API**: The RESTful alternative to the `radosgw-admin` CLI.
*   **IAM Policies**: Mentioned as a secondary layer of access control that can be overridden by the `--admin` flag.

### When to update this file (for AI/DevOps)
This file should be updated whenever code changes occur in the following areas:
*   **CLI Argument Changes**: If new flags are added to `radosgw-admin` (e.g., new `caps` types or `ratelimit` options).
*   **JSON Schema Changes**: If the output format of `user info` or `usage show` is modified.
*   **Default Behavior**: If default quota logic or rate-limit calculation intervals change in the C++ source.
*   **New Config Options**: When new `rgw_*` parameters related to administration or logging are introduced (e.g., the "Umbrella" release usage log transition).
*   **Feature Deprecation**: If specific subuser or Swift-related features are phased out.