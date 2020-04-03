# Built-in behaviors

The authoring of Rust behaviors in-browser is coming soon. In the meantime, we've compiled a handful of useful Rust behaviors for you which span a variety of topics, including common utilities and a number of more sophisticated behaviors such as diffusion.

**Random\_movement**

This behavior causes an agent to move by a random amount defined in `random_movement_step_size` \(default 1\). If `random_movement_seek_min_neighbors` or `random_movement_seek_max_neighbors` is defined, then the agent will stop moving if it has an amount of neighbors falling in the specified range.

```text
// This agent will move 0 or 3 spaces up/down and left/right, until it has at least 5 neighbors
const attentionSeekingAgent = {
    behaviors: ["random_movement"],
    position: [6, 3],
    random_movement_step_size: 3,
    random_movement_seek_min_neighbors: 5
}
```

**Random\_away\_movement**

This behavior causes an agent to choose a random neighbor each time step and move away from them. If an agent has no neighbors it does not move.

```text
// This agent will keep moving away from its neighbors until it has none.
const lonerAgent = {
    position: [6, 3],
    behaviors: ["random_away_movement"]
}
```

**Move\_in\_direction**

This behavior causes an agent to move in the direction specified by the `direction` vector. The magnitude of the vector affects the distance the agent moves.

```text
// This agent will keep moving "south" until `direction` is modified
const migratingAgent = {
    position: [6, 3],
    behaviors: ["move_in_direction"],
    direction: [0, -1]
}
```

**Age**

This behavior causes an agent’s `age` property to increment by 1 each time step of the model. If the agent is not initialized with the `age` property, it will default to 1 after the agent first runs the behavior \(essentially having been initialized at 0\).

```text
// This agent's age starts at 0 and will increment by 1 each time step 
const newbornAgent = {
    behaviors: ["age"],
    age: 0,
}
```

**Diffusion**

This behavior diffuses the value\(s\) of the agent properties specified in the `diffusion_targets` array. Every step, the value of each property is updated based on the total difference between its neighbors and itself. The `diffusion_coef` property will determine the rate at which diffusion occurs, with the default, 0.5, meaning that half the total difference is added each step.

```text
// With a behavior that updates its color based on the red, green, and blue properties, this agent will slowly 'absorb' the color from its neighbors
const colorfulAgent = {
    behaviors: ["diffusion"],
    diffusion_targets: ["],
    diffusion_coef: 0.3
}
```

**Decay**

This behavior causes an agent to randomly set its `decayed` property to `true`. At every time step this may occur with a likelihood of `decay_chance`. If it is not specified then `decay_chance` defaults to 0.5.

```text
// Each time step, this agent has a 10% chance of decaying
const decayingAgent = {
    behaviors: ["decay"],
    decayed: false,
    decay_chance: 0.1
}
```

**Orient\_toward\_value**

This behavior causes an agent to update its direction vector based on the `orient_toward_value` property of its neighbors. If `orient_toward_value_uphill` is `true` then the agent will orient towards the position of the neighbor with the maximum value, and vice vice versa. The default state is `true`. If `orient_toward_value_cumulative` is true, then the behavior will orient towards the _location_ with the max/min value by summing the values of neighbors which share the same position.

```text
// This agent will climb "uphill". Each time step it first faces its tallest neighbor, and then moves towards it
const mountainClimber = {
    behaviors: ["orient_toward_value", "move_in_direction"],
    orient_toward_value_uphill: true,
    orient_toward_value: ["height"]
}
```

**Reproduce**

This behavior causes an agent to produce a new child agent with the properties defined in the `reproduction_child_values` property. The agent will produce `reproduction_rate` children each time step \(defaulting to 1\).

```text
// This rabbit agent creates a new juvenile rabbit each time step
const matureRabbit = {
    behaviors: ["mature_rabbit", "reproduce"],
    height: 2,
    reproduction_child_values: {
        behaviors: ["juvenile_rabbit"],
        height: 1,
    }
}
```

**Counter**

This behavior is essentially a cyclical version of the **Aging** behavior. Every step of the simulation, the agent’s `counter` property is incremented by the `counter_increment` property \(the default is 1\). When `counter` is equal to `counter_reset_at`, it is set to the value specified in `counter_reset_to`.

If `counter` is not specified at initialization, it defaults to 0. If either `counter_reset_at` or `counter_reset_to` are not specified, then no resetting occurs.

```text
// This agent will count from 0 to 2 and back to 0 during the simulation. With a %, you can cause certain behaviors to trigger at regular intervals
const countingAgent = {
    behaviors: ["counter"],
    counter: 0,
    counter_increment: 1,
    counter_reset_at: 3,
    counter_reset_to: 0
}
```

**Conway**

This behavior causes an agent to follow the Game of Life rules created by John Conway. Agents are either alive or dead, as specified by the boolean `alive` property. A live agent remains alive if it has 2 or 3 neighbors, and a dead agent comes to life if it has exactly 3 neighbors.

```text
// This agent will start out alive and change state based on its neighbors
const conwayAgent = {
    behaviors: ["conway"],
    position: [6, 3],
    alive: true
})
```

