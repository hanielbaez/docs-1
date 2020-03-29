# Create an ICU



So now we can tell people they’re sick, and when they hear they’re sick they stay at home until they’re better. But we also want to account for another function of a hospital - providing treatment for severe cases.  


Let’s breakdown what we’ll need to modify in our existing model:

1. Severity. Right now a person is infected or not infected, but we want to delineate between mild and severe cases. 
2. ICU Capacity. A hospital, so long as it has room in its intensive care unit, should treat severe cases. Instead of going home, the person will move to the hospital and stay there until they’re recovered.



In your properties tab, add a value for icu\_beds. This represents the number of icu beds that a hospital has.  


```text
 "icu_beds": 10,
```

In the initialState model, expand the hospital agent and add a value for icu\_capacity.

```text
{
 agent_name: "Hospital",
 position: [20,20],
 height: 4,
 color: "blue",
 icu_beds: properties.icu_beds,
 behaviors: ["test_for_virus"]
}

```

If you reset the simulation and click on the hospital agent the inspect modal will now display the value for icu\_beds. Click the inverted pyramid to select/deselect displayed attributes.  


![](https://lh4.googleusercontent.com/PqbkGFIaaymIjL1HVPBW6Ca1abWdk_VAS46jf5hFyUlCGu6wAcPy7v0oZtApKkSP_ewJjWj3yg4YDJ0bQCGQGFuSMJ7T_Cd_RLu8Px8gbFoVmhhsLClTrSe_GlDHIFFx-Ps8tVME)

We’ve given each person a mild or severe case of the virus and it’s up to our hospital agent to detect which is which.

  
Open the known\_infection file. A person agent is sending a request to the hospital to test them; now they should also send personal information to the hospital. In particular their severity \(it’s a little bit of a handwave that they are directly sending their severity level - you can imagine they’re sending a blood/spit sample and don’t know what the severity property is!\)  


Include a key,value in msg.data for the person’s severity.  


```text
function check_hospital(state){
   state.messages.push({
     to: "Hospital",
     type: "test",
     data: {
       test_sick: true,
       Severity: state.severity
     }
   })
   return state;
 }
```

Open the test\_for\_virus file and, in our message parsing loop, add control-flow logic to differentiate severe and non-severe cases.  


```text
 test_messages.forEach((m) => {
... 
   if (m.data.severity) {
	//add logic
   }   
})

```

  
When a person is seriously ill, they get a bed in the hospital, so long as there’s a bed to give:  


```text
  if (m.data.severity && state.icu_beds) {
       state.icu_beds -= 1;
     }
```

  
  
Let’s add a flag that the person has a case severe enough that they will stay in the hospital; this is how we’ll let the person know they either need to stay at the hospital or they can rest up at home. Modify the message sent to include that variable.

```text
   let icu_or_home = false;
   ...
   if (m.data.severity && state.icu_beds) {
     state.icu_beds -= 1;
     icu_or_home = true;
   }
   state.messages.push({
     to: m.from,
     type: "test_result",
     data: {
       sick: true,
       icu_or_home: icu_or_home,
       position: state.position,
     }
   })
```

As you can see we included the hospital’s position in our message to the person agent. We want the person to stay at the hospital if they’re sick, and they’ll need to know the position of the hospital in order to move towards it. You could instead set the hospital position as a global property, or create a separate send/receive pattern for location requests.  


Let’s return to our person agent. They’ve just received a message from the hospital telling them if they have a mild or severe case of the virus. We already have the mild case handled - they stay at home until they’re better. We need to add logic for the severe case.

```text
 //If a person agent think's they're sick, they will stay at home
 
 //A person checks for messages from the hospital telling them their
 //test results
 let msgs = context.messages.filter((msg) => msg.type == "test_result");
 for (let msg of msgs){
   if (msg.data.sick && msg.data.icu_or_home) {
     state.icu = true;
     state.home_status = "goto_hospital";
   }
   //TODO add more options/polish
   else if(msg.data.sick) {
     state.home_status = state.home_status !== "home" ? "returning" : "home";
   }
 }
```

Now if a person finds out they have a severe case, their home\_status is set as “goto\_hospital”. In our home\_movement file we add a condition to direct the person to the hospital.

```text
else if (state.home_status === "goto_hospital") {
   // move to home location
   const [arrived, dir] = direction(state, state.hospital_pos);
   state.direction = dir;
 
   state.behaviors = ["infection", "home_movement", "check_infected", "move_in_direction"];
 
   if (arrived) {
     state.position = state.hospital_pos;
     state.direction = null;
     // if I reach the hospital, I'm in the icu
     state.home_status = "icu";
     state.behaviors = ["infection"];
   }     
 }
```

  
When they recover, the polite thing to do is let the hospital know they’re leaving and that there’s a free ICU bed available. We need to add: 

* A message sender that, when a person is recovered, sends a message to the hospital that they’re better.
* A message handler that increments the hospital's icu\_capacity by 1 when it receives the “recovered” message.

**Infection**

```text
if (state.icu) {
         state.messages.push({
           to: "Hospital",
           type: "recovered",
           data: {
             msg: "All Better!"
           }
         })
         state.icu = false;
         state.health_status = "returning";
         state.behaviors = ["infection", "home_movement", "check_infected", "move_in_direction"];
```

  
**Test\_for\_virus**

```text
 const recovered_messages = context.messages.filter(m=> m.type == "recovered");
 //Frees up a bed for each (recovered,severe) case
 recovered_messages.forEach((m) => {
   state.icu_beds += 1
 })
```

