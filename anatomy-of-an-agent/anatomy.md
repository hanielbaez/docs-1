# Anatomy

While an agent can store arbitrary data in its own state, some state values have special meaning in HASH. You're already familiar with a few:

```javascript
{ 
  agent_id: string // auto-created identifier. Agents receive messages to their ID.
  agent_name: string // optional identifier. Agent's receive messages to their name. 
  behaviors: [] // Filenames of the behaviors that the agent apply to advance their state every simulation step N to N+1. 
  messages: [] // Outbound messages from agent 
  position: [x, y, z] // display's agents in the viewer and used to calculate neighbors 
  search_radius: i64// context.neighbors() will return a list of all the agents within the search radius
}
```

There are a number of others which the simulation viewer looks at to try to infer meaningful ways to display the agent. Specifically:

* 
