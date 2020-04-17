---
description: Become a Pro with the Properties tab
---

# Basic Properties

The properties tab is the place to define global characteristics for our simulation. The simulation properties are immutable while the simulation is running and are accessible to all agents simultaneously. Accessing the properties of the simulation is as simple as using the keyword `properties` in our agent behavior. To change properties while the simulation is running, make sure to pause the simulation, make the appropriate changes, and resume.

If, for example, we wanted to cap the height of all trees in a forest simulation, simply check the properties object for our globally-defined `"maxTreeHeight"` attribute.

The properties tab would look something like:

```javascript
{
    "maxTreeHeight":50,
    ...
}
```

The associated tree growth behavior would follow:

```javascript
(state, properties) => {
    ...
    if (state.height + growth <= properties['maxTreeHeight']) {
        growtree()
    }
    ...
}
```

Interestingly, the properties tab is actually interpreted as JavaScript which allows the use functional programming, variables, and constant definitions. For instance, if we wanted the simulation to start out with even numbers of tree and fire agents, we could drive our parameters with a variable and a function. An advanced use case of using JavaScript functionality to drive simulation parameters would be sweeping set of parameters or loading data in from an external source.

```javascript
function numAgents(){
    return 20
}

const numTrees = numAgents();
const numCows = numAgents();

{
    "initialTrees": numTrees,
    "initialCows": numCows,
    ...
}
```

