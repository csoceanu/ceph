This documentation file, `dev/config.rst`, provides a comprehensive guide to the **Ceph Configuration Management System**. It serves as the primary technical reference for how Ceph daemons acquire, store, and update configuration settings.

### 1. Primary Purpose
The file documents the architecture and implementation of Ceph’s configuration system. It explains how settings are prioritized, how developers should define new options in the source code, and the programmatic interfaces for reading or modifying those settings at runtime.

### 2. Key Topics Covered
*   **Configuration Sources**: Hierarchical lookup via `ceph.conf`, command-line arguments, environment variables, and runtime injections.
*   **Stanza Logic**: How settings are scoped (e.g., `[global]`, `[osd]`, or specific IDs like `[osd.0]`).
*   **Metavariables**: Support for variable expansion using symbols like `$host`, `$type`, and `$id`.
*   **Programmatic Access**: Methods for code to interact with configuration (e.g., `g_conf`, observers).
*   **Defining Options**: The transition to YAML-based option definitions (`.yaml.in` files) and the `y2c.py` build-time translation.
*   **Documentation Standards**: How to use the `:confval:` directive in reStructuredText to document and reference settings.

### 3. Technical Keywords
*   **Core APIs/Objects**: `g_conf`, `g_ceph_ctx->_conf`, `common/config_obs.h` (Observer pattern), `set_val`, `apply_changes`.
*   **CLI/Runtime Commands**: `injectargs`, `config set`, `parse_argv`, `parse_env`.
*   **Configuration Files**: `ceph.conf`, `CEPH_CONF` (env var), `src/sample.ceph.conf`.
*   **YAML Definition Keys**: `level` (basic/advanced/dev), `default`, `services`, `tags`, `flags` (runtime/startup/cluster_create), `enum_values`.
*   **Tools**: `y2c.py` (YAML to C++ translator).

### 4. Target Audience
*   **Ceph Developers**: Specifically those adding new features that require configurable parameters or modifying existing daemon behavior.
*   **System Architects**: Understanding the lifecycle and propagation of settings across a distributed cluster.
*   **Documentation Maintainers**: Learning the syntax for referencing configuration values in the official docs.

### 5. Related Concepts
*   **Daemon Management**: Relates to how OSDs, Monitors (MONs), and MDS daemons initialize.
*   **Ceph Manager (MGR) Modules**: While MGR modules use this system, they have a unique Python-based definition process mentioned as a related reference.
*   **Build System**: Integration with the build process where YAML files are compiled into C++ headers.

---

### Triggering Updates: When to Edit This File
An AI or developer should update `dev/config.rst` (or follow its instructions to update YAML files) if:
1.  **New Parameters are Added**: If a developer adds a new feature that needs a toggle or value, they must define it in `common/options/*.yaml.in`.
2.  **Changing Option Behavior**: If an option that was previously "startup-only" becomes "runtime-changeable," the `flags` in the YAML must be updated.
3.  **Deprecating Access Methods**: If the programmatic internal APIs (like `g_conf`) are further restricted or replaced by new patterns.
4.  **Documentation Standards Change**: If the naming conventions or reStructuredText directives for documenting options are modified.
5.  **New Metavariables**: If the configuration engine is updated to support new expansion variables beyond `$host` or `$id`.