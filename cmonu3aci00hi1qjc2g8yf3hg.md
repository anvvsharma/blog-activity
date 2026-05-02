---
title: "Core Integration Patterns in OIC: Architecture & Implementation Guide"
datePublished: 2026-05-02T04:20:50.085Z
cuid: cmonu3aci00hi1qjc2g8yf3hg
slug: core-integration-patterns-in-oic-architecture-implementation-guide
cover: https://cdn.hashnode.com/uploads/covers/69ce72fa0ff860b6ded94f66/b192b761-3adb-43b2-8267-41f2d474b60a.png

---

## Executive Summary

Oracle Integration Cloud (OIC) provides a standardized set of integration patterns to address common enterprise connectivity challenges. These patterns define the structural approach for data movement, transformation, and orchestration. Selecting the correct pattern is critical for performance, scalability, and maintainability.

This guide details the seven non-negotiable core patterns, their technical implementation in OIC, and the architectural distinctions between Gen2 and Gen3.

* * *

## 1\. App-Driven Orchestration

**Definition:** A synchronous flow initiated by a user action or application event, executing a sequence of business logic steps.

*   **Trigger:** REST Adapter (Incoming) or SOAP Adapter.
    
*   **Logic:** Sequential execution of activities (Invoke, Transform, Assign).
    
*   **Use Case:** Real-time order processing, user registration, credit checks.
    
*   **Gen3 Note:** Flows run in isolated containers; scale independently based on request volume.
    

* * *

## 2\. Scheduled Orchestration

**Definition:** A time-triggered flow executed on a defined cron schedule for batch processing.

*   **Trigger:** Schedule Adapter (Cron expression).
    
*   **Logic:** Bulk data retrieval, transformation, and loading.
    
*   **Use Case:** Nightly financial reconciliation, daily inventory sync, report generation.
    

**Best Practice:** Use Pagination for large datasets to avoid memory overflow. Implement Checkpointing to resume from the last successful record on failure.

* * *

## 3\. File Transfer Pattern

**Definition:** Movement and transformation of files between systems (SFTP, Cloud Storage, Local).

*   **Trigger:** SFTP/FTP Adapter (Polling) or Object Storage Adapter (Event-based).
    
*   **Logic:** File parsing (CSV, XML, JSON), transformation, and archival.
    
*   **Use Case:** Legacy system data migration, EDI file processing.
    
*   **Gen3 Note:** Native integration with OCI Object Storage allows for event-driven triggers (e.g., "On Object Created") rather than polling.
    

* * *

## 4\. Basic Routing (Deprecated in Gen3)

**Definition:** A simple pass-through flow that forwards data from Source to Destination with minimal transformation.

*   **Status:** Deprecated in OIC Gen3.
    

**Gen2 Implementation:**  
REST Adapter -> Mapper (Identity) -> Target Adapter.

**Gen3 Replacement:**

*   API Gateway: For ingress routing with security/throttling.
    
*   Event Streams: For decoupled, high-throughput routing.
    

**Reason:** "Dumb pipes" waste container resources and lack observability.

* * *

## 5\. Publish/Subscribe (Pub/Sub)

**Definition:** An event-driven pattern where a producer publishes an event to a topic, and multiple consumers subscribe to receive it.

*   **Trigger:** Event Stream Adapter (Publisher).
    
*   **Logic:** Decoupled consumption. Consumers process events asynchronously.
    
*   **Use Case:** Notification systems, microservices communication, real-time analytics.
    
*   **Gen3 Note:** Leverages OCI Event Streams (Kafka-compatible) for high throughput and durability.
    

* * *

## 6\. Request-Response

**Definition:** A synchronous pattern where the caller waits for an immediate response from the target system.

*   **Trigger:** REST/SOAP Adapter.
    
*   **Logic:** Immediate invocation and response mapping.
    
*   **Use Case:** Real-time lookups (e.g., "Check Inventory Availability"), validation services.
    

**Constraint:** High latency in the target system blocks the caller. Use timeouts and circuit breakers.

* * *

## 7\. Fire-and-Forget (Async Callback)

**Definition:** An asynchronous pattern where the caller sends a request and does not wait for the response.

*   **Trigger:** REST Adapter (Fire-and-Forget mode).
    
*   **Logic:** Request sent; response handled via Callback or Polling.
    
*   **Use Case:** Long-running processes (e.g., "Initiate Loan Approval"), high-volume data ingestion.
    

**Implementation:** Requires a secondary flow to handle the callback or a polling mechanism to check status.

* * *

## Gen2 vs. Gen3: Pattern Evolution

| Pattern | Gen2 Implementation | Gen3 Implementation | Key Difference |
| --- | --- | --- | --- |
| Orchestration | Monolithic Thread Pool | Container-Native | Gen3 scales per flow; Gen2 scales globally. |
| Scheduling | Cron Polling | Serverless Timer | Gen3 scales to zero when idle. |
| Routing | Basic Pass-Through | API Gateway / Event Stream | Gen3 enforces intelligence and observability. |
| Pub/Sub | Simple Queue | OCI Event Streams | Gen3 offers higher throughput and Kafka compatibility. |
| File Transfer | SFTP Polling | Event-Driven Storage | Gen3 triggers on file arrival, eliminating polling latency. |

* * *

## Implementation Best Practices

*   **Error Handling:** Always implement Fault Policies (Retry, Escalation, Compensation) at the flow level.
    
*   **Security:** Enforce OAuth 2.0 or API Keys at the adapter level. Never expose backend credentials in the flow.
    

### Performance:

*   Use Parallel Execution for independent steps.
    
*   Enable Compression for large payloads.
    
*   Tune Batch Sizes for scheduled flows.
    
*   **Observability:** Utilize OpenTelemetry in Gen3 for distributed tracing. Monitor specific metrics: FlowDuration, ErrorRate, Throughput.
    

* * *

## Key Takeaways

*   Pattern Selection: Match the pattern to the business requirement (Synchronous vs. Asynchronous, Real-time vs. Batch).
    
*   Gen3 Shift: Abandon "Basic Routing"; adopt API Gateway and Event Streams for modern architectures.
    
*   Scalability: Leverage Gen3's container-native architecture for fine-grained autoscaling.
    
*   Resilience: Design for failure. Implement retry logic and dead-letter queues for all critical flows.
    

> 💡 **Pro Tip:**  
> When migrating from Gen2, audit all "Basic Routing" flows. Refactor them into API Gateway policies or Event Stream subscriptions to align with Gen3 best practices and optimize costs.

##### Stay Tune

> Written by [anvvsharma](https://anvvsharma.hashnode.dev)