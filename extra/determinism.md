# Determinism

## Why is determinism important?

Stochasticity is a key element of many multi-agent models. Introducing randomness allows you to explore the full possibility space you're modeling and account for uncertainty. However, in many situations you'll want to reproduce identical results and model states across a given set of parameters. **Deterministic** **simulations**, where the same inputs get you the same outputs, are an important feature of HASH.

HASH is designed to execute deterministically; however, there can be situations where simulation runs are different even with the same parameters. This usually happens because of:

1. randomness included within the simulation logic;
2. reliance on external APIs/data sources which can't always be guaranteed to return the same results \(for example, the most efficient public transportation route between two destinations\); or
3. the manner in which the hEngine builds [context](../anatomy-of-an-agent/context.md) for an agent.

### 1. Simulation Logic

Often you'll create simulations with probabilities, where an agent takes an action X% of the time. This might be by drawing random numbers using the [`hash_stdlib.random()`](../libraries/hash/random.md#random) function

{% tabs %}
{% tab title="JavaScript" %}
```javascript
state.foo = hash_stdlib.random() > 0.5 ? true : false
```
{% endtab %}
{% endtabs %}

or by using one of the [Statistic libraries](../libraries/#hash-standard-library) included in HASH, such as those outlined in [Designing with Distributions](../concepts/designing-with-distributions.md).

{% tabs %}
{% tab title="JavaScript" %}
```javascript
  const { poisson, uniform, triangular, normal } = hash_stdlib.stats;
  ...
  
  let foo = triangular.sample(0, 100, 50)
```
{% endtab %}
{% endtabs %}

To ensure the same numbers are generated every execution run, you can **set the seed** of the random number generator.

1. Call `hash_stdlib.setSeed(str: s)` to set a seed at the beginning of your simulation run. 
2. Use `hash_stdlib` functions for stochasticity. They'll return the same random number every simulation run.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
if (state.timestep == 1) {
  // If it's the first timestep, set the initial seed of the sim.
  hash_stdlib.setSeed("initial_seed");
}

// will return the same sample every run
const num_new_agents = poisson.sample(10); 
```
{% endtab %}
{% endtabs %}

{% embed url="https://hash.ai/@hash/determinism" caption="An example simulation demonstrating Determinism" %}

HASH standard library functions for distributions and random number generation are currently only available in JavaScript; [Python supports setting a seed natively](https://docs.python.org/3/library/random.html) and, if creating a Python simulation, you should consider using `random.seed()` and/or [numpy](../libraries/python-packages.md).

{% hint style="warning" %}
hEngine execution order can, at times, cause different agents to execute in different orders, causing different random numbers to be pulled between runs. We'll be building in guarantees around this in an upcoming update.
{% endhint %}

### 2. Reliance on External APIs

Currently, we only make the Mapbox API directly accessible from within both hCore and hCloud. For this and other future hIndex-listed APIs and external integrations we intend to provide labeling regarding which services claim to support reproducibility. When using hEngine users will have to track and manage these conditions themselves.

### 3. Context

HASH's [actor model](https://hash.ai/wiki/actor-model) lets the engine execute agents in parallel. This allows us to run models across machines and ensure that they don't fall prey to sequencing problems, where the execution order of agents generates artifacts in your simulations. The current release of hEngine does **not** guarantee ordering of the agent interaction methods provided on the context of an agent.

#### Neighbors

Once all agents have computed their states for the next time step, hEngine calculates an agent's neighbors and delivers them to each agent's `context`. Neighbors are not calculated deterministically. This means that even in a simulation with identical initialization, globals, and behaviors, an agent will not necessarily see its neighbors in the same order every time. On the other hand, hEngine does not guarantee randomization either. 

#### Messages

hEngine delivers messages once all agents have computed their next states. In current releases of HASH, messages are not collected or delivered deterministically. That means there is no guarantee that two identical agents in an otherwise identical simulation state will have inboxes that are sorted the same way.

Like with neighbors, randomization is not guaranteed. Shuffling messages before you access them ensures that your simulation is free of artifacts.

{% hint style="info" %}
If your simulation relies upon the order of the messages and neighbors array, consider adding a sorting function to the simulation that will return the agents in a predictable manner. For example, sort based on an `agent_name` field you specify in the initialization of the simulation.
{% endhint %}



