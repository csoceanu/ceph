This documentation file, `dev/ceph_krb_auth.rst`, serves as a foundational guide for implementing and configuring Kerberos-based authentication within a Ceph cluster. It bridges the gap between general Kerberos theory and specific Ceph implementation requirements.

### 1. Primary Purpose
The file documents the architectural background and step-by-step setup of **Kerberos (KRBv5) authentication for Ceph**. It specifically focuses on the use of the MIT Kerberos implementation and the GSSAPI (Generic Security Services Application Program Interface) to provide secure, credential-based identity verification between Ceph nodes (monitors, OSDs) and clients.

### 2. Key Topics Covered
*   **Fundamental Concepts**: Definitions of Directory Services (LDAP), Authentication vs. Authorization, Auditing, and SASL mechanisms.
*   **Ceph’s Implementation Logic**: Explains why Ceph uses **GSSAPI** (for portability) rather than native Kerberos APIs and discusses "Quality of Protection" (Integrity and Confidentiality).
*   **Environment Prerequisites**: Requirements for NTP/Chrony (time sync) and DNS (Forward/Reverse/SRV records) for Kerberos to function.
*   **Administrative Workflow**: Creating Principals, generating Keytabs, and distributing them to Ceph nodes.
*   **Ceph Configuration**: How to modify `ceph.conf` to require GSS authentication and point to specific keytab files.
*   **KDC Setup**: A manual guide for setting up a Kerberos Key Distribution Center (KDC) on SUSE or Red Hat systems for development.

### 3. Technical Keywords
*   **APIs**: GSSAPI, SASL, SSPI (Windows), `gss_str_to_oid()`, `gss_release_buffer()`.
*   **Configuration Options**: `auth_cluster_required = gss`, `auth_service_required = gss`, `auth_client_required = gss`, `gss_ktab_client_file`.
*   **Commands**: `kadmin.local`, `ktadd`, `kinit`, `klist`, `kdestroy`, `kdb5_util`.
*   **Files**: `/etc/krb5.conf`, `kdc.conf`, `kadm5.acl`, `/etc/krb5.keytab`.
*   **Security Terms**: TGT (Ticket Granting Ticket), Principal, Realm, OID (Object Identifier), MIC (Message Integrity Code).

### 4. Target Audience
*   **Ceph Developers**: Specifically those working on `auth` modules or networking security.
*   **System Administrators**: Responsible for hardening Ceph clusters in enterprise environments.
*   **Security Engineers**: Integrating Ceph with existing identity providers (Active Directory, FreeIPA).

### 5. Related Concepts
*   **Identity Management**: LDAP (OpenLDAP, Active Directory), SSSD.
*   **Network Protocols**: DNS (SRV/TXT records), NTP, TCP/IP.
*   **Ceph Security**: `cephx` (the default Ceph authentication, which Kerberos complements or replaces in these scenarios).

---

### Triggering Updates (Maintenance for AI Systems)
This file should be updated if any of the following code changes occur:
*   **Auth Method Strings**: If "gss" is renamed or if new specific GSS-related sub-methods are added.
*   **New `ceph.conf` Parameters**: If the logic for locating keytabs changes or if `gss_ktab_client_file` is deprecated in favor of a new parameter.
*   **Dependency Shifts**: If Ceph moves from MIT Kerberos to another implementation (e.g., Heimdal) or changes the version of GSSAPI/SASL required.
*   **Feature Expansion**: If the "Authorization" part of the roadmap (integrating LDAP as a backend for GSSAPI) is implemented.
*   **CLI Changes**: If `ceph` command-line tools introduce new flags for handling Kerberos tickets or debugging GSSAPI contexts.