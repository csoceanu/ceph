This documentation file, `radosgw/adminops.rst`, serves as the technical reference for the **Ceph RADOS Gateway (RGW) Admin Operations API**. It details the RESTful interface used to perform administrative tasks that go beyond the standard S3/Swift data APIs.

### 1. Primary Purpose
The file documents the **Admin API**, providing developers and administrators with the specific syntax, request parameters, and response formats required to manage RGW infrastructure programmatically. It acts as the "source of truth" for the RESTful endpoints used to manage users, buckets, usage logging, and quotas.

### 2. Key Topics Covered
*   **Infrastructure Info**: Retrieving RGW cluster and endpoint information.
*   **Usage Management**: Enabling, retrieving, and trimming bandwidth and operation usage logs.
*   **Account Management (New in Squid)**: Creating, modifying, and deleting multi-tenant accounts and their associated metadata.
*   **User & Subuser Management**: Full CRUD operations for users and subusers, including suspension and placement targets.
*   **Key Management**: Managing S3 and Swift access/secret keys.
*   **Bucket & Object Administration**: retrieving bucket metadata, checking/fixing bucket indexes, unlinking/linking buckets to users, and managing access policies.
*   **User Capabilities (Caps)**: Granting or revoking administrative permissions (e.g., `users=read`, `buckets=write`).
*   **Quotas & Rate Limiting**: Setting storage limits (size/object count) and throughput throttling (ops/bytes) at user, bucket, and account levels.

### 3. Technical Keywords
*   **APIs**: `GET /{admin}/info`, `GET /admin/usage`, `PUT /admin/user`, `POST /admin/account`, `POST /admin/ratelimit`.
*   **Configuration Options**: `rgw enable usage log`, `rgw admin entry point`.
*   **Capabilities (Caps)**: `usage`, `users`, `buckets`, `accounts`, `ratelimit` (with `read` or `write` levels).
*   **Entities**: `uid`, `subuser`, `access-key`, `secret-key`, `tenant`, `account-id`.
*   **Quotas/Limits**: `max-objects`, `max-size-kb`, `max-read-ops`, `max-write-bytes`, `accumulation interval`.
*   **Formats**: XML, JSON (default).

### 4. Target Audience
*   **Cloud Engineers/DevOps**: Automating user onboarding and resource provisioning.
*   **Billing Systems Developers**: Using the usage endpoints to calculate consumption for invoicing.
*   **Security Administrators**: Managing access keys and administrative capabilities.
*   **Storage Administrators**: Troubleshooting bucket index issues or enforcing resource quotas.

### 5. Related Concepts
*   **S3/Swift API**: The Admin API uses the same authorization mechanism as S3.
*   **Multi-tenancy**: Uses `tenant$user` syntax and account-level management.
*   **Librados**: The API interacts with the underlying Ceph RADOS layer (e.g., `cluster_fsid`).
*   **Garbage Collection (GC)**: Relevant in bucket/object removal operations (e.g., `bypass-gc`).

---

### Update Triggers for Maintenance
This file must be updated if code changes occur in the following areas:
*   **RGW Versioning**: Whenever new features are backported or added to releases (e.g., the "Squid" version added Account Management).
*   **API Syntax**: If the resource entry point logic or URL structures change.
*   **Command Line Interface (CLI)**: While this documents REST, changes to the underlying `radosgw-admin` logic often reflect new parameters available in the Admin API.
*   **Schema Changes**: If new response fields (JSON/XML) are added to user or bucket metadata.
*   **New Capabilities**: If a new administrative permission type is introduced in the RGW authorization module.