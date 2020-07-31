# Factor #2: Reliable eventing

Factor #2 is to use an event system to invoke business logic. Instead of writing long procedures which manipulate state in-memory and persist them in the database at the end, perform simple operations (for e.g. CRUD on a resource) that generates events and pushes them to an event queue. It is recommended that the events be persisted in an event log to keep the entire history of state changes. The event system then delivers these events to business logic functions. The event system should have the following properties:

- **Atomic**: Mutations to the application state should atomically create event(s).
- **Reliable**: Events should be delivered (to any consumer) with atleast-once guarantee.

## Traditional vs 3factor


| Traditional                                                                                                          | 3factor (Factor #2)                                                      |
| -------------                                                                                                        | -------------                                                            |
| On an API call: fetch relevant resources, perform business logic in a transaction and finally commit the transaction | On an API call: Produce and persist event(s) which represents the action |
| Avoid using async functionalities in a procedure because it is hard to rollback on failure                           | Fundamentally async                                                      |
| Implement error recovery logic in case of crashes                                                                    | Simple error recovery logic by redelivering the event                    |

## Benefits

There are numerous benefits of architecting apps based on an immutable event log:

1. The biggest advantage in making your app event-driven is how it seamlessly handles error recovery. Whereas in a traditional workflow there will be some additional logic to recover from intermittent crashes because of partial side-effects, there is no such logic required if your application is emitting atomic events which can be easily retried on errors/failures.

2. With the application only producing and persisting events, there is a detailed audit trail of each action in the application. Hence, we get logging and observability built-in with this type of system.

3. In case you need to replicate your entire app in a different environment, it is easy to do so by just reprocessing each event in the event log.

## Reference implementation

A reliable event system can be hard to implement but it can be done in few ways:

1. [Change data capture](https://en.wikipedia.org/wiki/Change_data_capture) patterns can be used to atomically generate events on database changes. There are tools like [Debezium](https://debezium.io/) which provide reliable streaming of change events to your application.

2. One common change data capture pattern is using triggers on tables to write to an event log. A implementation of such a system is [Hasura Event Triggers](https://hasura.io/event-triggers).

3. Event sourcing is another pattern which can be used to set up an event system: [https://martinfowler.com/eaaDev/EventSourcing.html](https://martinfowler.com/eaaDev/EventSourcing.html)

