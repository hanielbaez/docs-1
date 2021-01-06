# Sending Messages

Agents send messages by adding JSON objects to their `messages` array. The object is defined as:

```javascript
{
    to: "string" //an agent_id or agent_name
    type: "string" //a string that defines the type of the message
    data: {} // an object that serves as the payload 
}
```

* **to:** An agent id or an agent name. The agent with the corresponding `agent_id` field or the agent\(s\) with the corresponding `agent_name` field will receive the message in the next time step.

{% hint style="info" %}
You can send a message to multiple agents - multicasting - by sending a message to an agent\_name that is shared by multiple agents. For instance, if five agents have the field `agent_name` set to`"forklift"`, they will all receive a message defined with `to: "forklift"`
{% endhint %}

* **type:** A user defined string that serves as metadata for the object, describing the message and/or its purpose. 
* **data:** A user defined object that can contain any arbitrary data.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const behavior = (state, context) => {
    let messages = state.get("messages");
    
    messages.push({
        to: "people", 
        type: "greeting", 
        data: {"msg": "hello"}
    });
    
    state.set("messages", messages);
}
```
{% endtab %}

{% tab title="Python" %}
```python
messages = state.get("messages")
messages.append({"to": "people", "type": "greeting", "data": {"msg": "hello"}})
state.set("messages", messages)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
We provide helper functions on the state object for adding messages. state.addMessage\(\) and state.add\_message\(\), for JavaScript and Python, respectively.
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
state.addMessage("people", "greeting", {"msg": "hello"})
```
{% endtab %}

{% tab title="Python" %}
```python
state.add_message("people", "greeting", {"msg": "hello"})
```
{% endtab %}
{% endtabs %}

