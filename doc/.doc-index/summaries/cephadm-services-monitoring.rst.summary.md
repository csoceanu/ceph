This documentation provides a technical overview of how **cephadm** manages the monitoring stack for a Ceph cluster. It details the deployment, security configuration, and customization of various observability tools integrated into the Ceph ecosystem.

### 1. Primary Purpose
The file serves as the authoritative guide for administrators to deploy, configure, and maintain the Ceph monitoring infrastructure using `cephadm`. It explains how to integrate **Prometheus, Grafana, Alertmanager, and Node Exporter** to provide metrics and visualization for the Ceph Dashboard.

### 2. Key Topics Covered
*   **Deployment Options**: Automated deployment via `cephadm` (default), manual deployment for existing infrastructures, or skipping the stack entirely.
*   **Service Components**: Roles of Prometheus (metrics), Grafana (visualization), Alertmanager (alerts), and Node Exporter (host metrics).
*   **Security & Hardening**: Enabling TLS/SSL, basic authentication for endpoints, and managing credentials for service discovery.
*   **Centralized Logging**: Integration of **Loki** (log aggregation) and **Alloy** (log forwarding).
*   **Customization**: Using custom container images, overriding Jinja2 configuration templates, and defining service specifications (YAML).
*   **Service-Specific Tuning**: Prometheus retention policies (time/size), Grafana SSL certificates, and Alertmanager webhooks.
*   **External Integration**: How to expose a service discovery endpoint for external Prometheus instances.

### 3. Technical Keywords
*   **Orchestrator Commands**: `ceph orch apply`, `ceph orch reconfig`, `ceph orch redeploy`, `ceph orch rm`.
*   **Configuration Keys**: `mgr/cephadm/secure_monitoring_stack`, `container_image_...`, `mgr/cephadm/services/...`.
*   **Services/Daemons**: `prometheus`, `grafana`, `alertmanager`, `node-exporter`, `loki`, `alloy`.
*   **Networking/APIs**: `http_sd_config` (Prometheus Service Discovery), Port `8765` (SD endpoint), Port `9283` (MGR metrics).
*   **Templates**: Jinja2 (`.j2`), `config-key`.

### 4. Target Audience
*   **Ceph Cluster Administrators**: Responsible for setting up health monitoring and performance dashboards.
*   **DevOps/SRE Engineers**: Integrating Ceph metrics into existing enterprise monitoring tools.
*   **Security Officers**: Ensuring monitoring data and dashboards are protected via TLS and authentication.

### 5. Related Concepts
*   **Ceph Dashboard**: The primary consumer of the data visualized by Grafana and stored in Prometheus.
*   **Ceph Manager (mgr)**: The component that hosts the Prometheus module and the service discovery endpoint.
*   **RBD Monitoring**: Specific logic required to enable I/O statistics for block devices (disabled by default for performance).
*   **Containerization**: Since `cephadm` is container-based, this relates to image registries and lifecycle management of containers.

---

### Update Triggers for AI Systems
This file should be updated if any of the following code-level changes occur:
1.  **Default Image Changes**: If the default versions or paths for Prometheus, Grafana, or Loki images change in `src/python-common/ceph/cephadm/images.py`.
2.  **CLI Arguments**: If `ceph orch` commands for monitoring services add/remove flags or change syntax.
3.  **Template Logic**: If the Jinja2 templates in `src/pybind/mgr/cephadm/templates` are modified or if new template paths are added.
4.  **New Monitoring Services**: If new daemons (e.g., additional exporters) are added to the monitoring stack.
5.  **Config Key Changes**: If the internal keys used to store credentials or security settings (e.g., `mgr/cephadm/service_discovery/...`) are renamed or restructured.
6.  **Port Changes**: If the default ports for service discovery (8765) or exporters are altered in the manager module.