---
description: Add and remove agents by interacting with HASH Core
---

# Built-in message handlers

In addition to the custom messages you can send between individual agents in the simulation, HASH Core has a set of built-in messages that enable more advanced functionality.

Currently, the most powerful built-in message handlers allow agents to add or remove other agents from the simulation. These messages must be sent to `hash` to get processed by Core. If not, they will be directed to an agent with a matching ID/name, and you will be very confused. If the agent with a matching name doesn't exist, the message goes unsent and nothing will happen. Again, you will be very confused.

### Removing Agents via Messages

Any agent can remove any other agent with a special message sent directly to `hash`. Here, we remove an agent with the agent\_id of `Bill`. Before the next step starts executing, the message will be processed and `Bill` will be removed \(sorry Bill!\). 

{% hint style="warning" %}
Do note that **case sensitivity** **matters**. If a message is sent to `bill`, it will not get sent to `Bill`.
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
(state, context) => {
    state.messages.push({
        to: "hash",
        type: "remove_agent"
        data: { agent_id: "Bill" },
    });
    return state;
}
```
{% endtab %}

{% tab title="Python" %}
```python
    message =	{
      "brand": "schelling",
      "type": "data_point",
      "data": {
        "num_agents": 50
      }
    }
```
{% endtab %}

{% tab title="Rust" %}
```rust

```
{% endtab %}
{% endtabs %}

However, our goal is to prevent you from shooting yourself in the foot, so any message sent to hASh, Hash, HASH, haSh, etc. will all get forwarded straight to `hash`. This is the only exception to the rule!

If a "remove\_agent" message gets sent without a specified agent\_id, then the agent\_id defaults to that of the sender. Of course, we suggest including setting the field as  `state.agent_id`  for readability but it can be used as shorthand.

### Creating Agents via Messages

Any agent can also create new agents. Again, any message sent to `hash`, but this time with the `create_agent` type, will tell Core to spawn a new agent. By default the agent will not have a position or direction. Core does not know where you want the agent to be placed and will not try to figure it out. 

Here, anything in the data field will be used to create the new agent. The `newborn` behavior is given to this agent, but remember, it will not be run until the next step.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
(state, context) => {
    state.messages.push({
        to: "hash",
        type: "create_agent",
        data: {
            parent: state.agent_id, 
            behaviors: ["newborn"]
        }
    });
    return state;
}
```
{% endtab %}

{% tab title="Python" %}
```python
def behavior(state, context):
  # we can use either a dict

  message =	{
    "to": "schelling",
    "type": "data_point",
    "data": {
      "num_agents": 50
    }
  }
  
  # or a class
  
  message = Message()
  
  state.messages.append(message)

  return state
```
{% endtab %}

{% tab title="Rust" %}
```

```
{% endtab %}
{% endtabs %}







