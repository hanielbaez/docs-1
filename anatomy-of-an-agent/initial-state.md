---
description: Where simulated life begins
---

# Initial State

HASH simulations start life in the `initialState` file in your editor. In here we generate the initial conditions, or _starting state_ of the simulated world. There are two ways to populate this. You can:

1. Explicitly define the agents that will be in your model; or
2. You can define "creator" agents with behaviors that will generate more complex initial states. You can do this with published behaviors, or create your own.

Here's what explicitly defining your agents might look like:

![A simple set of agents ](../.gitbook/assets/image%20%286%29.png)

With "creator" agents you can initialize more interesting models. By accessing published behaviors, we can very easily generate common agent placements. These behaviors can be found in the lower left corner; click on them to add them to your simulation:

* `Create Grids`:  copy an agent to every unit within the topology bounds
* `Create Scatters`: copy an agent to random locations within the topology bounds 
* `Create Stacks`: copy an agent multiple times to the same location

Take a look at how we can use published behaviors in the following example, where [rabbits forage for food and reproduce](https://core.hash.ai/simulation/5e7d1664d945ef290d54be43/rabbits-grass-weeds), while grass and weeds grow around them. :

![](../.gitbook/assets/image%20%285%29.png)

You can see that we've added a few behaviors to our "creator" agent, and templates as well.  

`Create Grids` looks at the agent templates in the "grid\_templates" array, in this case the "ground". We're copying it to fill the space defined in the bounds of our "topology" field in`properties`:

![](../.gitbook/assets/image%20%289%29.png)

Then `Create Scatters` distributes the "rabbits" across the topology. Each one is placed in a random location.

Now we want to make a few adjustments to the agents we've generated which require a bit more logic. Luckily we can make use of the composable nature of HASH behaviors. `Create Grids` and `Create Scatters` has created  an "agents" object in our creator and filled it. We can access those agents by using the "template\_name" as a key: 

![](../.gitbook/assets/image%20%284%29.png)

Here we've randomly assigned the color of our "ground" agents, and given each of the "rabbits" a random starting amount of energy.

Our creator then runs two more published behaviors. `Create Agents` sends messages to the engine to generate every agent in the "agents" object, and `Remove Self`  gets rid of the "creator" agent, since it's finished all it needs to do. Again, these behaviors can be found in the lower left sidebar.

{% hint style="info" %}
You can create new agents during your simulation by sending a message to the reserved hash keyword.
{% endhint %}

```javascript
state.messages.push({
  to: "hash",
  type: "create_agent",
  data: { ...agent_details }
 })

```

If you'd like to explore another simple example that uses these published behaviors, take a look at the [Forest Fires model](https://core.hash.ai/simulation/5e7a36d4d945ef56af54bd3a), or the [Rock,  Paper, Scissors model](https://core.hash.ai/simulation/5e7a44d2d945ef3e5d54bd55).

{% hint style="info" %}
If you ever feel like you might be "re-inventing the wheel," make sure to check out [HASH Index](https://hash.ai/index/search?contentType=Behavior&sort=relevance&query=create&page=1) in case someone else has shared a similar behavior.
{% endhint %}

