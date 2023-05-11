# This is where we synchronize our Clients observability efforts

The goal is to have homogeneus observability experience - metrics(namees)/logs(format)/span attributes.
Ideally two services written in different languages using our clients should generate observability data in a such way that doesn't require changing dashobard, alerts, log queries, etc

For example, .Net uses `DistributedContextPropagator` class with default propagator created by `CreateDefaultPropagator` method tis W3C propagator by default. While in Opentelemetry world propagators are configurable with defaults being noop.


## Opentelemetry as a reference
Current [Messaging semantics conventions document](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/semantic_conventions/messaging.md) covers some general messaging concerns as well as not so many rabbitmq-specific. 
Regarding stability of semantic conventions. It seems that community as well as WG agrees that it's a WIP, however current version of semantic conventions is still implemented and intended to be implemented whatever it is. For example, Azure SDK. 

How we change versions of clients when adapting to semantic conventions changes?
Shall we have separate documents to track other RabbitMQ protocols 

## How it's done by comminity and what is the de-facto standard

for each language there is a second official opentelemetry repository called opentelementry-<language>-contrib. It usually has a folder called instrumentation/<name-of-library> i.e. (https://github.com/open-telemetry/opentelemetry-ruby-contrib/tree/main/instrumentation/bunny)[https://github.com/open-telemetry/opentelemetry-ruby-contrib/tree/main/instrumentation/bunny]. 
So the Opentelemetry integration is done in a separate otel-contrib repo. 

Depending on facilities provided by the language/runtime different patterns used but the most common are:
- manual wrappers, like in go kafka instrumentation
- some sort of built-in callbacks, like telemetry library for erlang or micrometer if I understand it correctly. Basically probes.
- nice magic like Python's `__setattr__`. I.e. repointing original funciton to new one wrapping old one.

It's definitely worth studying how kafka clients integrated with OT and try to reuse same mechanics. 

## Implementation table for 0.9.1

| *Client* | *Logs* | *Metrics* | *Traces* | *Context propagation* | *Logging Trace Info* | *Tutorial* |
|-----|-----|-----|-----|-----|-----|-----|
| [Java](https://github.com/rabbitmq/rabbitmq-java-client/pull/1017)   |  SLF4J   |  Micrometer, OT   |  Micrometer(in progress)  |   |
| [.Net](https://github.com/rabbitmq/rabbitmq-dotnet-client/pull/1261)      |     |     |    ActivitySource(in progress)  |  DistributedContextPropagator(in progress) uses W3C |
| [Go](https://github.com/rabbitmq/amqp091-go/issues/43)      |     |     |  OT(in progress)   |  OT(in progress) (uses whatever configured) |

## List of third-party OpenTelemetry integrations for popular clients
- Bunny - Ruby - https://github.com/open-telemetry/opentelemetry-ruby-contrib/tree/main/instrumentation/bunny
- Pika - Python - https://github.com/open-telemetry/opentelemetry-python-contrib/tree/main/instrumentation/opentelemetry-instrumentation-pika
- Amqplib - Javascript(Node) - https://github.com/open-telemetry/opentelemetry-js-contrib/tree/main/plugins/node/instrumentation-amqplib

## WIP: Spans

Here we amend OT semantic coventions and implement them for Rabbit. 
Note: Semantic conventions are currently very fluid, below we list spans and thier attributes for current semconv version.
We track upcoming version here https://github.com/open-telemetry/oteps/pull/220 and here https://github.com/open-telemetry/opentelemetry-specification/issues/3395.
We also take into account existing 3d party integrations i.e. de-facto state of things in the community.

### Publish

### Receive

### Process
