### Documentation Analysis: Rados Gateway S3 API Compliance

#### 1. Primary Purpose
The file `s3_compliance.rst` serves as a **gap analysis and traceability matrix** between the Amazon S3 API specification and the Ceph Rados Gateway (RGW) implementation. It tracks which S3 features, headers, and operations are supported by Ceph and maps them directly to the specific source code files and lines where they are implemented.

#### 2. Key Topics Covered
*   **Feature Naming Convention**: A Backus-Naur Form (BNF) definition for standardized naming of features within the Ceph codebase to ensure consistent referencing.
*   **Common Request/Response Headers**: A status report on supported HTTP headers (e.g., `Authorization`, `ETag`, `x-amz-request-id`).
*   **Service Operations**: Support status for top-level service actions (e.g., GET Service).
*   **Bucket Operations**: A detailed breakdown of bucket-level management features including ACLs, CORS, Policy, Lifecycle, and Versioning.
*   **Object Operations**: Support status for object-level actions including CRUD operations, Multipart Uploads, and Object ACLs.

#### 3. Technical Keywords
*   **APIs**: S3 REST API (BucketOps, ObjectOps, ServiceOps, RESTCommonRequestHeaders).
*   **HTTP Methods**: `GET`, `PUT`, `POST`, `DELETE`, `OPTIONS`, `HEAD`.
*   **RGW Source Components**: 
    *   `rgw_rest_s3.cc` (Main S3 REST logic)
    *   `rgw_op.cc` (Core RGW operations)
    *   `rgw_auth_s3.cc` (Authentication logic)
    *   `rgw_rest_bucket.cc` (Bucket-specific REST handling)
*   **Configuration/Headers**: `x-amz-date`, `Content-MD5`, `Authorization`, `x-amz-version-id`, `ETag`.
*   **Grammar**: BNF (Backus-Naur Form).

#### 4. Target Audience
*   **Ceph Developers**: To identify which S3 features are missing or need debugging.
*   **QA/Automation Engineers**: To cross-reference implemented features with existing or needed integration tests.
*   **System Architects**: To evaluate the compatibility of Ceph RGW with existing S3-native applications.

#### 5. Related Concepts
*   **AWS S3 Documentation**: The primary external reference point for the compliance standards.
*   **S3-tests**: The automated test suite used by the Ceph project to verify compliance.
*   **RGW (Rados Gateway)**: The Ceph component providing the object storage interface.
*   **RESTful Web Services**: The underlying architectural style of the S3 API.

---

### Update Triggers for AI Maintenance
This file is a "living document" that links documentation to code. It should be updated by an AI system or developer when:

1.  **Code Relocation**: If any logic in `src/rgw/` is refactored or moved to different files/line numbers, the "Code Links" URLs (which currently point to specific git blobs) will become broken or inaccurate.
2.  **New Feature Implementation**: When a feature currently marked as "No" or "?" (e.g., `Bucket lifecycle` or `Bucket tagging`) is implemented in a Pull Request, the "Supported?" column must be changed to "Yes."
3.  **API Versioning/Upgrades**: If Amazon S3 introduces new common headers or operations, new rows must be added to the tables.
4.  **Test Development**: As new integration tests are written in the `s3-tests` repository, the "Tests links" column should be populated to provide proof of compliance.
5.  **Refactoring Feature Names**: If the internal naming convention for request/response handling is changed, the BNF definition in the "Naming code reference" section must be updated.