# Spatial

### distanceBetween\(agentA, agentB, distanceFunction\)

This function returns the distance between two agents, using a specific distance function. You can pass the current agent's `state` as one of the agents. The different distance functions are:

* [Euclidean](https://en.wikipedia.org/wiki/Euclidean_distance) \(default\)
* [Manhattan](https://en.wikipedia.org/wiki/Taxicab_geometry)
* [Euclidean Squared](https://en.wikipedia.org/wiki/Euclidean_distance#Squared_Euclidean_distance)
* [Chebyshev](https://en.wikipedia.org/wiki/Chebyshev_distance)

```javascript
function behavior(state, context) {
    const { neighborA, neighborB } = context.neighbors();
    
    // Find the closest of 2 neighbors to you
    const distanceToA = hash_stdlib.distanceBetween(state, neighborA);
    const distanceToB = hash_stdlib.distanceBetween(state, neighborB);
    state.closest = distanceToB > distanceToA ? "A" : "B";
    
    // Check if neighbors are closer to each other than to you
    const neighborDistance = hash_stdlib.distanceBetween(neighborA, neighborB);
    const selfDistance = state.closest === "A" ? distanceToA : distanceToB;
    
    state.closer_to_neighbors = selfDistance < neighborDistance;
}
```

### normalizeVector\(vec\)

This function returns the unit vector of the `vec` array. You can use it to normalize an agent's direction vector after it's modified.

```javascript
function behavior(state, context) {
    const dir = state.direction;
    
    // Modify the direction by adding a vector [1, 2, 0]
    dir[0] += 1;
    dir[1] += 2;
    
    // Turn it back into a unit vector
    state.direction = hash_stdlib.normalizeVector(dir);
}
```

### randomPosition\(topology, z\_plane\)

This function returns a random integer position within the bounds of the `topology`. The Topology should be user-defined in **globals.json** \(see [Topology](../../configuration/topology/)\). By default`z_plane` is `false` and the returned position is in a 2D plane. Pass true to return a position in 3D space.

```javascript
function behavior(state, context) {
    // Move to a random position
    const topology = context.globals().topology;
    const new_pos = hash_stdlib.randomPosition(topology);
    
    state.position = new_pos;
}
```

