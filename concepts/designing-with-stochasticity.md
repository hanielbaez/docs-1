# Designing with Stochasticity

Multi-Agent Simulation approaches problem-solving from a stochastic lens. Instead of quantitatively or numerically determining the solution to a set of equations representing a system, HASH will let you approximate messy, complex real-world systems, and empirically and statistically come to conclusions about how they behave. In order to better approximate the real world, you'll often want to initialize agents with heterogeneous properties, and ensure your behaviors properly randomize when necessary. Here are a number of ways you can incorporate stochasticity into your HASH simulations.

## Distributions

Initializing agent properties using different types of distributions is a common practice in Multi-Agent models. In HASH, you can use the jStats library or the NumPy package for sampling distributions. Here's an example that uses a number of these distributions to create agents: 

* We've sampled a Poisson distribution to determine how many new birds arrive at each step.
* We've sampled a uniform distribution to determine its `x` and `y` coordinates.
* We've sampled a triangular distribution to determine its altitude.
* We've sampled a normal distribution to determine its speed.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
function behavior(state, context) {
  const { poisson, uniform, triangular, normal } = hash_stdlib.stats;

  const num_new_agents = poisson.sample(10); // expected occurence rate

  for (let i = 0; i < num_new_agents; i++) {
    const x = uniform.sample(0, 10); // min, max
    const y = uniform.sample(0, 10);

    const altitude = triangular.sample(10, 10000, 500) // min, max, mode

    state.addMessage("hash", "create_agent", {
      "agent_name": "bird",
      "position": [x, y, altitude],
      "speed": normal.sample(25, 10), // mean, standard deviation
    })
  }
};
```
{% endtab %}

{% tab title="Python" %}
```python
from numpy.random import poisson, uniform, triangular, normal

def behavior(state, context):
  num_new_agents = poisson(1); # expected occurence rate

  for i in range(num_new_agents):
    x = uniform(0, 10); # min, max
    y = uniform(0, 10);

    altitude = triangular(10, 50, 200) # min, mode, max

    state.add_message("hash", "create_agent", {
      "agent_name": "bird",
      "position": [x, y, altitude],
      "speed": normal(25, 10), # mean, standard deviation
    })
```
{% endtab %}
{% endtabs %}

