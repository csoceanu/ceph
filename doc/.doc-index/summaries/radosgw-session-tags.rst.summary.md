This documentation provides a technical overview of **Session Tags** and **Attribute-Based Access Control (ABAC)** within the Ceph RADOS Gateway (RGW) Secure Token Service (STS). It explains how identity providers (IdPs) can pass metadata via web tokens to control access to S3 resources dynamically.

### 1. Primary Purpose
The file documents the implementation and configuration of **Session Tags** for ABAC. It specifically focuses on how tags passed during federation (via `AssumeRoleWithWebIdentity`) are used to evaluate IAM and trust policies to authorize actions on Ceph Object Storage.

### 2. Key Topics Covered
*   **Federated Identity Integration**: How OpenID Connect (OIDC) tokens from providers like Keycloak pass tags in the `https://aws.amazon.com/tags` namespace.
*   **Trust Policy Configuration**: Requirements for the `sts:TagSession` permission to allow tag ingestion.
*   **Tag Scoping and Logic**: Detailed explanation of different tag "keys" used in policy evaluation.
*   **S3 Resource Tagging**: A comprehensive mapping of S3 operations (e.g., `GetObject`, `PutBucketTags`) to the specific tag type (Bucket vs. Object) used for authorization.
*   **Implementation Constraints**: Limits on tag count, character length, and naming conventions.
*   **End-to-End Example**: A Python (Boto3) implementation showing provider creation, role tagging, and federated access.

### 3. Technical Keywords
*   **APIs/Actions**: `AssumeRoleWithWebIdentity`, `sts:TagSession`, `sts:PrincipalTag`, `s3:PutBucketTagging`, `iam:create_role`.
*   **Policy Condition Keys**:
    *   `aws:RequestTag`: Validates tags sent in the immediate request/token.
    *   `aws:PrincipalTag`: Validates tags attached to the temporary credentials/user session.
    *   `iam:ResourceTag`: Validates tags attached to the IAM Role itself.
    *   `s3:ResourceTag`: Validates tags on the S3 Bucket or Object (notably supported in RGW even where AWS may differ).
    *   `aws:TagKeys`: Used for restricting which tag keys are allowed in a request.
*   **Configuration**: `https://aws.amazon.com/tags` (namespace), `ForAllValues:StringEquals` (set operator).

### 4. Target Audience
*   **Storage Administrators**: Configuring Ceph RGW to work with external Identity Providers.
*   **Security Engineers**: Designing ABAC models to replace or augment RBAC.
*   **Developers**: Implementing application-level integration using Boto3 and OIDC tokens to access Ceph.

### 5. Related Concepts
*   **Ceph RGW STS**: The underlying service providing temporary AWS-compatible credentials.
*   **Keycloak Integration**: Specifically referenced for OIDC token generation.
*   **IAM Policies**: The document relies heavily on AWS-standard IAM policy syntax.
*   **S3 Object/Bucket Tagging**: The mechanism by which resources are labeled to match session tags.

---

### Maintenance Triggers for AI Systems
This file should be updated if any of the following code changes occur:
*   **API Changes**: If new STS actions are added or if `AssumeRoleWithWebIdentity` logic is modified.
*   **Namespace Updates**: If the required tag namespace (`https://aws.amazon.com/tags`) changes or expands.
*   **Limits/Validation Logic**: If the maximum tag count (50), key length (128), or value length (256) constants are modified in the RGW source code.
*   **Feature Parity/Diffs**: If RGW adds or removes support for specific `s3:ResourceTag` operations (referencing the operation/tag type table).
*   **ABAC Logic**: If new condition keys (e.g., `aws:SourceVpce`) are integrated into the tag evaluation engine.