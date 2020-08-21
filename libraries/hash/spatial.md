# Spatial

```javascript
/**
 * Returns the specified distance between two agents.
 *  distance is one of the four distance functions supported by HASH,
 *  defaults to manhattan distance.
 * @param a
 * @param b
 * @param distance
 */
distanceBetween(agent, agent, distanceFunction = "manhattan")


/** Returns the unit vector of a vector
* @param vec an array of numbers
*/
normalizeVector(vec: number[])

/**
 * Returns a position array of x,y,z is set to true
 * @param topology the Context.globals().topology object
 * @param z_plane defaults to false
 */
randomPosition(topology: topology object, z_plane = false)
```

