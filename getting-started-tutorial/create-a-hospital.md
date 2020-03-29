# Create a Hospital

Click over to your initial state file. It should look like this:

```text
(properties) =>[{
   agent_id: "homes_and_people",
   position: [0, 0],
   behaviors: ["create_homes_people"],
   agent_templates: {
     home: {
       height: 2,
       color: "yellow"
     },
     person: {
       height: 0.5,
       color: "green",
       was_sick: false,
       infected: false,
       infection_length: 0,
       behaviors: ["infection","known_infection","random_movement"]
     } 
   }
  }
 ]
```

The initial agent is a creator agent. It has one behavior, “create\_homes\_people”. In the first timestep of the simulation it will generate homes, randomly dispersed around the map, and people, grouped around their houses. After it does that it will remove itself.   
  


```text
 
 state.messages.push({
   to: "HASH",
   type: "remove_agent",
   data: {
     agent_id: state.agent_id
     }
    }
   )
```

The creator pattern is a common and recommended method for generating multiple agents, and it’s worth taking the time to double check your understanding \[link to behavior\].  


But to start, let’s create one hospital. Since we’re not dynamically generating multiple hospitals, we can just add the hospital directly in our initialState page.  


```text
{
 agent_name: "Hospital",
 position: SET_POSITION,
 height: SET_HEIGHT,
 color: SET_COLOR,
 behaviors: []
}

```

Set the position, color, and height as whatever you’d like. Position is an array. The zeroeth index is the x position, and the first index is y position. Height can be any integer - the standard block size is 1. 

Click refresh \(highlight the reset button\). Congratulations, you’ve built your first hospital! We’re proud of you.  


At the moment the hospital doesn’t do anything. It just sits there, begging for a purpose. So let's add some functionality. There’s two things we want our hospital to do.   


1. Provide tests to people. If a person is infected and suspects they’re sick, they should be able to contact the hospital and request a test. The hospital will see if they’re sick and send them the results.
2. ICU capacity. If a person is really sick, they get admitted into the hospital’s ICU. The hospital should only have a limited set of beds, and if they’re over capacity the person is turned away.

