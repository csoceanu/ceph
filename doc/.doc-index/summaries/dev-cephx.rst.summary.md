### **Documentation Analysis: Cephx Authentication Protocol (`dev/cephx.rst`)**

#### **1. Primary Purpose**
This file serves as the technical specification for **Cephx**, the proprietary authentication protocol used by the Ceph distributed storage system. It describes the cryptographic handshake and message exchange patterns used to verify the identity of clients and daemons and to distribute authorization capabilities.

#### **2. Key Topics Covered**
*   **Architecture & Roles**: Defines the "KDC-like" role of Monitors and the relationships between Clients, Services (OSDs, MDSs, MGRs), and Principals.
*   **Ticket System**: Explains the distinction between *Auth Tickets* (proving identity to Monitors) and *Service Tickets* (proving identity/authorization to storage daemons).
*   **Three-Phase Authentication Flow**:
    *   **Phase I**: Initial handshake to obtain a global ID and authentication session key.
    *   **Phase II**: Requesting specific service tickets (e.g., for OSD access).
    *   **Phase III**: Establishing a secure connection to a specific service daemon with replay protection.
*   **Security Mechanisms**: Details on shared secrets, rotating service keys, session key negotiation, and challenge-response nonces.
*   **Version Evolution**: Explicitly documents differences in message structures between "Pre-Luminous/Mimic," "Luminous/Mimic," and "Nautilus+" versions.

#### **3. Technical Keywords**
*   **Entities**: `monitor`, `principal`, `client`, `service`, `global_id`.
*   **Cryptographic Keys**: `principal_secret`, `mon_secret`, `service_secret` (rotating), `session_key`, `connection_secret`.
*   **Message Types/Headers**: `CephxServerChallenge`, `CephXAuthenticate`, `CephxAuthorizer`, `CephxTicketBlob`, `CephxServiceTicketInfo`.
*   **Opcodes/Constants**: `CEPHX_GET_AUTH_SESSION_KEY`, `CEPHX_GET_PRINCIPAL_SESSION_KEY`, `CEPHX_GET_ROTATING_KEY`, `AUTH_MODE_AUTHORIZER`.
*   **Protocols**: `Messenger protocol`, `msgr2`, `Kerberos` (as a design reference).

#### **4. Target Audience**
*   **Core Ceph Developers**: Maintaining or extending the authentication subsystem.
*   **Security Auditors**: Reviewing the cryptographic implementation of the Ceph cluster.
*   **Protocol Engineers**: Implementing third-party clients or debugging low-level network captures (e.g., via Wireshark).

#### **5. Related Concepts**
*   **Messenger Protocol (msgr1/msgr2)**: The transport layer that carries these authentication payloads.
*   **RADOS Capabilities (Caps)**: The authorization strings embedded within Cephx tickets that define what a user is allowed to do (e.g., `allow rwx pool=rbd`).
*   **Kerberos**: The conceptual precursor to the Cephx design.
*   **Monmap/OSDMap**: Internal cluster maps that distribute the identity of valid daemons.

---

### **Maintenance Guide: When to Update This File**
An AI or developer should trigger an update to `dev/cephx.rst` if any of the following code changes occur:

1.  **Message Schema Changes**: If any `struct` or `class` related to `CephxAuthenticate`, `CephxAuthorizer`, or `CephxTicketBlob` in the C++ source code (likely in `src/auth/cephx/`) is modified with new fields or different data types.
2.  **Protocol Versioning**: If a new `encoding_version` is introduced or a new release name (e.g., "Quincy+") requires a specific branch in the logic (similar to how "Nautilus+" is currently documented).
3.  **Cryptographic Transitions**: If the encryption method changes (e.g., moving from AES-128 to a different cipher) or if the way the `key` is derived from challenges changes (e.g., switching from XOR `^` to a different KDF).
4.  **New Ticket Types**: If a new service type is added to the Ceph ecosystem that requires a unique `CEPH_ENTITY_TYPE`.
5.  **Handshake Logic**: If the state machine for Phase I, II, or III is altered—specifically regarding how "Challenges" and "Nonces" are exchanged to prevent replay attacks.
6.  **Secret Management**: Any change to how `rotating_service_secrets` are generated, distributed, or invalidated.