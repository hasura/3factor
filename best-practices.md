# Best Practices

## Error handling

These are some recommendations for better error handling in 3factor apps.

- **Error webhook**: There should be an error webhook that the event system falls back to, in case of unexpected errors. In this way, if something goes wrong, your event system can call the webhook with details and handle the error.
- **Retry configuration**: The event system should have a retry configuration i.e. if the serverless function does not return 200, the event system should retry "n" number of times before falling back to the error webhook.
- **Timeouts**: There should be an appropriate timeout in your serverless functions so that the client does not have to wait indefinitely for updates. Your serverless functions should return a 500 if timeout is reached and the event system can either retry or call the error webhook.
- **Client intervention**: In rare cases when the error webhook is also down or unable to handle failures, the client must intervene to take appropriate actions (like force abort) if a threshold period is crossed.
- **Subscribe to errors**: It is good to have a type in your GraphQL schema that keeps track of errors. Your client should be subscribed to this type so that it can respond pro-actively to errors.
- **Duplicate events**: The 3factor spec needs your serverless functions to be idempotent. This is important in cases when the function has mutated some state but failed to return 200 and the event system retries delivery.
- **Avoid contacting APIs directly**: It is recommended to call APIs via the event system whenever possible because you can reuse the retry and fallback logic without writing it again on the client.

## FAQ

**Q: What happens when my serverless function is down?**<br>
A: Your event system must have a valid retry configuration i.e. whenever the function does not receive a successful response, the event system should retry at most *n* times. If the function still does not receive a successful response, then the event system should call an *error webhook* with details so that you can handle the situation however you want (such as aborting the action, notifying support or trying alternate webhooks, etc).

**Q: What happens if there is an unexpected internal error?**<br>
A: Same as above. Your event system must have a valid retry configuration i.e. whenever the function does not receive a successful response, the event system should retry at most *n* times. If the function still does not receive a successful response, then the event system should call an *error webhook* with details so that you can handle the situation however you want (such as aborting the action, notifying support or trying alternate webhooks, etc).

**Q: What happens if there is an expected downtime?**<br>
A: Sometimes, your app might be under high load which you want to throttle or you are undergoing some critical upgrades for which you want to shutdown certain services. In such cases, your app should be subscribed to an error API (via realtime GraphQL) so that it can handle such errors gracefully.

**Q: What happens if my client crashes?**<br>
A: Nothing :) The event system, the state, and the serverless functions are outside your client application. This means that you can just refresh your client and everything should just work.

**Q: What happens when the error webhook is down?**<br>
A: In case there are internal errors and your error webhook is also down, your frontend might be stuck waiting for events. In such cases, if the client has waited long enough for updates, it should intervene to cancel the action or notify the UI about the delay.

**Q: How do I ensure idempotency?**<br>
A: Your serverless functions should be ready to handle cases when the same event is delivered to them more than once. A good way to do this is to make your serverless functions fetch the current state of your application (for that particular event) before performing any mutations. You should ideally perform the state check and mutation in a single transaction.

