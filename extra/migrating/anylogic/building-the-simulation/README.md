# Building the Simulation

Working outside in, we will start with **Tankers**. We’ll point out key pieces of logic contained in each agent’s behaviors, show you some code snippets, and call out when we made use of published behaviors in hIndex. We encourage you to follow along in the [existing simulation](https://hash.ai/@hash/oil-supply-chain) to see how these snippets fit into the full behaviors and simulation.

To initialize our simulation, we'll define a **Creator** agent in our [initial state](../../../../anatomy-of-an-agent/initial-state.md) and begin crafting our agents inside it. Even though in our final simulation we'll have multiple of each type of agent, as we initially build our simulation with a single copy of each so that we can easily see that our agents are behaving correctly.

{% code title="init.json" %}
```javascript
[
    {
        "agent_name": "creator",
        "behaviors": [
            "initialize.py",
            "@hash/remove-self/remove_self.js"
        ]
    }
]
```
{% endcode %}

We'll import the `Remove Self` published behavior using the lower left sidebar, and create the initialize.js behavior in our simulation files sidebar \(top left\).

