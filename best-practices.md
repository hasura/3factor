# Best Practices

## Error handling

These are some recommendations for better error handling in 3factor apps.

- **Error webhook**: There should be an error webhook that the event system falls back to, in case of unexpected errors. In this way, if something goes wrong, your event system can call the webhook with the details and the webhook can handle the error.
- **Retry configuration**: The event system should have a valid retry configuration i.e. if the serverless function does not return 200, the event system should retry "n" number of times before falling back to the error webhook.
- **Timeout**: There should be an appropriate timeout in your serverless functions so that the client does not have to wait indefinitely for updates. Your serverless functions should return a 500 if timeout is reached and the event system can retry invoking the function or calling the error webhook.
- **Client intervention**: In rare cases when the error webhook is down, the client must intervene to take appropriate actions if it has been waiting for a long time.
- **Error state in GraphQL Schema**: It is good to have a field in your GraphQL schema that keeps track of errors, mainly unexpected errors. Your client should be subscribed to this field so that it is is notified in case of unexpected errors.
- **Idempotency**: The 3factor spec needs your serverless functions to be idempotent. This means, your serverless functions should be ready to handle cases when the same event is delivered to it multiple times. This is important in cases when the function has mutated the state but did not return 200 and the event system retries.
- **Avoid contacting APIs directly**: It is best to call APIs via the event system whenever possible because you can reuse the retry and fallback logic without writing it again on the client.

## FAQ

**Q: What happens when my serverless function is down?**
A: Your event system must have a valid retry configuration i.e. whenever the function does not receive a successful response, the event system should retry at most *n* times. If the function still does not receive a successful response, then the event system must call an *Error Webhook* with the details where you can handle the situation however you want (such as setting the action as cancelled, trying alternate webhooks etc).

**Q: What happens if there is an unexpected internal error?**
A: Your event system must have a valid retry configuration i.e. whenever the function does not receive a successful response, the event system should retry at most *n* times. If the function still does not receive a successful response, then the event system must call an *Error Webhook* with the details where you can handle the situation however you want (such as setting the action as cancelled, try alternate webhooks etc).

**Q: What happens when my client crashes?**
A: Nothing :) The event system, the state, and the serverless function are outside your client application. This means that you can just refresh your client and everything will just work.

**Q: What happens when the error webhook is down?**
A: If your serverless functions are working fine, your app would still work fine. In cases where the client has waited long enough for updates, the client should intervene to cancel the action or notify the UI about the delay.

**Q: How do I ensure idempotency?**
A: Your serverless functions should be ready to handle cases when the same event is delivered to them multiple times. This is important in cases when the function has mutated the state but did not return 200 and the event system retries. A good way to do this is to make your serverless functions check the state of your application before performing any mutation whatsoever.

