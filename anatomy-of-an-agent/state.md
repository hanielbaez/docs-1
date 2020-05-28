# State

Every agent has a private **state**. The agent's state reflects the... well the state of the agent! Is the agent of height 1 or height 2? Is the agent's name "foo" or it's name = "bar"? These fields are expressed and saved on the agent's state. 

Your agents can have any fields you want. Here's an agent that uses the Monte-Carlo method to approximate the value of pi via randomness:

```javascript
LeaderAgent {
    spawned_datapoints: 0,
    neighbor_points: 0,
    pi_estimate: 0,
    behaviors: ["spawn_samples", "estimate_pi"],
    position: [0, 0, 0],
    agent_name: "leader",
    search_radius: 1,
}
```

Notice how we use multiple custom fields to store data in the agent.

Important: Only the agent can modify its own state. For example an agent could have a [behavior](../behaviors/) that modifies its age field.

```python
def behavior(state, context):
    state.age += 1
    return state
```

This behavior takes in the current state and [context](context.md) of the agent, adds 1 to the age property stored on the state, and then returns the state. Since the behavior is being run by the agent, it can change the state.

{% hint style="info" %}
If an agent wants to prompt another agent to perform a state change, it can [send a message](../agent-messages/) to trigger an update. 
{% endhint %}

{% hint style="info" %}
Agents can read one another's state - for example if agent "foo" is a [neighbor](context.md) of agent "bar", agent "bar" can access the fields of agent "foo", it just can't make any changes to those fields. That's what makes the state _**private**_.
{% endhint %}

