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

## Getting and Setting State

The state object has accessor methods for getting and setting state.

* state.get\(field\_name\) : returns the value of the field
* state.set\(field\_name, value\) : sets the field as the value
* state.modify\(field\_name, fn\(field\): return value\) : get's and sets the field.

Example:

This behavior takes in the current state and [context](context.md) of the agent, adds 1 to the age property stored on the state, and then returns the state. 

```python
def behavior(state, context):
    age = state.get("age")
    age += 1
    state.set("age", age)
    return state
```

or, using modify

{% tabs %}
{% tab title="Python" %}
```python
def behavior(state, context):
    state.modify("age", age: return age += 1)
    return state
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
function behavior(state, context) {
    state.modify("age", age => age + 1)
}
```
{% endtab %}
{% endtabs %}

Important: Only the agent can modify its own state. If an agent wants to prompt another agent to perform a state change, it can send a message to trigger an update. 

{% hint style="info" %}
Agents can read one another's state - for example if agent "foo" is a [neighbor](context.md) of agent "bar", agent "bar" can access the fields of agent "foo", it just can't make any changes to those fields. That's what makes the state _**private**_.
{% endhint %}

