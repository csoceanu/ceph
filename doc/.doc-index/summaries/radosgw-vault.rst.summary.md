This documentation file, `radosgw/vault.rst`, is the definitive technical guide for integrating **HashiCorp Vault** as a Key Management Service (KMS) for **Ceph Rados Gateway (RGW)**. It specifically addresses **Server-Side Encryption (SSE-KMS)** workflows.

### 1. Primary Purpose
The file documents how to configure Ceph RGW to communicate with HashiCorp Vault to store, retrieve, or utilize encryption keys for S3-compliant object encryption. It ensures that master keys are managed in a dedicated secure environment (Vault) rather than within the Ceph cluster itself.

### 2. Key Topics Covered
*   **Architecture Flow**: A visual sequence diagram showing the interaction between the Client, RGW, Vault, and OSDs during object creation and encryption.
*   **Secrets Engines**: Detailed setup for two primary Vault engines:
    *   **KV (Key-Value) v2**: Storing static 256-bit keys.
    *   **Transit**: Using Vault as a cryptography-as-a-service provider (performing encryption/decryption within Vault or rotating keys).
*   **Authentication Mechanisms**:
    *   **Token-based**: Direct use of a plaintext token file.
    *   **Vault Agent**: Using a local proxy/daemon to handle complex auth (AppRole, AWS, etc.) and automatic token renewal.
*   **Policy Management**: Specific HCL (HashiCorp Configuration Language) policy examples required for RGW to access Vault paths.
*   **Security & Namespaces**: Guidance on using Vault Enterprise namespaces and securing the RGW-to-Vault connection via SSL/TLS.
*   **Compatibility**: Legacy support for older Transit engine implementations.

### 3. Technical Keywords
*   **APIs/Engines**: `kv-v2`, `transit`, `AppRole`.
*   **RGW Configuration Options**:
    *   `rgw crypt s3 kms backend = vault`
    *   `rgw crypt vault secret engine` (kv vs. transit)
    *   `rgw crypt vault auth` (token vs. agent)
    *   `rgw crypt vault addr`, `rgw crypt vault prefix`, `rgw crypt vault namespace`.
    *   `rgw crypt vault ssl cacert/clientcert/clientkey`.
*   **Commands**: `vault secrets enable`, `vault policy write`, `aws s3 cp --sse=aws:kms`.
*   **Crypto**: `aes256-gcm96`, `base64`, `256-bit keys`.

### 4. Target Audience
*   **Ceph Administrators**: Setting up secure storage clusters.
*   **Security Engineers**: Implementing KMS policies and managing secrets lifecycles.
*   **Cloud Architects**: Designing S3-compatible storage solutions with strict compliance requirements.

### 5. Related Concepts
*   **Server-Side Encryption (SSE)**: Specifically SSE-KMS and SSE-S3.
*   **S3 API**: The user-facing interface for object uploads.
*   **HashiCorp Vault Ecosystem**: Concepts like Tokens, Policies, and Agents.
*   **RADOS OSDs**: The backend storage where encrypted data ultimately resides.

---

### When to update this file (for AI/DevOps maintenance)
This file should be updated if any of the following code changes occur in the Ceph RGW component:
1.  **New Configuration Keys**: If a new `rgw_crypt_vault_*` option is added to the Ceph configuration schema.
2.  **New Secrets Engines**: If support is added for other Vault engines (e.g., PKI or specialized cloud engines).
3.  **Authentication Changes**: If RGW adds native support for Vault auth methods like AWS IAM or Kubernetes without requiring the Vault Agent.
4.  **API Path Changes**: If the internal logic for constructing Vault URLs (e.g., adding `/v1/` prefixes or modifying data paths) is changed.
5.  **Deprecations**: If "transit compatibility" modes are removed or if KV v1 support is officially dropped.
6.  **Dependency Updates**: If the required version of HashiCorp Vault changes or if new TLS/SSL verification parameters are introduced.