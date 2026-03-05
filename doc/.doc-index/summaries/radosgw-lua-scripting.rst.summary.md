This documentation file describes the **Lua Scripting** feature in the Ceph RADOS Gateway (RGW), introduced in the "Pacific" release. It details how users can inject custom logic into the S3/Swift request pipeline and data processing stream.

### 1. Primary Purpose
The file provides a functional and technical overview of how to write, manage, and execute Lua scripts within the Rados Gateway. It serves as the authoritative guide for extending RGW functionality without recompiling the C++ source code, specifically for tasks like custom logging, request modification, and data analysis.

### 2. Key Topics Covered
*   **Execution Contexts**: Defines five specific stages where scripts can run:
    *   `prerequest` / `postrequest`: Logic before/after an operation.
    *   `background`: Periodic tasks (e.g., monitoring/analytics).
    *   `getdata` / `putdata`: Intercepting object data during download or upload.
*   **Resource Management**: Memory limits (default 500KB) and runtime timeouts (default 1000ms).
*   **Package Management**: How to use `luarocks` to add third-party Lua modules (e.g., `luasocket`, `cjson`) and how to allowlist them.
*   **Script Management**: Commands to upload, retrieve, and delete scripts via the CLI.
*   **The Object Model**: Detailed tables describing the `Request`, `Bucket`, `Object`, and `User` fields available to the Lua environment.
*   **Global Persistence**: The `RGW` table, which allows sharing data across requests and contexts on a per-instance basis.

### 3. Technical Keywords
*   **APIs & Functions**: `RGWDebugLog()`, `Request.Log()`, `Request.Trace.SetAttribute()`, `Request.Trace.AddEvent()`, `RGW.increment()`, `RGW.decrement()`.
*   **CLI Commands**: `radosgw-admin script put`, `radosgw-admin script-package add`, `radosgw-admin script-package reload`.
*   **Configuration Parameters**: `rgw_lua_max_memory_per_state`, `rgw_lua_max_runtime_per_state`, `rgw_luarocks_location`.
*   **Fields**: `Request.RGWOp`, `Request.HTTP.Metadata`, `Request.Response.HTTPStatusCode`, `Data`, `Offset`.

### 4. Target Audience
*   **Cloud Architects/Administrators**: Who need to implement custom authentication, throttling, or logging policies.
*   **Storage Developers**: Building extensions or integrations (e.g., sending metadata to external search engines like Elasticsearch).
*   **DevOps Engineers**: Monitoring RGW performance or auditing specific user activities in real-time.

### 5. Related Concepts
*   **S3 & Swift Protocols**: The scripting logic directly manipulates headers and responses for these protocols.
*   **Ceph Tracing (Jaeger)**: The scripting engine integrates with RGW's tracing capabilities to add custom spans and events.
*   **RADOS Gateway Ops Log**: Scripts can manually trigger entries into the standard operations log.
*   **Luarocks**: The external package manager used to extend the script's capabilities.

---

### Maintenance Triggers for AI Systems
This file should be updated if any of the following code changes occur:

1.  **Field Additions/Removals**: If new fields are added to the RGW request structure (C++ `req_state`) and exposed to Lua, or if existing fields are renamed/deprecated.
2.  **Context Changes**: If a new execution context is added (e.g., a "pre-auth" context) or if the behavior of existing contexts (like `background` timing) changes.
3.  **CLI Updates**: If `radosgw-admin` adds or modifies commands related to `script` or `script-package`.
4.  **Resource Constraints**: If the default memory or runtime limits are changed in the underlying C++ implementation.
5.  **Global Table Behavior**: If the `RGW` global table limits (currently 100k entries) or persistence logic is modified.
6.  **Dependency Changes**: If the minimum supported Lua version or the way `luarocks` is integrated changes.