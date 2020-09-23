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

For an example that uses these statistics functions, see the example on the [Designing with Stochasticity](../concepts/designing-with-distributions.md) page.

