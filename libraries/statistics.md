# Statistics

All statistics functions are available through the stats package, `hash_stdlib.stats`

HASH uses the [jStat](http://jstat.github.io/distributions.html) library to provide the following functions. Currently only the functions listed below are "fully supported". Other functions are available and will work, though we've left them undocumented as the interface/names might change.

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



