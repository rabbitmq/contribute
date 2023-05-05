# This is where we synchronize our Clients observability efforts

The goal is to have homogeneus observability experience - metrics(namees)/logs(format)/span attributes.
Ideally two services written in different languages using our clients should generate observability data in a such way that doesn't require changing dashobard, alerts, log queries, etc

For example, .Net uses `DistributedContextPropagator` class with default propagator created by `CreateDefaultPropagator` method tis W3C propagator by default. While in Opentelemetry world propagators are configurable with defaults being noop.


## Opentelemetry as a reference
Current [Messaging semantics conventions document](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/semantic_conventions/messaging.md) covers some general messaging concerns as well as not so many rabbitmq-specific. 

How we change versions of clients when adapting to semantic conventions changes?
Shall we have separate documents to track other RabbitMQ protocols 


## Implementation table for 0.9.1

| *Client* | *Logs* | *Metrics* | *Traces* | *Context propagation* | *Logging Trace Info* | *Tutorial* |
|-----|-----|-----|-----|-----|-----|-----|
| [Java](https://github.com/rabbitmq/rabbitmq-java-client/pull/1017)   |  SLF4J   |  Micrometer, OT   |  Micrometer(in progress)  |   |
| [.Net](https://github.com/rabbitmq/rabbitmq-dotnet-client/pull/1261)      |     |     |    ActivitySource(in progress)  |  DistributedContextPropagator(in progress) uses W3C |
| [Go](https://github.com/rabbitmq/amqp091-go/issues/43)      |     |     |  OT(in progress)   |  OT(in progress) (uses whatever configured) |
