This documentation file provides a technical specification for the **Ceph msgr2 protocol**, the second iteration of Ceph’s on-wire communication layer. It outlines how Ceph components (OSDs, Monitors, Clients, etc.) establish connections, negotiate security, and exchange data.

### 1. Primary Purpose
The file documents the **msgr2 network protocol** (specifically versions 2.0 and 2.1). It defines the frame structures, state machines, and handshake sequences required to replace the legacy "SimpleMessenger" (msgr1) protocol. Its primary goals are to provide flexible protocol negotiation, native wire encryption (AES-GCM), improved performance through better alignment/padding, and more robust error detection.

### 2. Key Topics Covered
*   **Connection Phases**: Detailed lifecycle of a connection including Banner exchange, Authentication, Message Flow Handshake (Identification), and Message Exchange.
*   **Protocol Iterations**: Comparison between **msgr2.0** and **msgr2.1**, highlighting how v2.1 fixed security vulnerabilities (decryption oracles and nonce reuse) and performance regressions (header verification).
*   **Connection Modes**:
    *   `crc`: For integrity without encryption.
    *   `secure`: For full AES-128-GCM on-wire encryption.
*   **Frame Structure**: The anatomy of a frame (preamble, segments, epilogue) and how segments are padded for alignment.
*   **Session Management**: Mechanisms for session persistence, reconnecting after TCP drops, and handling sequence numbers (`global_seq`, `connect_seq`).
*   **Compression**: Negotiation of on-wire compression algorithms and their interaction with secure modes.
*   **Error Handling**: Logic for handling missing features, stale sessions, and connection races.

### 3. Technical Keywords
*   **APIs & Protocols**: `msgr2.0`, `msgr2.1`, `AES-128-GCM`, `CRC32-C`, `CEPHX`.
*   **Tags (Control Frames)**: `TAG_HELLO`, `TAG_AUTH_REQUEST`, `TAG_AUTH_DONE`, `TAG_CLIENT_IDENT`, `TAG_RECONNECT`, `TAG_MSG`, `TAG_KEEPALIVE2`.
*   **Configuration/Flags**: `CEPH_CON_MODE_CRC`, `CEPH_CON_MODE_SECURE`, `FRAME_EARLY_DATA_COMPRESSED`, `CEPH_MSGR2_FEATURE_REVISION_1`.
*   **Data Structures**: `entity_addr_t`, `ceph_msg_header2`, `preamble`, `epilogue`, `late_status`.

### 4. Target Audience
*   **Ceph Core Developers**: Implementing or debugging the `AsyncMessenger` or networking layer.
*   **Security Researchers**: Auditing the cryptographic handshake and frame encryption implementation.
*   **Client Developers**: Creating third-party Ceph clients (e.g., in Python, Go, or Rust) that must communicate natively with Ceph daemons.
*   **Kernel Developers**: Working on the Ceph kernel client (`libceph`).

### 5. Related Concepts
*   **CephX**: The internal authentication system used during the `TAG_AUTH` phase.
*   **Messenger (msgr1)**: The legacy protocol this replaces.
*   **CRUSH & RADOS**: The underlying systems that use this protocol for data replication and cluster heartbeats.
*   **Smart NICs/Offloading**: The protocol's alignment and padding features are designed to facilitate zero-copy and hardware acceleration.

### Implementation Watchlist (When to update this file)
This file should be updated if code changes occur in the following areas:
1.  **Frame Modifications**: Changes to the preamble size, segment count, or CRC/Auth tag placement in `Messenger.cc` or `ProtocolV2.cc`.
2.  **New Features**: Adding new `CEPH_MSGR2_FEATURE` bits or compression algorithms.
3.  **Cryptographic Changes**: Altering the AES-GCM nonce construction, switching cipher suites, or changing how `late_status` is calculated.
4.  **Handshake Logic**: Changing the state machine for how clients and servers identify each other or handle session resets.
5.  **New Tags**: Adding any new `TAG_*` constants for protocol control.