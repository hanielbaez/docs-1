# Getting the Behavior Index

Sometimes behaviors may need to know which behaviors executed before it, and which behaviors will execute after it, in the agent's behavior chain. For this, the agent's `state` has a special method — `state.behaviorIndex()` in Javascript and `state.behavior_index()` in Python — which returns the index of the currently executing behavior in the agent's behavior chain.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const behavior = (state, context) => {
    const index = state.behaviorIndex();
    console.log("behavior index =", index);
    console.log("behavior name =", state.behaviors[index]);
}
```
{% endtab %}

{% tab title="Python" %}
```python
def behavior(state, context):
    index = state.behavior_index()
    print("behavior index =", index)
    print("behavior name =", state.behaviors[index])
```
{% endtab %}
{% endtabs %}

