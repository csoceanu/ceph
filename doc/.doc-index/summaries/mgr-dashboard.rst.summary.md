This documentation file describes the **Ceph Dashboard**, a built-in web-based management and monitoring interface for Ceph clusters. It serves as both a feature manual and a configuration guide for administrators.

### 1. Primary Purpose
The file provides a comprehensive guide for enabling, configuring, and using the Ceph Dashboard. It explains the architecture (a `ceph-mgr` module using CherryPy and Angular), details the administrative capabilities across all Ceph services (RBD, RGW, CephFS, etc.), and provides troubleshooting steps.

### 2. Key Topics Covered
*   **Core Infrastructure**: Installation, enabling the module, and configuring SSL/TLS certificates.
*   **Access Control**: Multi-user management, RBAC (Role-Based Access Control) with predefined "system roles," and password policies.
*   **External Integrations**:
    *   **Monitoring**: Integration with Prometheus, Grafana (embedding dashboards), and Alertmanager (silences and notifications).
    *   **Authentication**: Single Sign-On (SSO) via SAML 2.0 and OAuth2.
*   **Service Management**: Front-end management for OSDs, Pools, RBD (mirroring/snapshots), CephFS, iSCSI, and Object Gateway (RGW).
*   **Network Configuration**: Handling proxy setups, URL prefixes, and active/standby manager redirection.
*   **Auditing & Logging**: Logging API requests (PUT/POST/DELETE) and centralized logging via Loki/Promtail.
*   **Troubleshooting**: Commands to locate the dashboard URL, debug login issues, and increase log verbosity.

### 3. Technical Keywords
*   **APIs/Frameworks**: CherryPy (backend), Angular/TypeScript (frontend), REST API, SAML 2.0, OAuth2, LogQL.
*   **Commands**: 
    *   `ceph mgr module enable dashboard`
    *   `ceph dashboard create-self-signed-cert`
    *   `ceph dashboard ac-user-create`
    *   `ceph dashboard set-grafana-api-url`
*   **Configuration Keys**: `mgr/dashboard/server_addr`, `mgr/dashboard/ssl`, `url_prefix`, `standby_behaviour`.
*   **Components**: `ceph-mgr`, Prometheus, Grafana, Alertmanager, Node Exporter, `rbd-target-api`, NFS Ganesha.

### 4. Target Audience
*   **Storage Administrators**: Seeking to manage Ceph via a GUI rather than the CLI.
*   **System Engineers**: Responsible for setting up monitoring stacks and security (SSL/SSO).
*   **Developers**: Contributing to the dashboard or integrating it with third-party tools.

### 5. Related Concepts
*   **Ceph Manager (ceph-mgr)**: The dashboard runs as a module within this daemon.
*   **Cephadm**: The orchestrator often used to deploy the monitoring stack (Prometheus/Grafana) that the dashboard consumes.
*   **Monitoring Stack**: Closely tied to the `mgr/prometheus` module.
*   **External Gateways**: Integrates with `ceph-iscsi` and RGW for storage provisioning.

### When to Update this File
This documentation should be updated if any of the following code changes occur:
*   **CLI Changes**: Any modification to the `ceph dashboard ...` command sub-tree.
*   **New Features**: Adding management support for new Ceph services or adding new cards/metrics to the Landing Page.
*   **Security/Auth**: Changes to supported SSO protocols, password hashing methods, or SSL requirements.
*   **Dependencies**: Changes in supported browser versions or required external libraries (e.g., `python-saml`).
*   **Configuration Schema**: Changes to the `mgr/dashboard/` configuration options or default port settings.
*   **Integration Logic**: Modifications in how the dashboard interacts with Prometheus, Grafana, or the Ceph Orchestrator.