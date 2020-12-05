# Messages

Information in a simulation can propagate in one of two ways - either through neighbors which we cover in [Behaviors](../behaviors/) and [Topology](../configuration/topology/), or through message passing. 

Messages are a simple yet powerful way of moving data around a simulation and can be used to do things like adding, removing, and collecting data from agents that are not necessarily near each other. 

Agents can create a message to send to either another agent or the simulation engine itself. Here, we send a message to an agent with the name of `schelling`:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const behavior = (state, context) => {
    let messages = state.get("messages")
    messages.push({
        to: "schelling",
        type: "data_point",
        data: {
            num_agents: 50
        }
    });
    
    state.set("messages", messages)
}
```
{% endtab %}

{% tab title="Python" %}
```python
def behavior(state, context):
  # we can use either a dict
  messages = state.get("messages")
  
  message =	{
    "to": "schelling",
    "type": "data_point",
    "data": {
      "num_agents": 50
    }
  }
  
  # or a class
  
  message = Message()
  
  messages.append(message)

  state.set("messages", messages)
  
  
#########################
class Message:
  to = "schelling"
  type = "data_point"
  data = { "num_agents": 50 }


```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you want to jump right into code you can take a look at our [Message Passing Colors ](https://hash.ai/index/5e1c9163fee0a34d2f9be2c6/message-passing-colors)simulation, which demos message passing among many agents.
{% endhint %}

You'll notice that each message is comprised of three fields: "to", "type", and "data." 

* `to`:  the name or ID of the agent that the message will be deliever to
* `type`: the type of message being sent for the message handler to select the right course of action
* `data`: any data you want to send along with the message

{% hint style="info" %}
You can use the helper function state.addMessage\(to&lt;String&gt;, type&lt;String&gt;, data&lt;Dict&gt;\) to add messages directly to the state messages field.
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const behavior = (state, context) => {
    state.addMessage("foo", "bar", {msg: "hello"})
}
```
{% endtab %}

{% tab title="Python" %}
```python
def behavior(state, context):
  state.add_message("foo", "bar", {msg: "hello"})
```
{% endtab %}
{% endtabs %}

Messages are produced during a step, but are not delivered and processed until the next step.

![Data flow for a single simulation step in HASH](../.gitbook/assets/image%20%2824%29.png)

Agents send messages by adding them to `state.messages`, and they receive them by checking `context.messages().` 

