# Context

Besides their [**state**](state.md), an agent always has access to their **context**. The context is the parts of the simulation that the agent is exposed to; another way of thinking about it is that an agent's context are the parts of the simulation  that the agent "can see".

The context is an object with three functions, **neighbors\(\), messages\(\)**, and **globals\(\)**.

```javascript
Context {
    globals(): {
        /* returns an immutable JSON object of the simulation's 
           constants, as defined in globals.json */
    },
    neighbors(): [
        /* Returns a collection of the agent's neighbors.
        Neighbors are calculated using the 'position' field.
        */
    ],
    messages(): [
        /*  
            Returns a collection of messages received by the agent this 
            timestep.
         */
    ],
    /* other context values to come. */
}
```

Context is 'read-only' and can't be changed directly by the agent. 

By default the agent's context, along with the agent's state, is passed into every [behavior](../behaviors/) attached to the agent. 

