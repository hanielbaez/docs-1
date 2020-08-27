# Context

Besides their [**state**](state.md), an agent always has access to their **context**. The context is the parts of the simulation that the agent is exposed to; another way of thinking about it is that an agent's context are the parts of the simulation  that the agent "can see".

The context is an object with four functions, **neighbors\(\), messages\(\)**, **globals\(\)** and **data\(\)**.

```javascript
context {
    globals(): {
        /* 
            Returns an immutable JSON object of the simulation's 
            constants, as defined in globals.json 
        */
    },
    neighbors(): [
        /* 
            Returns a collection of the agent's neighbors.
            Both the "position" and "search_radius" fields must be set in order
            to properly calculate and return neighbors
        */
    ],
    messages(): [
        /* 
            Returns a collection of messages received by the agent this 
            timestep 
        */
    ],
    data(): {
        /* 
            Returns an immutable JSON object of the simulation's datasets
            that have been added through the "Add to Simulation" toolbar
        */
    }
    /* other context values to come. */
}
```

Context is 'read-only' and can't be changed directly by the agent. 

By default the agent's context, along with the agent's state, is passed into every [behavior](../behaviors/) attached to the agent.

