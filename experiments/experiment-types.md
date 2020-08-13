---
description: Reference for different experiment options
---

# Experiment Types

### Value sweeping

For simulations with different categorical behavior or a very specific sampling of datapoints, it's possible to set custom values. This unlocks multi-parameter sweep and categorical sampling that are not accurately described by the pre-defined sampling methods defined below.

```javascript
"Radius values": {
  "steps": 100,
  "type": "values",
  "field": "radius",
  "values": [0, 1, 2, 3, 4, 5, 6, 7]
}
```

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

Arange if also another one of the most common types of parameter sweeping. Instead of using a number of samples like linspace, arange depends on the "increment" parameter, and will sample every increment between the specified start and stop fields.

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

### Monte carlo sweeping

Monte carlo sweeping allows random sampling from a custom distribution. Each supported distribution can be customized by assigning various associated parameters. Each parameter is defaulted to 1 if not present in the definition

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

With HASH, it's also possible to run groups of experiments together simply by placing their name in the `runs` array of the `group` definition.

```javascript
"Sweep radius": {
  "steps": 100,
  "type": "group",
  "runs": ["Radius values", "Radius linspace", "Radius arange", "Radius monte"]
}
```



