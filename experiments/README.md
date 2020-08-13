---
description: 'Sweep parameters, explore a search space, and find optimal configurations'
---

# Experiments

In addition to running single simulation runs, HASH enables simulataneous simulation runs with different parameters. In HASH, experiments are defined in the `experiments.json` file. Currently supported are:

* `linspace` - vary a single parameter within a range
* `arange` - vary a parameter based on an increment
* `values` - manually enter values for a specific parameter
* `monte-carlo` - generate random numbers according to a distribution
* `group` - group together multiple experiment types into a single experiment

Here's an example for the `values` definition:

{% code title="experiments.json" %}
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
{% endcode %}

This definition will now show up in the "Experiment Runner" button in the runner controls, denoted with a beaker icon. If there are no parsing errors, a new option to "Sweep Values" will be present in the selector. Here, we've named a custom experiment "Sweep Values"

![Sweep values demonstration](../.gitbook/assets/image%20%2816%29.png)

Running this experiment will generate 7 new simulations, each with a slightly different globals.json. If we run the simulation, we can see exactly which parameters get changed in the sidebar:

![Radius options](../.gitbook/assets/image%20%2817%29.png)

To access the changed varied parameter from within the simulation, we simply access that parameter in globals:

```javascript
const behavior = (state, context) => {
  const { radius } = context.globals();
};
```

Now, any behaviors that rely on the "radius" parameter from globals will use the corresponding value.



