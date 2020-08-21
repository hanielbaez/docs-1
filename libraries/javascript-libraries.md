# Javascript Libraries

HASH currently provides access to the [jStat](http://jstat.github.io/distributions.html) library for accessing statistics classes and functions. You can access the library through `hash_stdlib.stats` .

Currently only the functions and classes listed below are "fully supported". Others are available and will work, though we've left them undocumented as the interface/names might change.

### [jStat Vectors](http://jstat.github.io/vector.html)

```javascript
sum()
sumsqrd()
sumsqerr()
sumrow()
product()
min()
max()
mean()
meansqerr()
geomean()
median()
cumsum()
cumprod()
diff()
rank()
mode()
range()
variance()
pooledvariance()
deviation()
stdev()
pooledstdev()
meandev()
meddev()
skewness()
kurtosis()
coeffvar()
quartiles()
quantiles()
percentile()
percentileOfScore()
histogram()
covariance()
corrcoeff()
```

### [jStat Distributions](http://jstat.github.io/distributions.html)

```javascript
beta(alpha, beta)
centralF(df1, df2)
cauchy(local, scale)
chisquare(dof)
exponential(rate)
gamma(shape, scale)
invgamma(shape, scale)
kumaraswamy(alpha, beta)
lognormal(mu, sigma)
normal(mean, std)
pareto(scale, shape)
studentt(dof) 
tukey(nmeans, dof)
weibull(scale, shape)
uniform(a,b)
binomial
negbin
hypgeom
poisson
triangular
arcsine(a,b)
```

Here's an example that uses a number of these distributions to create bird agents: 

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
from scipy.stats import uniform, triang, norm, poisson

def behavior(state, context):
  num_new_agents = poisson.rvs(10); # expected occurence rate

  for i in xrange(num_new_agents):
    x = uniform.rvs(0, 10); # min, max
    y = uniform.rvs(0, 10);

    altitude = triang.rvs(10, 10000, 500) # min, max, mode

    state.add_message("hash", "create_agent", {
      "agent_name": "bird",
      "position": [x, y, altitude],
      "speed": norm.rvs(25, 10), # mean, standard deviation
    })
```
{% endtab %}
{% endtabs %}

