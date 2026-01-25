# HARDWARE-MONITORING Documentation Index

## Overview
This documentation covers the `node-proxy` agent, a specialized hardware monitoring component within the Ceph ecosystem. Its primary purpose is to provide operators with deep visibility into physical server health (storage, CPU, power, etc.) by bridging the gap between Out-Of-Band (OOB) RedFish APIs and the Ceph Manager (`ceph-mgr`) dashboard and CLI.

## Files Summary
*   **hardware-monitoring/index.rst**: The primary guide for the hardware monitoring subsystem. It details the architectural flow, deployment steps via `cephadm`, CLI usage for health reporting, and internal Python API endpoints for developers.

## Code Changes That Would Require Documentation Updates
*   **CLI Command Schema**: Any changes to the `ceph orch hardware status` command, including new flags, modified arguments, or changes to the supported `--category` types.
*   **Data Collection Logic**: Modifications to how `node-proxy` interacts with the RedFish API or changes in the networking requirements (e.g., moving away from OOB network dependency).
*   **Hardware Category Expansion**: Adding support for new hardware types (e.g., GPU monitoring, SmartNICs, or specialized NVMe sensors).
*   **Configuration Parameters**: Changes to the `mgr/cephadm/hw_monitoring` config key or the introduction of new configuration tunables for the agent.
*   **Orchestrator Spec Schema**: Updates to the `host` service specification, specifically the `oob` (Out-of-Band) credential block structure (addr, username, password).
*   **API Endpoint Signatures**: Modifications to the `cephadm.agent.NodeProxyEndpoint` class or its methods (`memory`, `storage`, `led`, etc.), which would invalidate the developer reference section.
*   **UI/Dashboard Integration**: Significant changes in how data is passed from the manager to the dashboard that would alter the architectural diagram.

## Key Technical Concepts
*   **node-proxy**: The internal name for the hardware inventory and monitoring agent.
*   **RedFish API**: The industry-standard RESTful API used for platform management and hardware monitoring.
*   **OOB (Out-Of-Band)**: The dedicated management network/interface used to communicate with the hardware (e.g., iDRAC, iLO).
*   **Service Spec (host.yml)**: The YAML definition used by `cephadm` to define host properties and OOB credentials.
*   **Categories**: Specific hardware subsystems including `storage`, `memory`, `processors`, `network`, `power`, `fans`, `firmwares`, and `criticals`.
*   **NodeProxyEndpoint**: The developer-facing Python class in `cephadm.agent` responsible for handling hardware data requests.
*   **LED Control**: Functionality exposed via the API (e.g., `NodeProxyEndpoint.led`) to interact with physical identification lights on hardware.

## Related Components
*   **Ceph Manager (ceph-mgr)**: The daemon that receives and aggregates data from the `node-proxy`.
*   **Cephadm**: The orchestrator used to deploy the agent and apply host configurations.
*   **Ceph Dashboard**: The web interface that visualizes the hardware statuses queried from the manager.
*   **Orchestrator (orch)**: The CLI module used to trigger hardware status reports.
*   **Hardware Vendors**: Physical server components (IBM, Dell, HP, etc.) that provide RedFish compliant management controllers.