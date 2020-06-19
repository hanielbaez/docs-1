# Anatomy

While an agent can store arbitrary data in its own state, some state values have special meaning in HASH. You're already familiar with a few:

```javascript
{ 
  agent_id: string // auto-created identifier. Agents receive messages to their ID.
  agent_name: string // optional identifier. Agent's receive messages to their name. 
  behaviors: [] // Filenames of the behaviors that the agent apply to advance their state every simulation step N to N+1. 
  messages: [] // Outbound messages from agent 
  position: [x, y, z]// display's agents in the viewer and used to calculate neighbors 
}
```

There are a number of others which the simulation viewer looks at to try to infer meaningful ways to display the agent. Specifically:

* `shape [string]`: if provided overrides the default "box" visualization for the agent. 

  * Shape options are "box", "cone", "cylinder", "dodecahedron", "icosahedron", "octahedron", "plane", "sphere", "tetrahedron", "torus", or "torusknot".

* `height [i64]`: if provided \(and agent is rendering on a 2d grid\), will set the display height of the agent.

* `position [i64, i64, i64]`: if provided, will render the agent in 3d space \(and override the grid position\)

* `direction [i64, i64, i64]`: if provided, will render the agent as a cone oriented along that unit vector

* `color [string]`: if provided, will color the agent.  Named colors \("red", "green", "blue", etc.\) are supported, alongside hex color codes \(`#223344`\) and RGB values \(`rgb(12,244,155)`\). 
* `search_radius [i64]`: context.neighbors\(\) will return a list of all the agents within the search radius.

