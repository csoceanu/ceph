This documentation file, `radosgw/swift/containerops.rst`, serves as the technical reference for **Container Operations** within the Ceph RADOS Gateway (RGW) Swift-compatible API.

### 1. Primary Purpose
The file documents the RESTful API interface for managing **containers** (the Swift equivalent of S3 buckets) in a Ceph cluster. It defines how users can create, list, modify, and delete containers, as well as how to manage metadata, Access Control Lists (ACLs), and object versioning.

### 2. Key Topics Covered
*   **Container Fundamentals**: Definition of a container, naming constraints (no `/` characters, < 256 bytes), and the concept of "pseudo-hierarchical" storage.
*   **CRUD Operations**:
    *   **Create (PUT)**: Idempotent container creation.
    *   **List (GET)**: Retrieving object lists within a container with support for pagination and filtering.
    *   **Delete (DELETE)**: Removing empty containers.
*   **Access Control**: Using headers to define read/write permissions for specific users or making containers public (anonymous access).
*   **Metadata Management**: Adding or updating custom key-value pairs to containers.
*   **Object Versioning**: Configuring an archive container to store historical versions of objects.
*   **S3 Compatibility**: Clarification of terminology (Container vs. Bucket).

### 3. Technical Keywords
*   **HTTP Methods**: `PUT`, `GET`, `POST`, `DELETE`, `HEAD`.
*   **Headers**: 
    *   `X-Auth-Token` (Authentication)
    *   `X-Container-Read` / `X-Container-Write` (ACLs)
    *   `X-Container-Meta-{key}` (Metadata)
    *   `X-Versions-Location` (Versioning)
*   **Query Parameters**: `format` (JSON/XML), `prefix`, `marker`, `limit`, `delimiter`, `path`, `allow_unordered`.
*   **Ceph Configuration Options**: 
    *   `rgw swift account in url`
    *   `rgw swift versioning enabled`
*   **Status Codes**: `202` (Accepted), `204` (No Content), `409` (Conflict/BucketAlreadyExists).

### 4. Target Audience
*   **Cloud Developers**: Building applications that interface with Ceph using the Swift API.
*   **Storage Administrators**: Configuring Ceph RGW to support Swift-based workflows and versioning.
*   **API Integrators**: Migrating workloads from OpenStack Swift to Ceph RGW.

### 5. Related Concepts
*   **OpenStack Swift**: The native API this interface emulates.
*   **Amazon S3**: Referenced for terminology mapping (Buckets).
*   **Keystone**: Mentioned in the context of endpoint definitions and tenant IDs.
*   **RADOS Gateway (RGW)**: The Ceph component providing the object storage gateway.

---

### Maintenance Guide (When to update this file)
This file should be updated if any of the following changes occur in the RGW source code:
1.  **API Schema Changes**: If new headers are supported (e.g., adding `X-History-Location`) or if existing header behaviors change.
2.  **Configuration Defaults**: If the default behavior of `rgw swift versioning enabled` or `rgw swift account in url` changes in `ceph.conf`.
3.  **Feature Extensions**: If new query parameters are added to the listing API (similar to the non-standard `allow_unordered` extension).
4.  **Limits & Constraints**: If the maximum container name length (256 bytes) or the default listing limit (10,000) is modified.
5.  **Error Handling**: If new HTTP status codes are introduced for specific failure modes (e.g., changes to how `409 Conflict` is handled).