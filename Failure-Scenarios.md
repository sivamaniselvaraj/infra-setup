1. Messaging system is unavailable
	by using outbox pattern when the messaging system in available the data are published to the event topics by a backend publisher(batch).

2. Database failure during order creation. (Assuming other services are up)
Transaction never commits and no order is created
Circuit breaker is open
Client receives an error message
Saga never starts

3. Processing service crash during execution
As we are using saga pattern a compensation process is triggered and order service is called to update the order status/ rollback the order entire transaction.

4. Notification service downtime
	all the events are store in the outbox events table. When the service is available those notification from the order and processing service are published to the notification topic.

5. Duplicate order requests
	by using idempotent key/events table we can handle the duplicate order request
    -> check if the event_id exists in the events table
    -> if exists skip the event
    -> else process the event
    -> update the processed_status in event table to “processed” 
	
6. Service instance crashes during processing
    Circuit breaker is open to keep the system stable.
        Event driven architecture emits the failure message to the service.
        Start the compensation or retry mechanism. 
        Update the status on the event table
        Send notification to the client 
    Eg.
        Order created
        Insert into orders
        Insert into orderItems
        Insert into outbox event
        Publish to kafka event (OrderCreated)
        Processing service consumes OrderCreated event
        Insert into jobs
        Insert into processing status
        Insert into outbox event
        Publish to kafka event (OrderProcessed)

    If failure occurs
        emit an event to order service kafka topic(processing_failed)
