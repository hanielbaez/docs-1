---
description: Important considerations to keep in mind when building your simulation
---

# Designing with the Actor Model

## Actors and Messaging

HASH is designed around an [actor oriented programming model](https://en.wikipedia.org/wiki/Actor_model).

* **Each agent in a simulation is an actor** with its own private state.
* **Actors communicate by sending and receiving** [**messages**](../agent-messages/), through which they may attempt to influence other agents to change state.
* **Ultimately agents are responsible for changing their own state.** An agent's state can only ever be modified by the agent itself.

This is key to handling the computational complexity of large simulations, as it removes the need for lock-based synchronization. An unlimited number of agents can act without blocking one another's execution, and you can trust that an agent's state will only be changed by that agent, cleanly separating the inputs and outputs of agents.

> “The big idea is 'messaging' … The key in making great and growable systems is much more to design how its modules communicate rather than what their internal properties and behaviors should be.”  
> **Alan Kay**

## Timing and Race Conditions

A benefit of the actor model is that HASH avoids race conditions by default, through isolation of state. Agents calculating the next step \(_t+1_\) are only able to use the information they currently have available to them; the state and context from the current step \(_t_\). Because each agent has a private state HASH won't run into conflicts - only one thing, the agent itself, can change its state. 

{% hint style="info" %}
The updates to each agents individual state, and which in the aggregate create the next state of the simulation, are applied synchronously. Messages between agents are collected and distributed by the engine between _t_ and _t+1_.
{% endhint %}

There won't be a situation where the order in which an agent is run changes the next state of the simulation. However you might encounter something like a race condition if you don't take into account the "travel time" of messages.

Since HASH implements an actor oriented paradigm, information is sent between agents through messages. If at timestep 1 \(_t_\) Agent A sends Agent B a message, Agent B will receive it and be able to act on the message in timestep 2 \(_t+1_\). The earliest Agent A could receive a response is in timestep 3 \(_t+2_\).. 

![](../.gitbook/assets/image%20%2815%29.png)

### Redundant Messaging

**Watch out for redundant messaging.** You can run into trouble with a naive message sending pattern where an agent sends messages until it receives a response.

```javascript
// Potentially Bad 

function behavior(state, behavior) {
    if (!receivedResponse) {
        sendStateChangeMessage()
    }
}
```

This would send a message at timestep 1 \(_t_\) and timestep 2 \(_t+1_\), and if the message was prompting Agent B to make a change to its state, you might inadvertently apply an update twice.

You can avoid this in several ways, including by modifying Agent A's send behavior to only send every n timesteps, or modifying Agent B such that the effect is the same whether it receives one or many messages \(e.g. an idempotent messaging pattern\).

### Sequential Behavior Execution

**Another potential pitfall to be aware of is that behaviors run sequentially.** So if in Behavior A you send a message and in Behavior B you check if you received a message and set receivedRespone = True, if the agents behavior array is:

```javascript
 ["behaviorA", "behaviorB"]
```

a message would be sent before the agent checks if they've received a response.

