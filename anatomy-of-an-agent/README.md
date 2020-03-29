# Agents

As the name suggests, **agents** lie at the heart of _agent_-based modeling.

Every agent has a name and a unique identifier. As a simulation creator, you can set and change the name, but not the ID, so don't worry about including this when creating agents.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
{
    agent_id: <uuid v4>,
    agent_name: <string>
}
```
{% endtab %}

{% tab title="Python" %}
```python
class Agent:
    agent_id = <uuid v4>
    agent_name = <string>
    
```
{% endtab %}

{% tab title="Rust" %}
```rust
struct Agent {
    agent_id: uuid,
    agent_name: String
}
```
{% endtab %}
{% endtabs %}

Naming your agent is entirely opiontla. The simplest possible agent is simply `{}` \(although it won't do much of anything!\)

Your agents can contain any fields you want. Here's an agent we have that uses the Monte-Carlo method to approximate the value of pi via randomness:

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

Notice how we use multiple custom fields to store data in the agent. This will come in handy during the _analysis_ phase where we can actually see how these values changed during the simulation.

### 

