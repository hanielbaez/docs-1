# State

Every agent has a private **state**. Is the agent of height 1 or height 2? Is the agent's name "foo" or is its name = "bar"? These properties are expressed and saved on the agent's state.

Your agents can have any fields you want. Here's an agent that uses the Monte-Carlo method to approximate the value of pi via randomness:

```javascript
// LeaderAgent
{
    agent_name: "leader",
    behaviors: ["spawn_samples.js", "estimate_pi.js"],
    spawned_datapoints: 0,
    neighbor_points: 0,
    pi_estimate: 0,
    position: [0, 0, 0],
    search_radius: 1,
}
```

Notice how we use multiple custom fields to store data in the agent.

## Getting and Setting State

The state object has accessor methods for getting and setting state.

* `state.get(field_name)` : returns the value of the field
* `state.set(field_name, value)` : sets the field as the value

**Example**:

This behavior takes in the current state and [context](context.md) of the agent, adds 1 to the age property stored on the state, and then returns the state.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const behavior = (state, context) => {
    let age = state.get("age");
    age += 1;
    state.set("age", age);
}
```
{% endtab %}

{% tab title="Python" %}
```python
def behavior(state, context):
    age = state.get("age")
    age += 1
    state.set("age", age)
```
{% endtab %}
{% endtabs %}

Important: Only the agent can modify its own state. If an agent wants to prompt another agent to perform a state change, it can send a message to trigger an update.

{% hint style="info" %}
Agents can read one another's state - for example if agent "foo" is a [neighbor](context.md) of agent "bar", agent "bar" can access the fields of agent "foo", it just can't make any changes to those fields. That's what makes the state _**private**_.
{% endhint %}

### Modify

HASH also has a "modify" helper function which allows you to make certain state changes a one-liner:

* `state.modify(field_name, function)` 

will allow you to pass a function that will be run on the given field in the agent's state. The above example can now be written like so:

{% tabs %}
{% tab title="Javascript" %}
```javascript
const behavior = (state, context) => {
    state.modify("age", age => age + 1)
}
```
{% endtab %}

{% tab title="Python" %}
```python
# We haven't yet enabled this helper function for Python
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The state value will be set to the **return value** of the function. If you use a method that modifies a variable in place, like`Array.push(x)`the return value is `x` and not the modified array.
{% endhint %}

### Reserved Fields

While an agent can store arbitrary data in its own state, some state values have special meaning in HASH. The fields below are all reserved, in addition to fields tied to visualization which can be found [here](visualization/).

```javascript
{ 
  agent_id: string // auto-created identifier. Agents receive messages to their ID.
  agent_name: string // optional identifier. Agent's receive messages to their name. 
  behaviors: [] // Filenames of the behaviors that the agent apply to advance their state every simulation step N to N+1. 
  messages: [] // Outbound messages from agent 
  position: [x, y, z] // displays agents in the viewer and used to calculate neighbors 
  search_radius: i64 // context.neighbors() will return a list of all the agents within the search radius
  direction: [x, y, z]
  velocity: [x, y, z]
}
```

