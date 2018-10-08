# 3factor

3factor apps are fast to iterate on, resilient and highly scalable. They promise
great user _and_ developer experience.

The 3 factors for an application backend are:

1. Realtime GraphQL
2. Event-driven
3. Async serverless

## Realtime GraphQL: Iterate faster on your frontend

GraphQL APIs promise a much faster front-end developer workflow. In addition
your API should also be:

- Low-latency: An end-user should see [instant
  feedback](https://stackoverflow.com/a/164290/3364697) of an action and not
  have to wait on an API (<100ms ideal, upto 1 second at worst).
- GraphQL API should support subscription for consuming information
  asynchronously from the backend i.e a “realtime” GraphQL API. Refactor
  high-latency synchronous API responses to be reactive instead.

Example: Instead of REST APIs, use GraphQL as much as possible to make app
development faster. Further, consider a naive GraphQL mutation to place an order
that would have executed a workflow or orchestrated microservices and hence
taken a longer time to respond. Refactor this to an “atomic” GraphQL mutation
that “places” an order and responds with an “order-id”. Update UI based on
realtime workflow updates to the order-id, that the end-user can consume,
confident that the order is placed and does not need their attention.

## Event-driven: Make your backend resilient

Remove workflow and orchestration state from your backend APIs and persist them
into events:

- Changes to the backend datastore, via GraphQL mutations or otherwise should
atomically emit events.
- Events should be delivered reliably.

Example: Instead of writing an “place order” API endpoint that orchestrates
upstream microservices by making API calls to them in a workflow, emit events
that capture the state machine. Your event system should deliver the events
reliably to other microservices. This makes your application resilient to
transient failures because the retry/failure logic is captured in the event
system. It makes your application resilient to larger scale failures because the
event-system and your data store can easily be replicated across availability
zones making application recovery straightforward.

## Async serverless: Scale your backend infinitely

Write business logic that scales infinitely and requires no ops:

- Most business logic as far as possible should be written in serverless
functions (or microservices) that get triggered by events.
- These functions can also modify the backend state which may trigger further
events and which the end-user app can subscribe to via GraphQL subscriptions if
required.
This also allows for rapid iteration in the business logic without impacting the
GraphQL contract.

Example: In your food ordering workflow, instead of writing a payment processing
microservice that captures different failure modes, write a payment processing
function that processes a payment or fails. The event system should capture the
retry or failure handling logic so that your business logic is simple and easy
to scale.

A 3factor app requires you to remove state from your code and put it in your
datastore and in your event queues. Making your business logic asynchronous
requires a proportional investment in your realtime GraphQL API to allow the
end-user app to consume asynchronous information easily. A 3factor app is
analogous to the redux dataflow model on a react app, but applied to the
fullstack.
