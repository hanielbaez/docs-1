# Handling Messages

So far, we've talked about sending messages. What happens when an agent receives a message?

As we've mentioned previously, the Context passed to every agent provides a list of messages in the agent's inbox in the `messages` field. Here, we can iterate through the list of messages sent to the agent and make some decisions.z

```javascript
Context {
    properties: {
        /* access to the simulation's global constants, aka the 'Properties' tab. */
    },
    neighbors: [
        /* A collection of the agent's neighbors.
        Neighbors are calculated using the 'position' field.
        */
    ],
    messages: [
        /*  
            Any messages send to the given agent on this step.
            If the agent wants to preserve access to these on future steps, they'll need to store them in their own state.
         */
    ],
    /* other context values to come. */
}
```

