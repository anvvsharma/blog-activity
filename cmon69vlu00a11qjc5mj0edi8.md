---
title: "OIC Pattern Mastery Series - Foundation: The Architect's Toolkit: 7 Patterns Every Integration Project Needs"
datePublished: 2026-03-28T03:30:00.000Z
cuid: cmon69vlu00a11qjc5mj0edi8
slug: oic-pattern-mastery-series-foundation-the-architect-s-toolkit-7-patterns-every-integration-project-needs
cover: https://cdn.hashnode.com/uploads/covers/69ce72fa0ff860b6ded94f66/7c7a5d8b-a83e-456d-be2e-8f89ccd0f6fd.png

---

Building robust enterprise integrations without a defined architectural pattern is akin to constructing a skyscraper without blueprints; it may stand initially, but it will crumble under scale. In Oracle Integration Cloud (OIC), skipping this step leads to point-to-point spaghetti code, poor observability, and brittle error handling.

**The Solution:** Adopting the 7 non-negotiable integration patterns. These serve as the foundational blueprints for decoupling systems, ensuring scalability, and maintaining data integrity.

---

## TL;DR (2-Minute Quick Reference)

**The 7 Essential Patterns:**
1. **App-Driven Orchestration** - Real-time user-triggered flows
2. **Scheduled Orchestration** - Batch processing on cron schedules
3. **File Transfer** - Legacy system file movement & transformation
4. **Basic Routing** - ⚠️ Deprecated in Gen3 (use API Gateway instead)
5. **Publish/Subscribe** - Event-driven decoupled architecture
6. **Request-Response** - Synchronous real-time lookups
7. **Fire-and-Forget** - Asynchronous high-volume processing

**Key Gen3 Shifts:**
- Container-native runtime (Kubernetes) → Per-flow autoscaling
- Event-driven triggers → Near real-time processing
- Saga pattern → Replace global transactions with compensating logic

**Pattern Selection Rule:** Match to requirement (Sync vs Async, Real-time vs Batch)

