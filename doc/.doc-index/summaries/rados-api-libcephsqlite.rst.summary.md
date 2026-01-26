This documentation provides a technical overview of **libcephsqlite**, a SQLite Virtual File System (VFS) that allows SQLite databases to be stored and managed directly on Ceph’s RADOS object store.

### 1. Primary Purpose
The file documents how to use the `libcephsqlite.so` extension to back SQLite databases with RADOS. It bridges the gap between the structured relational capabilities of SQLite and the high availability/scalability of Ceph. It is specifically designed for applications needing to store relational data across multiple objects (striping) while maintaining consistency through RADOS locks.

### 2. Key Topics Covered
*   **Operational Model**: Explains that this is *not* a distributed SQL engine; it is intended for single-client access at a time, using RADOS locks for serial access.
*   **Usage & Connection**: Detailed URI syntax for opening databases (`file:///<pool>:[namespace]/<dbname>?vfs=ceph`).
*   **Performance Tuning**: Guidance on `PRAGMA` settings (page size, cache size, journal modes) optimized for high-latency RADOS backends.
*   **Locking & Data Integrity**: Detailed explanation of RADOS exclusive locks, the necessity of client blocklisting to prevent corruption, and lock timeout/renewal configurations.
*   **Database Management**: Methods for extracting striped databases from RADOS and why standard SQLite backup APIs are preferred over manual extraction.
*   **Observability**: Integrated SQLite functions (`ceph_status()`, `ceph_perf()`) for monitoring VFS-specific performance counters and client addresses.

### 3. Technical Keywords
*   **APIs**: SQLite VFS API, SQLite Extension Loading API, RADOS Striper.
*   **Configuration Options**: `cephsqlite_lock_renewal_timeout`, `cephsqlite_lock_renewal_interval`, `cephsqlite_blocklist_dead_locker`.
*   **SQL Pragmas**: `page_size`, `cache_size`, `journal_mode = PERSIST`, `locking_mode = EXCLUSIVE`, `temp_store = memory`.
*   **Commands**: `.load libcephsqlite.so`, `osd blocklist`, `rados --striper get`.
*   **Environment Variables**: `CEPH_CONF`, `CEPH_KEYRING`, `CEPH_ARGS`.

### 4. Target Audience
*   **Ceph Developers**: Specifically those working on `ceph-mgr` or internal components that require relational storage.
*   **Database Engineers**: Looking to decentralize SQLite databases for improved availability without using a full distributed SQL cluster.
*   **System Administrators**: Responsible for managing Ceph auth permissions and troubleshooting database locks or blocklisted clients.

### 5. Related Concepts
*   **RADOS Striper**: The underlying mechanism used to spread the database file across multiple OSD objects.
*   **Ceph Blocklisting**: A critical security/integrity feature used to fence off dead or "zombie" clients.
*   **RBD (RADOS Block Device)**: Used as a conceptual analogy (exclusive access via RADOS) to explain the locking model.
*   **OMAP**: Mentioned as a scaling alternative; libcephsqlite is recommended when OMAP (single object) scale limits are reached.

---

### Maintenance Guide (When to update this file)
This documentation should be updated if any of the following code changes occur:
*   **URI Parsing**: Changes to how the SQLite URI is structured (e.g., changes to pool/namespace delimiters).
*   **Locking Logic**: If the default RADOS lock names (`striper.lock`) or the timeout/renewal logic is modified in the C++ source.
*   **New Performance Counters**: Adding or renaming performance counters that would appear in the output of `ceph_perf()`.
*   **Dependency Changes**: Changes in how `libcephsqlite.so` is loaded or integrated with standard Ceph authentication profiles (like `simple-rados-client-with-blocklist`).
*   **Feature Support**: If concurrent read access or readahead capabilities are implemented.