---
description: 'Get started creating agents in our Hello, HASH tutorial!'
---

# Hello, HASH!

## Getting started with HASH

We're going to create a basic agent based model where two agents will exchange pleasantries. 

To start, create a new simulation file from scratch by using the _File_ menu and clicking _New Simulation._

In your new simulation workspace, open the **init.json** file ****shown in the lefthand side panel. You'll see an empty pair of square brackets.

The **init.json** file defines the 'initial state' or starting point of a simulation as a collection of objects in a JSON array.

We'll begin by adding two agents into the array, and give them names.

```javascript
[ 
  { 
    "agent_name": "Alice" 
  },
  { 
    "agent_name": "Bob"
  } 
] 
```

_`"agent_name"` is a_ [_reserved keyword on agents_](https://docs.hash.ai/core/anatomy-of-an-agent) _that lets us reference a specific agent by a string._

Click the **Start Simulating** button beneath the workspace's right-hand view-pane, and then select the raw output tab in the viewer. Congratulations! You've given life to Alice and Bob. 

However, if you toggle back from the raw output view to the 3D viewer, you may notice... nothing at all. Alice and Bob haven't been rendered, because they haven't been given a position in space. Let's go ahead and fix that.

```javascript
[ 
  { 
    "agent_name": "Alice", 
    "position": [0,0] 
  },
  { 
    "agent_name": "Bob", 
    "position": [2,0] 
  }
] 
```

When you've finished adding positions to your agents, click **Reset Simulation**. You should now see two blobs in the 3D viewer representing our agents.

### Saying Hello

The agents are polite and would like to say hello to one another. We can give the agents "greeting" [behaviors](https://docs.hash.ai/core/behaviors). In initialState.json add file names into each of the behavior arrays.

```javascript
[
  { 
    "agent_name": "Bob", 
    "behaviors": ["hello_bob.js"],
    "position": [0,0] 
  }, 
  { 
    "agent_name": "Alice", 
    "behaviors": ["hello_alice.js"],
    "position": [2,0] 
  }
] 
```

Then create the files by clicking the new file button in the left hand sidebar.

![](../.gitbook/assets/screen-shot-2020-04-16-at-7.51.31-am.png)

In hello\_bob.js, we want to send a message **from** Bob **to** Alice. 

HASH has built in support for message passing. Push a message object to an agents message array, and it will deliver the message to the target agent in the next timestep. We need to add three things to our message.

* to: agent\_id or agent\_name
* type: a string that identifies the type of message that is being sent
* data: A JSON object containing the message data

Since we're only sending a message to one agent, Alice, we can use the agent\_name in the to field, and let's call the type of message a "greeting". And in our data packet, we can write a nice greeting.

```javascript
(state, context) => {
state.messages.push({
    to: "Alice",
    type: "greeting",
    data: {
      msg: "Hello, Alice."
    }
  })
  return state;
}
```

Run the simulation. You won't see anything in the 3d viewer, but if you click Raw Output you'll see our Bob agent now has an array of messages with one message to Alice._The Bob agent is sending the same message every timestep._ 

In our hello\_alice.js function, we want to receive the messages and do something when we get it.

When an agent receives a message addressed to them, it's stored in their "context", under context.messages. The agent can iterate through the messages array and act on specific messages. Let's find all of the messages that are greetings.

```javascript
const greetings = context.messages.filter(msg => msg.type == "greeting");
if (greetings.length > 0) {
    //do something
}
```

Adding visual indicators of state changes is an easy way to communicate what's happening in your simulation. Change Alice's color when they receive a greeting. 

```javascript
(state, context) => {
  const greetings = context.messages.filter(msg => msg.type == "greeting");
  if (greetings.length > 0) {
    state.color = "blue";
  }
  return state;
}
```

Now reset your simulation and run it. On the second timestep the Alice agent will change color to blue. Neat! 

To respond to Bob, we can send a message back addressed to the first message sender, with an appropriate response.

```javascript
(state, context) => {
  const greetings = context.messages.filter(msg => msg.type == "greeting");
  if (greetings.length > 0) {
    state.color = "blue";
    greetings.forEach(m => {
    state.messages.push({
        to: m.from,
        type: "greeting",
        data: {
          msg: "Go away, Bob! I’m social-distancing"
        }
      })
  })
  }
  return state;
}
```

We're iterating through the filtered messages and pushing to Alice's message array a new message, addressed to whichever agent sent the message, greeting them back.

Back in hello\_bob.js, we can add a similar message handler for Bob. 

```javascript
(state, context) => {
  const greetings = context.messages.filter(msg => msg.type == "greeting");
  if (greetings.length > 0){
    state.color = "red"
  }
  state.messages.push({
    to: "Alice",
    type: "greeting",
    data: {
      msg: "Hello, Alice."
    }
  })
  return state;
}
```

Reset and run the simulation. Alice and Bob turn blue and red, respectively, after they receive the others greeting.

It's a little boring to just have them stay red and blue though. We can make it more interesting by having them flip colors whenever they receive a new message.

```javascript
    state.color = state.color == "blue" ? "green" : "blue"
```

```javascript
    state.color = state.color == "purple" ? "red" : "purple"
```

![Hello HASH!](../.gitbook/assets/blocks_flipping.gif)

Finally, since Alice clearly would like some socially responsible distance from Bob, we can add movement to the agents. Using the builtin "random\_movement" behavior, each agent will move about the landspace at random.

```javascript
[ 
  { 
    "agent_name": "Bob",
    "behaviors": ["hello_bob.js", "random_movement"], 
    "position": [1,1] 
  },
  { 
    "agent_name": "Alice", 
    "behaviors": ["hello_alice.js", "random_movement"], 
    "position": [0,0] 
  }
] 
```

To prevent them from expanding the bounds of the map, select properties.json and add x and y bounds.

```javascript
{
    "topology": {
        "x_bounds": [0, 20],
        "y_bounds": [0, 20]
    }
}
```

![Random Movement](../.gitbook/assets/apr-17-2020-14-40-53.gif)

