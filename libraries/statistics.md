# Statistics

All statistics functions are available through the stats package, `hash_stdlib.stats`

HASH uses the [jStat](http://jstat.github.io/distributions.html) library to provide the following functions. Currently only the functions and classes listed below are "fully supported". Others are available and will work, though we've left them undocumented as the interface/names might change.

### Vectors

[jStat documentation](http://jstat.github.io/vector.html)

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

### Distributions

[jStat documentation](http://jstat.github.io/distributions.html)

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

Here's an example that uses a number of these distributions. This behavior creates a bird agent with the following attributes: 

* We've sampled a Poisson distribution to determine how many new birds arrive at each step.
* We've sampled a uniform distribution to determine its `x` and `y` coordinates.
* We've sampled a triangular distribution to determine its altitude.
* We've sampled a normal distribution to determine its speed.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
function behavior(state, context) {
  const poisson = hash_stdlib.stats.poisson;
  const uniform = hash_stdlib.stats.uniform;
  const triangular = hash_stdlib.stats.triangular;
  const normal = hash_stdlib.stats.normal;

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
```

```
{% endtab %}
{% endtabs %}

