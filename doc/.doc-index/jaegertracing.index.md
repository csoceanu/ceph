# JAEGERTRACING Documentation Index

## Overview
This documentation covers the integration of Jaeger Distributed Tracing within the Ceph ecosystem. It provides a foundational understanding of tracing terminology, deployment strategies (including manual and `cephadm` methods), and specific instructions for enabling and interpreting traces within Ceph services like RGW and OSD.

## Files Summary
*   **jaegertracing/index.rst**: The primary guide for Jaeger tracing in Ceph. It defines core architecture, provides deployment commands, explains global/per-entity configuration, and details how RGW requests and multipart uploads are represented in the Jaeger UI.

## Code Changes That Would Require Documentation Updates
*   **Default Port Changes**: Any modification to the default Ceph tracing port (currently `6799`) or the default Jaeger Query port (`16686`).
*   **Tracing Configuration Keys**: Changes to the configuration names (e.g., renaming `jaeger_tracing_enable`) or the introduction of new tracing-related config flags.
*   **New Instrumented Entities**: If tracing support is added to new Ceph components beyond RGW and OSD (e.g., MDS, Manager, or RBD-mirror).
*   **RGW Tagging Logic**: Adding, removing, or renaming tags attached to RGW spans (e.g., `User id`, `Object name`, `Bucket name`, `Upload id`).
*   **Span/Trace Naming Conventions**: Changes to the string formatting for trace names, such as the `<command> <transaction id>` or `multipart_upload <upload id>` patterns.
*   **Deployment Orchestration**: Updates to `cephadm` that change how the Jaeger service is deployed or managed as a containerized stack.
*   **Dependency Updates**: Shifts from OpenTracing to OpenTelemetry APIs within the Ceph source code, or changes to the required `jaegertracing/all-in-one` image version.

## Key Technical Concepts
*   **Architecture Components**: Trace, Span, Jaeger Client, Jaeger Agent (Sidecar), Jaeger Collector, Jaeger Query/Console.
*   **Configuration Commands**: `ceph config set global jaeger_tracing_enable`, `ceph config set <entity> jaeger_tracing_enable`.
*   **Network/Ports**: `6799` (Ceph default compact port), `6831` (Official default), `16686` (Frontend UI), `9411` (Zipkin), `4317/4318` (OTLP).
*   **RGW Tracing Attributes**: `Operation name`, `User id`, `Object name`, `Bucket name`, `Upload id`, `transaction id`.
*   **Deployment Modes**: `cephadm` services, All-in-one Docker container, Manual deployment.
*   **Data Flow**: Instrumented App -> Local Agent (UDP) -> Collector -> Database/UI.

## Related Components
*   **RGW (Rados Gateway)**: Specifically mentioned regarding request tracing and multipart upload spans.
*   **OSD (Object Storage Daemon)**: Identified as a service visible within the Jaeger Frontend.
*   **Cephadm**: The primary orchestrator used for deploying Jaeger as a managed service.
*   **OpenTracing / OpenTelemetry**: The underlying APIs and protocols used for instrumentation.
*   **Docker**: Used for manual/test deployments of the Jaeger stack.