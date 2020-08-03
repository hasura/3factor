# 3factor app

3factor app is an architecture pattern for modern full-stack apps. 3factor enables building apps that are robust and scalable from the get go by using modern API architectures along with the power of Cloud. [Jump](https://github.com/hasura/3factor-example) to reference implementation.

[We](https://hasura.io) propose an architecture pattern which is composed of 3 factors:

1. [Realtime GraphQL](realtime-graphql.md)
2. [Reliable eventing](reliable-eventing.md)
3. [Async serverless](async-serverless.md)

Consider a food ordering application built in traditional way vs 3factor way:

![3 factor architecture](./3factor-migration.png)

We can see here the essense of the architecture i.e. all business logic is invoked via events. The app manipulates state via a GraphQL API and is also subscribed (realtime) to any new state. We will describe the 3factors in more detail below.


## Factor #1: Realtime GraphQL

Apart from providing an amazing frontend developer experience, GraphQL is a crucial component in 3factor architecture as it allows flexible API access and realtime capabilities. The GraphQL API should have the following properties:

- **Low-latency**: An end-user should see [instant
  feedback](https://stackoverflow.com/a/164290/3364697) of an action (i.e. state manipulation) and not
  have to wait long on an API (<100ms ideal, upto 1 second at worst).
- **Support subscriptions**: Consume information "realtime" from the backend via GraphQL Subscriptions.
  Avoid the use of continuous polling (for scalability).

[Read more...](realtime-graphql.md)

## Factor #2: Reliable eventing

In 3factor, business logic is initiated via events. This removes complex state management in your API layer and defers it to bespoke business logic functions. Events can also be persisted so that entire history of state is available for observability. Further, the event system must have the following 2 properties:

- **Atomic**: Mutations to the application state should atomically create event(s).
- **Reliable**: Events should be delivered (to any consumer) with atleast-once guarantee.

[Read more...](reliable-eventing.md)

## Factor #3: Async serverless

Write business logic as event handling functions. Each function only cares about one event and is hence small & cohesive. The easiest way to deploy such functions is in serverless compute. Serverless minimizes backend ops and gives "infinite" scalability while being cost-efficient. The serverless functions should follow few best-practices:

- **Idempotent**: The code should be prepared for duplicate delivery of events.
- **Out-of-order**: Events may not be guaranteed to be received in any realtime order. The code should not depend on any expected sequence of events.

[Read more...](async-serverless.md)

---------------------------------------------------------

In short, a 3factor app requires you to remove state **management** from your API layer. It encourages fine-grained state changes and event generation (in data store) and corresponding event delivery (via event queues) to invoke business logic. The business logic can further change the state which is then delivered to subscribed clients via realtime GraphQL. 

The 3factor app architecture is an implementation of the [CQRS](https://martinfowler.com/bliki/CQRS.html) pattern in many ways. The frontend gives the "commands" and then "queries" (subscribes) for the new state while events invoke the business logic behind the scenes.

### Comparison to 12factor.net
The 3factor name is inspired from 12factor.net. 12factor.net, created 7 years ago by the folks at Heroku, is a guide/best-practices for creating microservices based applications for modern cloud. Although the name is similar, 3factor.app is actually an application design pattern.

### Reference implementation
A complete step-by-step reference implementation for a 3factor app is available at: [github.com/hasura/3factor-example](https://github.com/hasura/3factor-example/)

<!---
3factor, 3-factor, 3factor app, 3-factor app, Three Factor app, Three-Factor app
-->
