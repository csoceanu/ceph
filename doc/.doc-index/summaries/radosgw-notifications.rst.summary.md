This documentation file, `radosgw/notifications.rst`, serves as the definitive guide for the **Ceph Object Gateway (RGW) Bucket Notifications** feature. It explains how RGW can trigger and send event information to external endpoints when specific actions occur within a bucket.

### 1. Primary Purpose
The file documents the architecture, configuration, and management of the bucket notification system in Ceph. It provides the technical specifications for integrating Ceph storage events with external message brokers or webhooks, mirroring the functionality of AWS S3 Bucket Notifications but tailored for the Ceph RadosGW environment.

### 2. Key Topics Covered
*   **Notification Mechanisms**: Support for HTTP, AMQP 0.9.1, and Kafka endpoints.
*   **Data Formats**: Transition from legacy metadata to "v2" format (introduced in Squid) for multi-zone synchronization.
*   **Delivery Modes**: 
    *   *Synchronous*: Part of the operation; adds latency; fails if the endpoint is unreachable.
    *   *Asynchronous (Persistent)*: Committed to storage first; retried upon failure; managed via shards.
*   **Topic Management**: Creation, deletion, and listing of "topics" (the destinations for notifications).
*   **Filtering**: Ability to filter events based on object key prefixes/suffixes, metadata attributes, or object tags using regular expressions.
*   **REST API**: Detailed specifications for AWS-compatible SNS-style actions (CreateTopic, GetTopicAttributes, etc.).
*   **Event Schema**: The structure of the JSON payload sent to endpoints, including Ceph-specific extensions (e.g., `opaqueData`, `s3.eventId`).

### 3. Technical Keywords
*   **APIs/Actions**: `CreateTopic`, `GetTopicAttributes`, `DeleteTopic`, `ListTopics`, `SetTopicAttributes`, `sns:Publish`.
*   **CLI Tools**: `radosgw-admin topic list`, `radosgw-admin topic stats`, `radosgw-admin topic dump`.
*   **Configuration Options**: `rgw_enable_apis`, `rgw_bucket_persistent_notif_num_shards`, `rgw_http_notif_max_inflight`, `rgw_topic_persistency_time_to_live`.
*   **Endpoints/Protocols**: `amqp`, `kafka`, `http/https`, `cloudevents`, `SASL` (PLAIN, SCRAM, GSSAPI).
*   **Metadata/Objects**: `ARN` (Amazon Resource Name), `Topic`, `Notification`, `OpaqueData`, `Persistent`.

### 4. Target Audience
*   **Storage Administrators**: Using the CLI to manage topics, monitor performance stats, and troubleshoot persistent queues.
*   **Cloud Architects/Developers**: Using the REST API to integrate Ceph with event-driven applications (e.g., triggering a serverless function when a file is uploaded).
*   **DevOps Engineers**: Configuring endpoint security (SSL/TLS), retry logic, and performance sharding.

### 5. Related Concepts
*   **S3 Compatibility**: Directly relates to the AWS S3/SNS notification ecosystem.
*   **Multi-site/Zonegroups**: Relates to RGW zonegroup features, specifically how notification metadata is synced between clusters.
*   **Persistence/RADOS**: Relates to how Ceph stores internal objects for asynchronous queues in specific RADOS pools.
*   **PubSub**: The underlying architectural pattern for the notification system.

---

### When to update this file (for AI/Maintainers)
This file should be updated if any of the following code changes occur:
1.  **New Endpoints**: If support for a new message broker (e.g., NATS, MQTT) is added.
2.  **API Schema Changes**: If new parameters are added to the `CreateTopic` or `SetTopicAttributes` actions, or if a new event type is supported.
3.  **New Configuration Defaults**: If global variables for retries, timeouts, or sharding are modified or added.
4.  **Feature Flags**: If a new "Zone Feature" is introduced that changes how notification metadata is handled or synced.
5.  **Event Payload Changes**: If the JSON structure of the notification (the `Records` object) is modified to include more metadata from the underlying S3 operation.