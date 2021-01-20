---
description: 'Sweep parameters, explore a search space, and find optimal configurations'
---

# Experiments

In addition to running single simulation runs, HASH enables simultaneous simulation runs with different parameters.

In HASH, experiments are defined in the `experiments.json` file. Currently supported are:

* `linspace` - vary a single parameter within a range
* `arange` - vary a parameter based on an increment
* `values` - manually enter values for a specific parameter
* `monte-carlo` - generate random numbers according to a distribution
* `group` - group together multiple experiment types into a single experiment

For example, this `values` experiment will run seven experiments, setting a different value of \[0..6\] in the radius field in each one.{

{% tabs %}
{% tab title="experiments.json" %}
```javascript
{
    "Sweep values": {
        "type": "values",
        "field": "radius",
        "values": [0,1,2,3,4,5,6],
        "steps": 100    
    }
}
```
{% endtab %}
{% endtabs %}

To run an experiment, click the "Experiment Runner" button in the runner controls, denoted with a beaker icon. The option "Sweep Values" will be available in the selector. 

![Sweep values demonstration](../.gitbook/assets/image%20%2816%29.png)

{% hint style="success" %}
If you want to run an experiment from another users published simulation - for instance an example simulation - first save a copy to your drive and then click the experiment runner.
{% endhint %}

Running this experiment will generate 7 new simulations, each with a slightly different globals.json. If we run the simulation, we can see exactly which parameters get changed in the sidebar:

![Radius options](../.gitbook/assets/image%20%2817%29.png)

To access the changed varied parameter from within the simulation, we simply access that parameter in globals:

```javascript
const behavior = (state, context) => {
  const { radius } = context.globals();
};
```

Now, any behaviors that rely on the "radius" parameter from globals will use the corresponding value.

{% hint style="info" %}
You can run experiments locally or in [hCloud](../h.cloud.md)
{% endhint %}



