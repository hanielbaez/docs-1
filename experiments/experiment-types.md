---
description: Reference for different experiment options
---

# Experiment Types

### Value sweeping

Value sweeping runs a simulation for each specified value.

```javascript
"Radius values": {
  "steps": 100,
  "type": "values",
  "field": "radius",
  "values": [0, 1, 2, 3, 4, 5, 6, 7]
}
```

Value sweeping is particularly is useful for multi-parameter sweeps and categorical sampling.

### Linspace sweeping

Linspace is one of the most common types of parameter sweeping. Define start, stop, and number of samples to generate an even sampling between two numbers with a set number of datapoints.

```javascript
"Radius linspace": {
  "steps": 100,
  "type": "linspace",
  "field": "radius",
  "start": 0,
  "stop": 10,
  "samples": 11
}
```

### Arange sweeping

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

You can run groups of experiments together by adding experiment keys to the `runs` array of a `group` definition.

```javascript
"Sweep radius": {
  "steps": 100,
  "type": "group",
  "runs": ["Radius values", "Radius linspace", "Radius arange", "Radius monte"]
}
```



