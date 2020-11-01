---
description: The beating hearts of agent-based models
---

# Agents

As the name suggests, **agents** lie at the heart of _agent_-_based_ modeling.

Every agent has a name and a unique identifier. As a simulation creator, you can set and change the name, but not the ID. The HASH Engine takes care of this for you, so **don't worry about including the ID when creating agents.**

{% tabs %}
{% tab title="JavaScript" %}
```javascript
{
    agent_id: <uuid v4>, //Set on_create by the hEngine
    agent_name: <string>
}
```
{% endtab %}

{% tab title="Python" %}
```python
class Agent:
    agent_id = <uuid v4> #Set on_create by the hEngine
    agent_name = <string>
```
{% endtab %}

{% tab title="Rust" %}
```rust
struct Agent {
    agent_id: uuid, //set on_create by the hEngine
    agent_name: String
}
```
{% endtab %}
{% endtabs %}

Naming your agent is entirely optional. The simplest possible agent is simply `{}` \(although it won't do much of anything!\)

An individual agent has a [state](state.md) and a [context](context.md).

![An Agent](../.gitbook/assets/image%20%2814%29.png)

When we define the [initial conditions](initial-state.md) of a simulation, we're defining the initial agents that will be present in the first timestep of the simulation, each of which will have its own state and context.

![Three agents, ready to simulate.](../.gitbook/assets/image%20%2813%29.png)

