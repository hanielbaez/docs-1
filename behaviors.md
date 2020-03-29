---
description: Giving agents agency and specifying laws of the universe
---

# Behaviors

Agents can be sent messages by other agents through their `name` or `id`.

{% hint style="info" %}
Agent names _must_ be strings.
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

Agents can create new agents by sending a message to the special "hash" id.

For example, this behavior:

```javascript
(state, context) => {
    state.messages.push({
        to: "hash",
        type: "create_agent",
        data: {parent: state.agent_id, behaviors:["newborn"]}
    });
    return state;
}
```

leaves the parent unmodified while also creating a new agent.

To remove an agent, send a message with type 'remove\_agent' to 'hash'. Put the agent id in the data field as `agent_id`. For example, this agent will not live to see the next step of the simulation:

```javascript
(state, context) => {
    state.messages.push({
        to: "hash",
        data: { agent_id: state.agent_id },
        type: "remove_agent"
    });
    return state;
}
```

Agents have a special state property, "messages", which can be used to communicate with other agents. Between steps, messages are collected from their 'sending' agents and dispatched to their targets. For example, here Milford is going to send a message to Bueller on step 10.

```javascript
// step 10:
// simulation consists of two agents, Milford and Bueller:
[{
    agent_name: "Milford",
    messages: [{
        to: "Beuller"
        data: {
            words: "hey Beuller!"
            /* arbitrary manifest. */
        }
    }]
},
{
    agent_name: "Beuller",
    behaviors: ["checks_messages"]
}]
```

In computing step 11, agent behaviors are invoked. Milford has no behaviors, however his state is modified to remove the 'messages' from his state.

Bueller's checks\_messages behavior will be invoked, and they'll find Milford's message in their 'context.messages' collection, like this:

```javascript
context.messages : [
    {
        from: "<uuid of Milford>" /* note that the recipient gets the ID of the sender, so they can reply or store the sender's information, if they wish. */
        data: {
            words: "hey Beuller!"
            /* arbitrary manifest. */
        }
    }
]
```

Bueller can do whatever they'd like with it, including ignoring it, replying, etc. Let's assume 'checks\_messages' is a no\_op behavior, at our next step our world state is therefore:

```javascript
// step 11:
[{
    agent_name: "1",
    name: "Milford"
},
{
    agent_name: "2",
    name: "Beuller"
    behaviors: ["checks_messages"]
}]
```

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

### Special State

While an agent can store arbitrary data in its own state, some state values have special meaning in HASH. You're already familiar with a few: { agent\_id: string // Permits an agent to receive messages. agent\_name: string // Permits an agent to receive messages. behaviors: \[\] // Names of the hash library behaviors that the agent should apply to progress itself from simulation step N to N+1. messages: \[\] // Outbound messages from this agent position: \[x, y, z\] // to display agents in the viewer and to calculate neighbors }

There are a number of others which the simulation viewer looks at to try to infer meaningful ways to display the agent. Specifically:

* shape: either "box" or "cone". Overrides the default visualization for the agent.
* height: i64, if provided \(and agent is rendering on a 2d grid\), will set the display height of the agent.
* position \[i64, i64, i64\], if provided, will render the agent in 3d space \(and override the grid position\)
* direction \[i64, i64, i64\], if provided, will render the agent as a cone oriented along that unit vector
* color, if provided, will color the agent.  Color is defined using html, therefore named colors \("red", "green", "blue", etc.\) are supported,

  as are hash values \(`#223344`\) and rgb \(`rgb(12,244,155)`\)

### 

