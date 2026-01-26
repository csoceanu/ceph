This documentation provides a comprehensive guide to managing the monitoring infrastructure of a Ceph cluster using **cephadm**. It covers the deployment, configuration, security, and customization of the observability stack.

### 1. Primary Purpose
The file serves as the definitive reference for administrators to set up and manage the Ceph monitoring stack. It explains how `cephadm` orchestrates external open-source tools (Prometheus, Grafana, etc.) to provide metrics and alerting for the Ceph Dashboard.

### 2. Key Topics Covered
*   **Deployment Options**: Automatic deployment during bootstrap vs. manual installation vs. skipping the stack.
*   **Component Architecture**: Roles of Prometheus (metrics), Grafana (visualization), Alertmanager (alert routing), Node Exporter (host metrics), and Loki/Alloy (centralized logging).
*   **Security & Hardening**: Enabling TLS/SSL, basic authentication for components, and managing credentials.
*   **Customization**: Using custom container images, overriding default Jinja2 configuration templates, and setting network/port bindings.
*   **Advanced Configuration**: Setting Prometheus data retention policies, configuring Grafana SSL certificates, and adding Alertmanager webhooks.
*   **Integration**: Using the `cephadm` service discovery endpoint to integrate with external (non-cephadm) monitoring systems.

### 3. Technical Keywords
*   **Orchestrator Commands**: `ceph orch apply`, `ceph orch reconfig`, `ceph orch redeploy`, `ceph orch ls`, `ceph orch rm`.
*   **Services**: `prometheus`, `grafana`, `alertmanager`, `node-exporter`, `loki`, `alloy`.
*   **Configuration Keys**: `mgr/cephadm/secure_monitoring_stack`, `container_image_...`, `mgr/cephadm/service_discovery/root/password`.
*   **APIs & Protocols**: HTTP Service Discovery (`http_sd_config`), Jinja2 templates, TLS/SSL, Basic Auth.
*   **Parameters**: `retention_time`, `retention_size`, `initial_admin_password`, `anonymous_access`.

### 4. Target Audience
*   **Storage Administrators**: Responsible for cluster health and performance monitoring.
*   **DevOps/SREs**: Integrating Ceph monitoring into broader organizational observability platforms.
*   **Security Officers**: Ensuring monitoring data and web interfaces are encrypted and authenticated.

### 5. Related Concepts
*   **Ceph Dashboard**: The primary consumer of the data provided by this stack.
*   **Ceph Manager (mgr)**: The daemon that hosts the Prometheus exporter module and the `cephadm` orchestrator.
*   **External Observability**: Integration with non-Ceph Prometheus/Grafana instances via service discovery.
*   **Log Management**: Integration with Grafana Loki for centralized log aggregation.

---

### Update Trigger Guide for AI Systems
This file should be updated if any of the following code-level changes occur:
*   **New Monitoring Services**: If new daemons (e.g., additional exporters or log agents) are added to the stack.
*   **Orchestrator Logic**: If the `ceph orch` command syntax or behavior changes regarding monitoring components.
*   **Default Image Updates**: If the hardcoded default versions/paths for images in `images.py` are modified.
*   **Security Defaults**: If the default authentication or encryption state (currently insecure by default) is changed.
*   **Configuration Template Changes**: If the underlying Jinja2 templates for `prometheus.yml`, `grafana.ini`, etc., are relocated or their structure is significantly altered.
*   **Discovery API**: If the port (default 8765) or path (`/sd/`) for the Service Discovery endpoint is modified in the `ceph-mgr` code.