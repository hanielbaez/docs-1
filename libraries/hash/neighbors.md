# Neighbors

### neighborsOnPosition\(agentA, neighbors\)

This function returns all neighbors that share the same position as `agentA`. The current agent's `state` can be passed in as `agentA`, and dummy agents can be used as well.

```javascript
function behavior(state, context) {
    // Find neighbors on my position
    const neighbors = context.neighbors();
    const cohabitators = hash_stdlib.neighborsOnPosition(state, neighbors);
    
    // Create an adjacent "agent"
    let adjPos = state.position;
    adjPos[0] + 1;
    const adjAgent = { "position": adjPos };
    
    // Find the agents adjacent to me
    const adjacents = hash_stdlib.neighborsOnPosition({ "position": adjPos }, neighbors);
    
    ...
}
```

### neighborsInRadius\(agentA, neighbors, max\_radius, min\_radius, z\_axis\)

This function returns all neighbors within the specified radii. The current agent's `state` can be passed in as `agentA`, and dummy agents can be used as well. By default the max and min radii are \[1, 0\]. `z_axis` is false by default, but by passing true you can enable search in a spherical, as opposed to circular radius.

```javascript
function behavior(state, context) {
    // Count the number of electrons close to me
    const electrons = context.neighbors().filter(n => n.agent_type === "electron");   
    const close_electrons = neighborsInRadius(state, electrons, 2, 0, true).length;

    ...
}
```

### neighborsInFront\(agentA, neighbors, colinear\)

This function returns all neighbors located in front of an agent. `agentA` must have a "direction" property since "in front" is determined by the plane perpendicular to that vector. `colinear` defaults to false, but by passing true the function will only return agents on the same _line_ as the "direction". The current agent's `state` can be passed in as `agentA`, and dummy agents can be used as well.

```javascript
function behavior(state, context) {
    const neighbors = context.neighbors();
    
    // Check which of my neighbors I can see "in front" of me
    const visibleAgents = hash_stdlib.neighborsInFront(state, neighbors);
    
    // Check which agents are in front of one of my neighbors
    const neighborFront = hash_stdlib.neighborsInFront(neighbors[0], neighbors);
    
    ...
}
```

### neighborsBehind\(agentA, neighbors, colinear\)

This function returns all neighbors located behind the agent. It functions identically to **neighborsInFront** _****_but returns agents behind the plane of `agentA`. The current agent's `state` can be passed in as `agentA`, and dummy agents can be used as well.

```javascript
function behavior(state, context) {
    const neighbors = context.neighbors();
    
    // Check which of my neighbors are tailing me
    const tailingAgents = hash_stdlib.neighborsBehind(state, neighbors, true);
    
    // Check if my neighbor is being tailed
    const neighborTail = hash_stdlib.neighborsBehind(neighbors[0], neighbors, true);
    const neighborTailed = neighborTail.length > 0;
    
    ...
}
```

