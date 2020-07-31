# Factor #1: Realtime GraphQL

Factor #1 is to use realtime GraphQL as the API layer for your application. You will need the GraphQL layer for mutating state and for receiving realtime updates to state changes. Hence, the GraphQL layer should have the following 2 properties:

- **Low-latency**: An end-user should see [instant
  feedback](https://stackoverflow.com/a/164290/3364697) of an action (i.e. state manipulation) and not
  have to wait long on an API (<100ms ideal, upto 1 second at worst).
- **Support subscriptions**: Consume information "realtime" from the backend via GraphQL Subscriptions.
  Avoid the use of continuous polling (for scalability).

## Traditional vs 3factor

| Traditional                                                                | 3factor (Factor #1)                                |
| -------------                                                              | -------------                                      |
| Use REST APIs to interact with backend                                     | Use GraphQL to interact with backend               |
| Fetch related data via multiple calls (HATEOAS) or define complex resources | Fetch any kind of data in a single call            |
| Use Swagger or related tools for API docs                                  | Auto-generate entire API schema and related docs   |
| Setup custom APIs for realtime (or polling)                                | Use native GraphQL subscriptions                   |

## Benefits

The biggest benefit of GraphQL is the order of magnitude improvement in developer experience for the frontend. This directly translates to higher feature velocity for the app. Few of the reasons why this is possible:

1. With all the APIs available for introspection along with the ability to ask for precisely the data that one needs, GraphQL helps in massively accelerating frontend development.

2. GraphQL APIs are strongly-typed and hence enjoy all the benefits of strong types for e.g. type validation, auto-generating mock data, guranteed response structure and more.

3. With GraphQL, you don't need to setup and maintain another tool like Swagger for API discovery/documentation. The API schema lends itself to very simple documentation process which you can explore in a tool like GraphiQL.

4. Realtime makes for a much better user experience without blocking/polling on synchronous IO.

## Resources

There are many tools that can help you get started with building or using a realtime GraphQL server:

1. A catalog of server frameworks for various languages is listed here: [https://graphql.org/code/](https://graphql.org/code/)
2. Fullstack GraphQL bootcamp with Vladimir Novick: [Youtube](https://www.youtube.com/playlist?list=PL28aKhmSneX86qqmzVNjwYJ1OZORPFxAr)
3. Sara Vieira's workshop provides very good introductory material: [https://github.com/SaraVieira/graphql-workshop](https://github.com/SaraVieira/graphql-workshop)
4. Hasura GraphQL Engine gives instant realtime GraphQL on Postgres: [https://hasura.io](https://hasura.io)
5. AWS AppSync gives realtime GraphQL-as-a-service on DynamoDB and Aurora: [https://aws.amazon.com/appsync/](https://aws.amazon.com/appsync/)
