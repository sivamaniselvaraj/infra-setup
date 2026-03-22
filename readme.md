Design Decisions and Trade-offs

1. Messaging Choice
    Decision: Use Apache Kafka for communicating between services.
    Why this choice: services are loosely coupled

        Kafka provides:
            1. high throughput messaging
            2. durable event storage
            3. horizontal scalability through partitions

        Trade-offs
            Pros:   
                1. scalable event processing
                2. service decoupling
                3. resilience to failures
            Cons
                1. operational complexity
                2. message ordering challenges
                3. eventual consistency

    Alternative options:
    RabbitMQ (simpler queues)
    REST synchronous calls (simpler but tightly coupled)

    Takeaway: Kafka is better for high-scale event-driven systems.

2. Database Choice
    Decision: Use database-per-service model.
    Why this choice
        1. service decoupled
        2. independent schema evolution
        3. easier scaling
    Trade-offs
        Pros
            1. avoids shared DB bottleneck
            2. independent scaling
            3. service isolation
        Cons
            4. no distributed transactions
            5. requires Saga pattern
            6. data duplication between services

    Takeaway: This design embraces eventual consistency.

3. Communication Pattern
    Decision: Use asynchronous event-driven communication.
    Why this choice
        services are decoupled
        systems tolerate failures
        easier horizontal scaling
    Trade-offs
        Pros
            better resilience
            improved scalability
            reduced service dependencies
        Cons
            harder debugging
            eventual consistency
            complex error handling
    To maintain consistency, the system uses:
    Saga pattern
    Outbox pattern
    Inbox pattern

    Takeaway: Async communication improves resilience and scale.

4. Caching Strategy
    Decision: Use distributed caching with Redis.
    Why this choice: Caching reduces database load.
    Trade-offs
        Pros
            faster read performance
            reduced DB load
            scalable read layer
        Cons
            cache invalidation complexity
            possible stale data

    Takeaway: caching improves latency and scalability, but must be carefully managed.


Non-Functional Requirements (NFRs)

1. Performance
    Goal
        Low latency and high throughput for order processing and notifications.

    Design Choices
    --------------
    1. Caching Layer
        Using Redis to cache frequently accessed data.

        Benefits:
            reduces database load
            faster response times

    2. Asynchronous Processing
        Instead of blocking calls between the services use event streaming using messaging queues (Apache Kafka)
        This reduces response latency for the client.

2. Scalability
    Goal
        Handle increasing traffic and user growth.

    Design Choices
    --------------
        Horizontal Service Scaling
            Load balancers distribute requests.

        Event Streaming Scalability
            Kafka scales using partitions. Multiple consumers process events in parallel.

        Database Per Service
            Each service manages its own database. This avoids a shared database bottleneck.

3. Resiliency
    Goal
        Ensure the system continues functioning during failures.
    Design Choices
        Saga Pattern
            Handles distributed transactions.
        Circuit Breaker
            Use resilience libraries such as Resilience4j. Prevents cascading failures.
        Message Durability
            Using Apache Kafka ensures events are not lost. Events remain in Kafka, Consumer resumes later
        Inbox pattern or CDC (CDC tool - Change Data Capture eg: Debezium: An open-source, log-based CDC platform built on Kafka Connect, supporting MySQL, PostgreSQL, MongoDB, Oracle, and SQL Server. ) CDC is powerful but not free.
        Outbox pattern
        
4. Observability
    Goal
        Enable monitoring, debugging, and tracing.
    Design Choices
        Distributed Tracing
            Each request carries a Correlation ID.
        Metrics and Monitoring
            API latency
            Kafka lag
            Error rates
            WebSocket connections

            Monitoring tools: These provide dashboards and alerting.
                Prometheus
                Grafana

5. Security
    Goal
        Protect data, APIs, and communication.
    Design Choices
        Authentication & Authorization
            Use token-based authentication.
        Secure Communication
            All service communication uses: HTTPS / TLS encryption
            Prevents man-in-the-middle attacks.
        API Gateway Security
            Rate limiting
            Request validation
            Authentication