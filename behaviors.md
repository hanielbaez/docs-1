---
description: Giving agents agency and specifying laws of the universe
---

# Behaviors

Agents can be sent messages by other agents through their `name` or `id`.

{% hint style="info" %}
Any agent's `name` must be a string.
{% endhint %}

{% hint style="danger" %}
Please do not try to set or change an agent id. 
{% endhint %}

Agents can \(and typically do\) have behaviors:

```javascript
{
    ...
    behaviors: ['eats_fruit', 'messages_friends_about_fruit', 'has_friends']
}
```

Behaviors allow agents to have _agency_ in the simulation. An agent can have any number of behaviors.

Behaviors are pure functions in the form of: `(last_state, context) => [next_state]`

The exact semantics vary by programming language, but in spirit every behavior is a pure function which receives a given agent's last state, the world context visible to that agent, and outputs its next state, potentially along with the states of some to-be-created agents.

Most behaviors output a single state with the same agent\_id as they received. For example, this behavior:

```javascript
(state, context) => {
    state.position[0] += 1;
    return state;
}
```

causes an agent to move along the x axis.

Agents can create new agents by sending a message to the special "hash" id. This is covered more in-depth in [Messages](agent-messages/handling-messages.md).



### Context

The second parameter to a behavior, context, contains the world/simulation state than an agent is allowed to see. Here's what context currently provides:

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

### 

