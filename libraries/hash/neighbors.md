# Neighbors

```javascript
/**
 * Returns all neighbors that share an agent's position
 * @param agentA
 * @param neighbors - context.neighbors() array or array of agents
 * */
neighborsOnPosition(agentA: PotentialAgent, neighbors: PotentialAgent[])

/**
 * Returns all neighbors within a certain vision radius of an agent.
 * Defaults vision max_radius to 1, min_radius to 0
 * Default is 2D (z_axis set to false), set z_axis to true for 3D positions
 * @param agentA
 * @param neighbors - context.neighbors() array or array of agents
 * @param max_radius - defaults to 1
 * @param min_radius - defaults to 0
 * @param z_axis - defaults to false
 */
neighborsInRadius(
  agentA: PotentialAgent, 
  neighbors: PotentialAgent[], max_radius = 1,
  min_radius = 0,
  z_axis = false
) 

/**
 * Searches and returns all neighbors whose positions are in front of an agent.
 * Default is set to planer calculations and will return all neighbors located
 * in front of the plane created by the agent's direction
 *
 * Colinear - If set to true, will return all neighbors on the same line as agent a.
 * @param agentA
 * @param neighbors - context.neighbors() array or array of agents
 * @param colinear - defaults to false
 */
neighborsInFront(
  agentA: PotentialAgent,
  neighbors: PotentialAgent[],
  colinear = false
)

/**
 * Searches and returns all neighbors whose positions are behind an agent.
 * Default is set to planer calculations and will return all neighbors located
 * behind the plane created by the agent's direction
 *
 * Colinear - If set to true, will return all neighbors on the same line as agent a.
 * @param agentA
 * @param neighbors - context.neighbors() array or array of agents
 * @param colinear - defaults to false
 */

neighborsBehind(
  agentA: PotentialAgent,
  neighbors: PotentialAgent[],
  colinear = false
)
```

