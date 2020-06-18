# Globals

Global variables are defined in the `globals.json` file present within every simulation. These variables are immutable while the simulation is running and are accessible to all agents simultaneously.

Accessing the properties of the simulation is as simple as using the keyword `properties` in our agent behavior.

To change properties while the simulation is running, make sure to pause the simulation, make the appropriate changes, and resume.

If, for example, we wanted to cap the height of all trees in a [forest simulation](https://hash.ai/index/5e065650196c3fbd41d8bd43/forest), we might introduce the global variable `"maxTreeHeight"`. The `globals.json` file would contain something like:

```javascript
{
    "maxTreeHeight": 50,
    ...
}
```

The associated tree growth behavior would follow:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
function behavior(state, context) {
    ...
    if (state.height + growth <= context.globals()['maxTreeHeight']) {
        growtree()
    }
    ...
}
```
{% endtab %}

{% tab title="Python" %}
```python
def behavior(state, context):
    ...    
    if state.height + growth <= context.globals()['maxTreeHeight']):
        growtree()
    ...

```
{% endtab %}
{% endtabs %}



