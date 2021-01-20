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

The state object can be accessed as an object in JavaScript or a dictionary in Python.

* `state.<field_name>` : access and edit the value of the field in JavaScript
* `state['<field_name>']` : access and edit the field in Python

**Example**:

This behavior takes in the current state and [context](context.md) of the agent, adds 1 to the age property stored on the state, and then returns the state.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const behavior = (state, context) => {
    state.age += 1;
}
```
{% endtab %}

{% tab title="Python" %}
```python
def behavior(state, context):
    state['age'] += 1
```
{% endtab %}
{% endtabs %}

Important: Only the agent can modify its own state. If an agent wants to prompt another agent to perform a state change, it can send a message to trigger an update.

{% hint style="info" %}
Agents can read one another's state - for example if agent "foo" is a [neighbor](context.md) of agent "bar", agent "bar" can access the fields of agent "foo", it just can't make any changes to those fields. That's what makes the state _**private**_.
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
  search_radius: number // context.neighbors() will return a list of all the agents within the search radius
  direction: [x, y, z] // Will affect the visualization of the agent. Can be used for custom logic
  velocity: [x, y, z] // Will affect the visualization of the agent. Can be used for custom logic
  color: string // Color of the agent
  rgb: [r, g, b] // Color of the agent represented as an rgb array
  height: number // Height of the agent in the 3D Viewer
  scale: [x, y, z] // Agent model will be scaled along the corresponding axes
  shape: string // Determines the shape of the agent in the 3D Viewer
  hidden: boolean // Determines whether the agent is hidden in the 3D Viewer
  position_was_corrected: boolean // Used by the agent whenever agents reach topology bounds
}
```

