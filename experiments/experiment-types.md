---
description: Reference for different experiment options
---

# Experiment Types

### Value Sweeping

Value sweeping runs a simulation for each of the specified `values`. The `field` in **globals.json** is populated with the value for each run.

```javascript
"Radius values": {
  "steps": 100,
  "type": "values",
  "field": "radius",
  "values": [0, 1, 2, 3, 4, 5, 6, 7]
}
```

{% hint style="info" %}
Value sweeping is particularly is useful for multi-parameter sweeps and categorical sampling.
{% endhint %}

### Fixed Sample Sweeping \(linspace\)

Fixed sample sweeping or 'linspace' is one of the most common types of parameter sweeps. Define `start`, `stop`, and number of `samples` to generate an even sampling between two values with a set number of data points.

```javascript
"Radius fixed sample": {
  "steps": 100,
  "type": "linspace",
  "field": "radius",
  "start": 0,
  "stop": 10,
  "samples": 11
}
```

### Fixed Step Sweeping

Instead of using a set number of samples like linspace, arange samples every "increment" between the specified start and stop fields.

```javascript
"Radius arange": {
  "steps": 100,
  "type": "arange",
  "field": "radius",
  "start": 0,
  "stop": 10,
  "increment": 0.5
}
```

### Monte Carlo sweeping

Monte Carlo sweeping allows random sampling from a custom distribution. Each supported distribution can be customized through the associated parameters. Each parameter defaults to 1 if not defined.

```javascript
"Radius monte": {
  "steps": 100,
  "type": "monte-carlo",
  "field": "radius",
  "samples": 10,
  
  // Either combination of distributions and parameters:
  "distribution": "normal",
  "mean": 1,
  "std": 1
  
  // or
  "distribution": "log-normal",
  "mu": 1,
  "sigma": 1
  
  // or  
  "distribution": "poisson",
  "rate": 1
  
  // or  
  "distribution": "beta",
  "alpha": 1,
  "beta": 1
  
  // or  
  "distribution": "gamma",
  "shape": 1,
  "scale": 1
}
```

### Groups

You can run groups of experiments together by adding experiment keys to the `runs` array of a `group` definition. The below code, for example, would execute each of our experiments outlined above as sub-experiments of a new experiment:

```javascript
"Group Sweep": {
  "steps": 100,
  "type": "group",
  "runs": ["Radius values", "Radius linspace", "Radius arange", "Radius monte"]
}
```

### Multiparameter

In order to discover interaction effects in your model, you'll have to perform sweeps over multiple parameters. The multiparameter experiment generates a full factorial design with all of the experiments defined in `runs`.

```javascript
"Full factorial sweep": {
  "steps": 100,
  "type": "multiparameter",
  "runs": [
    "Radius values", 
    "Radius linspace", 
    "Radius arange", 
    "Radius monte"
  ]
}
```

### Optimizations

{% hint style="info" %}
**Optimization experiments are coming soon.** If you're interested in receiving early access, [contact us](https://hash.ai/contact) and let us know what you can about your use case.
{% endhint %}

