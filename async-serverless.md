# Factor #3: Async serverless

Factor #3 is to use serverless backends for business logic. As Factor #2 gives us a reliable eventing system, your business logic will comprise of event handlers (think of them as fine-grained microservices) which receive an event, perform computations and write back to state. This makes your app a composition of several functions and confers all the benefits (and some drawbacks) of event-driven architectures like high feature velocity due to the loose coupling of components. Serverless minimizes backend ops and gives "infinite" scalability while being cost-efficient. The serverless functions should have the following properties:

- **Idempotent**: The code should be prepared for duplicate delivery of events.
- **Out-of-order**: Events may not be guaranteed to be received in any realtime order. The code should not depend on any expected sequence of events.

## Traditional vs 3factor

| Traditional                                 | 3factor (Factor #3)                       |
| -------------                               | -------------                             |
| Write synchronous procedural business logic | Write loosely coupled event handlers      |
| Deploy on VMs or containers                 | Deploy on serverless platforms            |
| Manage the runtime yourself                 | Platform manages the runtime              |
| Requires operational expertise              | Does not require operational expertise    |
| Implement auto-scale, if possible           | Auto-scaling by default                   |

## Benefits

Writing business logic in an event-driven architecture has many [benefits and some challenges](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/event-driven). Once you have developed your application locally, using say HTTP event handlers, it is quite seamless to deploy the same on serverless platforms. Serverless, although still maturing, promises many capabilities:

1. No ops: Serverless manages the runtime of the application and reduces the operational burden on the team as much as possible.

2. Free scale: Serverless auto-scales based on utilization without having to setup any additional infrastructure.

3. Cost: Serverless is priced on a per-request basis. Hence, it is very cost-efficient for bursty workloads which would otherwise be billed on traditional infrastructure even while not being used.

## Reference implementation

There are many serverless platforms where you can host your backend:

1. AWS Lambda
2. Google Cloud Functions
3. Azure Functions
4. Zeit
5. OpenFaas, Kubeless, Knative (for Kubernetes)
6. Netlify