[Jump to detailed patterns →](#technical-architecture-the-7-non-negotiable-patterns)

---

## The Story: Rahul's Midnight Crisis

Meet Rahul, a senior integration lead at a mid-sized retailer. It's 2 AM, and his dashboard is flashing red.

His team had built a custom flow to sync inventory between their mobile app and the ERP. It worked fine for the first month. Then, the marketing team launched a flash sale. The mobile app sent 10,000 requests in a minute. The ERP choked. The flow crashed. The site went down.

Rahul realized too late: they hadn't used a pattern. They had just "glued" two systems together with a simple script.

Fast forward six months. Rahul returns with a new strategy. He doesn't just connect systems; he applies **patterns**. He implements a **Scheduled Orchestration** for bulk updates and a **Fire-and-Forget** pattern for high-volume sales.

The next flash sale hits. The system absorbs the load effortlessly. Rahul sleeps soundly.

The difference wasn't better code; it was the right **Architecture Pattern**.

### Technical Architecture: The 7 Non-Negotiable Patterns

The following patterns define the structural approach for data movement, transformation, and orchestration in OIC. Each pattern utilizes specific adapters, triggers, and logic flows.

#### 1\. App-Driven Orchestration

*   **Definition:** Synchronous flow initiated by a user action or application event, executing a sequence of business logic steps.
    
*   **Components:** **REST Adapter** (Trigger), **Invoke Activities**, **Fault Handler**, **Mapper**.
    
*   **Data Flow:**
    
    1.  **Trigger:** Incoming HTTP request (REST/SOAP).
        
    2.  **Process:** Sequential invocation of target systems (e.g., Inventory Check → Reserve Stock → Update CRM).
        
    3.  **Response:** Return success/failure to the caller.
        
*   **Gen2 vs. Gen3:**
    
    | Feature | Gen2 | Gen3 |
    | --- | --- | --- |
    | **Runtime** | Monolithic Thread Pool | **Container-Native** (Kubernetes) |
    | **Scaling** | Global (All flows share threads) | **Per-Flow** (Independent autoscaling) |
    | **Atomicity** | Global Transaction Mode | **Compensating Logic (Saga)** |
    
*   **Use Case:** Real-time order processing where the user waits for confirmation.
    

#### 2\. Scheduled Orchestration

*   **Definition:** Time-triggered flow executed on a defined cron schedule for batch processing.
    
*   **Components:** **Schedule Adapter**, **File Adapter**, **Database Adapter**, **Pagination Logic**.
    
*   **Data Flow:**
    
    1.  **Trigger:** Cron expression fires.
        
    2.  **Process:** Retrieve large dataset in chunks (pagination).
        
    3.  **Transform & Load:** Map data and bulk insert into target.
        
    4.  **Checkpoint:** Save progress to resume on failure.
        
*   **Gen2 vs. Gen3:**
    
    | Feature | Gen2 | Gen3 |
    | --- | --- | --- |
    | **Trigger** | Cron Polling | **Serverless Timer** |
    | **Resource** | Always Active | **Scales to Zero** (Idle) |
    | **Efficiency** | Fixed thread usage | **Dynamic** resource allocation |
    
*   **Use Case:** Nightly financial reconciliation or daily inventory sync.
    

#### 3\. File Transfer Pattern

*   **Definition:** Movement and transformation of files between systems (SFTP, Cloud Storage, Local).
    
*   **Components:** **SFTP/FTP Adapter**, **Object Storage Adapter**, **File Parser**.
    
*   **Data Flow:**
    
    1.  **Trigger:** Polling (SFTP) or Event (Object Storage "On Create").
        
    2.  **Process:** Parse file (CSV/XML), transform data.
        
    3.  **Action:** Move to archive folder or load into DB.
        
*   **Gen2 vs. Gen3:**
    
    | Feature | Gen2 | Gen3 |
    | --- | --- | --- |
    | **Trigger** | Polling (Interval) | **Event-Driven** (Webhook) |
    | **Storage** | Generic SFTP | **OCI Object Storage** Native |
    | **Latency** | High (Polling delay) | **Near Real-Time** |
    
*   **Use Case:** EDI file processing or legacy system data migration.
    

#### 4\. Basic Routing (Deprecated in Gen3)

*   **Definition:** A simple pass-through flow forwarding data from Source to Destination with minimal transformation.
    
*   **Components:** **REST Adapter**, **Identity Mapper**, **Target Adapter**.
    
*   **Data Flow:**
    
    1.  **Trigger:** Receive request.
        
    2.  **Process:** Pass-through (no logic).
        
    3.  **Action:** Forward to target.
        
*   **Gen2 vs. Gen3:**
    
    | Feature | Gen2 | Gen3 |
    | --- | --- | --- |
    | **Status** | Supported | **Deprecated** |
    | **Replacement** | N/A | **API Gateway** or **Event Stream** |
    | **Reason** | N/A | Lacks observability & security |
    
*   **Use Case:** *Legacy only.* In Gen3, use API Gateway for ingress or Event Streams for decoupling.
    

#### 5\. Publish/Subscribe (Pub/Sub)

*   **Definition:** Event-driven pattern where a producer publishes an event to a topic, and multiple consumers subscribe to receive it.
    
*   **Components:** **Event Stream Adapter**, **Topic**, **Subscription Filters**, **DLQ**.
    
*   **Data Flow:**
    
    1.  **Publish:** Producer sends event to Topic.
        
    2.  **Route:** Event Stream distributes to subscribers.
        
    3.  **Consume:** Independent flows process the event asynchronously.
        
*   **Gen2 vs. Gen3:**
    
    | Feature | Gen2 | Gen3 |
    | --- | --- | --- |
    | **Backend** | Simple Queue | **OCI Event Streams** (Kafka) |
    | **Throughput** | Limited | **High Throughput** |
    | **Durability** | Basic | **High Durability** |
    
*   **Use Case:** Notification systems or microservices communication.
    

#### 6\. Request-Response

*   **Definition:** Synchronous pattern where the caller waits for an immediate response from the target system.
    
*   **Components:** **REST/SOAP Adapter**, **Timeout Settings**, **Circuit Breaker**.
    
*   **Data Flow:**
    
    1.  **Request:** Caller sends data.
        
    2.  **Wait:** Flow blocks until response received.
        
    3.  **Response:** Return data to caller.
        
*   **Gen2 vs. Gen3:**
    
    | Feature | Gen2 | Gen3 |
    | --- | --- | --- |
    | **Blocking** | Yes | Yes |
    | **Resilience** | Basic Retry | **Circuit Breaker** Patterns |
    | **Timeout** | Global/Flow | **Activity-Level** Config |
    
*   **Use Case:** Real-time credit check or inventory availability lookup.
    

#### 7\. Fire-and-Forget (Async Callback)

*   **Definition:** Asynchronous pattern where the caller sends a request and does not wait for the response.
    
*   **Components:** **REST Adapter**, **Callback Handler**, **Polling Logic**.
    
*   **Data Flow:**
    
    1.  **Send:** Caller fires request and disconnects.
        
    2.  **Process:** Target processes asynchronously.
        
    3.  **Notify:** Target calls back or flow polls for status.
        
*   **Gen2 vs. Gen3:**
    
    | Feature | Gen2 | Gen3 |
    | --- | --- | --- |
    | **Mechanism** | Callback/Polling | **Event-Driven** |
    | **Scalability** | Thread-bound | **Serverless** |
    | **Complexity** | Manual | **Managed** by Event Streams |
    
*   **Use Case:** Long-running loan approvals or background report generation.
    

### Implementation Best Practices

*   **Error Handling:** Always implement **Fault Policies** (Retry, Escalation, Compensation) at the flow level. Configure exponential backoff for transient errors.
    
*   **Security:** Enforce **OAuth 2.0** or **API Keys** at the adapter level. Never expose backend credentials in the flow payload.
    
*   **Performance:**
    
    *   Use **Parallel Execution** for independent steps to reduce total flow duration.
        
    *   Enable **Compression** for large payloads to reduce network overhead.
        
    *   Tune **Batch Sizes** (e.g., 500-1000 records) for scheduled flows to balance throughput and memory.
        
*   **Observability:** Utilize **OpenTelemetry** in Gen3 for distributed tracing. Monitor specific metrics: `FlowDuration`, `ErrorRate`, `Throughput`.
    

### Real-World Use Case: Retail Order Management

**Scenario:** A retail platform must handle order placement, inventory checks, and CRM updates.

*   **Technical Constraints:** High volume during flash sales (10k req/min), need for real-time inventory validation, and asynchronous CRM updates.
    
*   **Pattern Application:**
    
    1.  **App-Driven Orchestration:** Used for the initial order submission (REST Trigger).
        
    2.  **Request-Response:** Used for real-time inventory validation (ERP Adapter).
        
    3.  **Fire-and-Forget:** Used for CRM updates to prevent blocking the user experience.
        
    4.  **Scheduled Orchestration:** Used for nightly reconciliation of failed orders.
        
*   **Result:** The system absorbs peak loads without crashing, ensures data consistency via **compensating logic** for failed steps, and decouples non-critical updates.
    

### Key Takeaways

*   **Pattern Selection:** Match the pattern to the business requirement (Synchronous vs. Asynchronous, Real-time vs. Batch).
    
*   **Gen3 Shift:** Abandon "Basic Routing"; adopt **API Gateway** and **Event Streams** for modern architectures.
    
*   **Scalability:** Leverage Gen3's container-native architecture for fine-grained autoscaling of specific flows.
    
*   **Resilience:** Design for failure. Implement **Compensating Logic** (Saga) and **Idempotency** for all critical flows, as global transactions are not supported.
    

💡 **Pro Tip:** When migrating from Gen2, audit all "Basic Routing" flows. Refactor them into **API Gateway** policies or **Event Stream** subscriptions to align with Gen3 best practices and optimize costs. **Crucially:** Replace any reliance on Gen2's "Transaction Mode" with explicit **Fault Handlers** that execute compensating actions.

##### Stay Tune

> Written by [anvvsharma](https://anvvsharma.hashnode.dev)