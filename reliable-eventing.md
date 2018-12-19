# Factor #2: Reliable eventing

Factor #2 is to use an event system to manage the state of the app. Instead of writing long procedures which manipulate state in-memory and persist them in the database at the end, perform simple operations (for e.g. CRUD on a resource) that initiate events and persists them to an event log. The backend should listen (or be triggered) from this immutable event log. The event system should have the following properties:

- **Atomic**: Mutations to the application state should atomically create event(s).
- **Reliable**: Events once emitted should be delivered (to any consumer) atleast once.

## Traditional vs 3factor


| Traditional                                                                                                          | 3factor (Factor #2)                                                          |
| -------------                                                                                                        | -------------                                                                |
| On an API call: fetch relevant resources, perform business logic in a transaction and finally commit the transaction | On an API call: Produce and persist event(s) which represents the intent |
| Avoid using async functionalities in a procedure because it is hard to rollback on failure                         | Fundamentally async                                                          |
| Implement error recovery logic in case of crashes                                                                    | No error recovery logic required as events are atomic                        |

## Benefits

There are numerous benefits of architecting apps based on an immutable event log:

1. The biggest advantage in writing and reading from an event log is how it seamlessly handles error recovery. Whereas in a traditional workflow there will be some additional logic to recover from intermittent crashes because of partial side-effects, there is no such logic required if your application is emitting atomic events. The application proceeds (crash or not) based on the current event log.

2. With the application only producing and persisting events, there is a detailed audit trail of each action in the application. Hence, we get logging and observability built-in with this type of system.

3. In case you need to replicate your entire app in a different environment, it is easy to do so by just reprocessing each event in the event log.

## Reference implementation

Coming soon.

## Video

Coming soon.
