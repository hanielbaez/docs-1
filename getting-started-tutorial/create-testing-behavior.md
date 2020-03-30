# Create testing behavior

To start let**'**s create a message from the person agent that contains the basics - they’re requesting a test. In the known behavior file, add:

```text
function check_hospital(state){
   state.messages.push({
     to: "Hospital",
     type: "test",
     data: {
       test_sick: true,
     }
   })
   return state;
 }
```

When run the check\_hospital function will send a message to the hospital with type test and data.test\_sick. But, we only want to send it when a person suspects that they are sick. We can add a property called time\_to\_symptoms. That’s how long it takes for a person to start showing symptoms.

```text
 "time_to_symptoms" : 5,
```

You'll need to also add it to the top of the test\_for\_virus behavior file.

```text
const { time_to_symptoms } = context.properties;
   -----
if (state.infected && state.infection_length >= time_to_symptoms) {
   state = check_hospital(state);
 }
```

Now after a certain period of time the person agent will get suspicious they’re sick, and send a message to hospital.

```text
 const test_messages = context.messages.filter(m=> m.type == "test");
```

On the receiving end we need to add a message handler for the hospital. Create a new behavior file called test\_for\_virus. 

The messages that arrive in an agent’s mailbox are those addressed to its id or name.

While right now it’s not necessary to filter by type == test, it’s good practice when, in the future, we send a wider variety of messages to the Hospital.

So what should we tell our patient? If the person is sick the test should detect that they are sick; right now the only time a person messages the hospital is if they’re sick, so testing will be easy!

Let's check all of the messages and respond to each person, letting them know they are in fact sick.

```text
 test_messages.forEach((m) => {
    state.messages.push({
     to: m.from,
     type: "test_result",
     data: {
       sick: true,
     }
   })
}

```

Back in the known\_infection file, we similarly want to check for any messages about our tests.

```text
 let msgs = context.messages.filter((msg) => msg.type == "test_result");
 msgs.forEach(msg => {
   if (msg.data.sick) {
     //do something
     break;
 })
```

Now that our agent knows it’s sick, what should we do? When you’re sick, you should stay home and rest. So should our person agents. We can change an agent’s behavior set during a simulation, for instance by removing their movement behavior and setting their position to their home. The `home_movement` file contains the logic for altering the person’s movement pattern based on their status.  In `check_infected`, set the health status as “returning” or, if they’re already at home, home.

```text
let msgs = context.messages.filter((msg) => msg.type == "test_result");
 msgs.forEach(msg => {
   if (msg.data.sick) {
state.home_status = state.home_status !== "home" ? "returning" : "home";
     	break;
   }
 })
```

Now our full page looks like this:

```text
(state, context) => {
  const { time_to_symptoms } = context.properties;
 
 
function check_hospital(state){
   state.messages.push({
     to: "Hospital",
     type: "test",
     data: {
       test_sick: true,
     }
   })
   return state;
 }
 
let msgs = context.messages.filter((msg) => msg.type == "test_result");
 msgs.forEach(msg => {
   if (msg.data.sick) {
    state.home_status = state.home_status !== "home" ? "returning" : "home";
     break;
 })
 
if (state.infected && state.infection_length == time_to_symptoms) {
   state = check_hospital(state);
 }
```

Run the simulation - our people are now being socially conscious and going back home when they’re sick. Hooray for well-being.

Of course, we’ve got one final part of the cycle to finish. Eventually the person will get better, and when they do they should feel free to leave their house. In the infection behavior file, reintroduce the movement behavior once a person has recovered.

```javascript
 
if (state.infection_duration === 0) {
  state.infected = false;
  state.immune = true;
  state.color = "grey";
  state.health_status = "returning";
  state.behaviors = ["infection", "home_movement", "check_infected", "move_in_direction"];
}

```

\[Extension\] Account for false negatives. Just like in real life tests have a chance of being wrong, add a chance the hospital sends back a false negative response and change the response behavior  


