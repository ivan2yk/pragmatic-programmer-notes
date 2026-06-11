# Event Driven Architecture

## Concept
Event Driven Architecture (EDA) is an architectural pattern where services communicate by producing and consuming events. Instead of services calling each other directly, they react to things that happen. This decouples producers from consumers — the producer doesn't know or care who reacts to its events.

## Benefits
- **Loose coupling** — producers and consumers don't know about each other, changes in one don't break the other
- **Scalability** — consumers can scale independently based on event load
- **Resilience** — if a consumer goes down, events queue up and get processed when it recovers
- **Flexibility** — new consumers can react to existing events without touching the producer
- **Natural fit for microservices** — services stay independent and communicate asynchronously

## Tools

| Tool | Provider |
|---|---|
| **Azure Event Hub** | Azure — high volume event streaming (telemetry, logs, IoT) |
| **Azure Service Bus** | Azure — reliable event/message delivery between services |
| **Apache Kafka** | Open source / multi-platform |
| **RabbitMQ** | Open source / multi-platform |
| **AWS EventBridge** | AWS |
| **AWS SNS / SQS** | AWS |
| **Google Pub/Sub** | GCP |

## Key Patterns

- **Choreography vs Orchestration**
  - **Choreography** — each service reacts to events independently, no central coordinator. Natural fit for EDA
  - **Orchestration** — one central service coordinates and tells others what to do. More control but more coupling

- **Event Sourcing**
  - Instead of storing current state, you store the sequence of events that led to that state
  - Example: instead of storing "balance = 500", store "deposited 300, deposited 400, withdrew 200"
  - Common in banking systems

- **CQRS (Command Query Responsibility Segregation)**
  - Separate the model that writes data from the model that reads data
  - Often paired with Event Sourcing in EDA systems

- **Dead Letter Queue**
  - When an event can't be processed, it goes to a special queue for inspection and retry
  - Azure Service Bus supports this natively