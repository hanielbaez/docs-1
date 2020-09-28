# Create an ICU

So now we can tell people they’re sick, and when they hear they’re sick they go home. But we also want to account for another function of a hospital - providing treatment for severe cases.

Let’s breakdown what we’ll need to modify in our existing model:

1. **At risk.** Right now a person is infected or not infected, but we want to delineate between the mild and severe cases. 
2. **ICU Capacity.** A hospital, so long as it has room in its intensive care unit, should treat severe cases. Instead of going home, the person will move to the hospital and stay there until they’re recovered.

On your Hospital initialization \(in `init.json`\), add a value for `icu_beds`. This will represent the number of Intensive Care Unit beds that a hospital has.

```javascript
 "icu_beds": 10,
```

In `init.json` , expand the hospital agent by adding a value for `icu_capacity`.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
{
  "agent_name": "Hospital",
  "position": [20,20],
  "height": 4,
  "color": "blue",
  "icu_beds": 10,
  "behaviors": ["test_for_virus.js"]
}
```
{% endtab %}

{% tab title="Python" %}
```javascript
{
  "agent_name": "Hospital",
  "position": [20,20],
  "height": 4,
  "color": "blue",
  "icu_beds": 10,
  "behaviors": ["test_for_virus.py"]
}
```
{% endtab %}
{% endtabs %}

If you reset the simulation and click on the hospital agent the inspect modal will now display the value for `icu_beds`. Click the inverted pyramid to select/deselect displayed attributes.

![](https://lh4.googleusercontent.com/PqbkGFIaaymIjL1HVPBW6Ca1abWdk_VAS46jf5hFyUlCGu6wAcPy7v0oZtApKkSP_ewJjWj3yg4YDJ0bQCGQGFuSMJ7T_Cd_RLu8Px8gbFoVmhhsLClTrSe_GlDHIFFx-Ps8tVME)

Open the `check_infected` file. A person agent is sending a request to the hospital to test them; now they should also send personal information to the hospital. In particular we want to know how likely it is they're `at_risk` of complications from the disease. It’s a little bit of a hand-wave that they are directly sending their `at_risk` level - you can imagine they’re sending a blood/spit sample and don’t know what it contains, or providing demographic info like their age or pre-existing conditions. In a more complicated model we'd likely determine their `at_risk` degree from a variety of different measures.

In this case, let's include a key-value pair in the message data packet for `at_risk`  in the "check\_infected" behavior:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
function check_hospital(){
   state.addMessage("Hospital", "test", {
       "test_sick": true,
       "at_risk": state.get("at_risk")
    })
 }
```
{% endtab %}

{% tab title="Python" %}
```python
def check_hospital():
   state.add_message("Hospital", "test", {
       'test_sick': True,
       'at_risk': state.get('at_risk')
    })
```
{% endtab %}
{% endtabs %}

Open the `test_for_virus` behavior and, in our message parsing loop, add control-flow logic to differentiate the risky cases from the non-risky cases. When a person is seriously ill, they get a bed in the hospital, so long as there’s a bed to give:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
test_messages.forEach(m => {
    // ... 
    let icu_or_home = false;
    
    if (state.get("icu_beds") && m.data.at_risk) {
        state.modify("icu_beds", icu => icu - 1);
        icu_or_home = true;
    }	
})
```
{% endtab %}

{% tab title="Python" %}
```python
for msg in test_messages:
    # ...
    icu_or_home = False
    icu_beds = state.get('icu_beds')
    
    if icu_beds > 0 and msg['data']['at_risk']:
        state.set('icu_beds', icu_beds - 1)
        icu_or_home = True
```
{% endtab %}
{% endtabs %}

Let’s add a flag that the person has a case severe enough that they will stay in the hospital; this is how we’ll let the person know they either need to stay at the hospital or they can rest up at home. Modify the message sent to include that variable:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
test_messages.forEach(m => {
    // ... 
    let icu_or_home = false;
    
    if (state.get("icu_beds") && m.data.at_risk) {
      state.modify("icu_beds", icu => icu - 1);
      icu_or_home = true;
    }
    
    state.addMessage(m.from, "test_result", {
        "sick": true,
        "icu_or_home": icu_or_home
    })
})
```
{% endtab %}

{% tab title="Python" %}
```python
for msg in test_messages:
    # ...
    icu_or_home = False
    icu_beds = state.get('icu_beds')
    
    if icu_beds > 0 and msg['data']['at_risk']:
        state.set('icu_beds', icu_beds - 1)
        icu_or_home = True
        
    state.add_message(m['from'], 'test_result', {
        'sick': True,
        'icu_or_home': icu_or_home
    })
```
{% endtab %}
{% endtabs %}

Let’s return to our person agent. They’ve just received a message from the hospital telling them if they're sick and if they should go home or come to the hospital. We already have the mild case handled - they go home. We need to modify the logic for the severe case:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
//A person checks for messages from the hospital telling them their test results
let msgs = context.messages().filter(msg => msg.type === "test_result");

msgs.forEach(msg => {
   if (msg.data.sick && msg.data.icu_or_home) {
      state.set("icu", true); 
      state.set("destination", state.get("hospital"));      
   } else if (msg.data.sick) {
      state.set("destination", state.get("home")); 
})
```
{% endtab %}

{% tab title="Python" %}
```python
# A person checks for messages from the hospital telling them their test results
msgs = list(filter(lambda m: m['type'] == 'test_result', context.messages()))

for msg in msgs:
  if msg['data']['sick'] and msg['data']['icu_or_home']:
    state.set('icu', True)
    state.set('destination', state.get('hospital'))
  elif msg['data']['sick']:
    state.set('destination', state.get('home')
```
{% endtab %}
{% endtabs %}

With this change if a person finds out they have a severe case, their destination is set as the hospital. 

We'll need to make a change to the `daily_movement` file as well, to prevent the agent from moving away once they've arrived at the icu until they're better.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Line 53
if (state.get("social_distancing") || state.get("icu")) {
  return;
}
```
{% endtab %}

{% tab title="Python" %}
```python
# Line 53
if state.get('social_distancing') or state.get('icu')):
  return
    
```
{% endtab %}
{% endtabs %}

When they recover, we want agents to signal to the hospital that they’re leaving and that there’s a free ICU bed available. And then, of course, we want agents to actually leave!

We need to add:

* A message sender that, when a person is recovered, sends a message to the hospital that they’re better.
* A message handler that increments the hospital's `icu_capacity` by 1 when it receives the “recovered” message.

The `infection` behavior handles the logic for infection state:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Line 87
if (state.get("infection_duration") === 0) {
    state.set("health_status", Math.random() < immunity_proportion ? "immune" : "healthy");
    state.set("color", "green");
    //TODO: notify the hospital the person has recovered
}
```
{% endtab %}

{% tab title="Python" %}
```python
# Line 74
if state.get('infection_duration') == 0:
  status = 'immune' if random() < g['immunity_proportion'] else 'healthy'
  state.set('health_status', status)
  state.set('color', 'green')
  # TODO: notify the hospital the person has recovered
```
{% endtab %}
{% endtabs %}

This is another opportunity to use message passing. We'll create a message to send to the Hospital telling them that the person has recovered.

{% hint style="info" %}
A key paradigm for HASH is message passing. HASH is based on the [actor model](https://en.wikipedia.org/wiki/Actor_model), and message passing between agents is how agents interact w/ one another.
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Line 87
if (state.get("infection_duration") === 0) {
    state.set("health_status", Math.random() < immunity_proportion ? "immune" : "healthy");
    state.set("color", "green");
    
    if (state.get("icu")) {
        state.addMessage("Hospital", "recovered", {
            "msg": "All Better!"
        })
        state.set("icu", false);
        state.set("destination", state.get("home"));
        state.set("out", true);
    }
}

```
{% endtab %}

{% tab title="Python" %}
```python
# Line 74
if state.get('infection_duration') == 0:
  status = 'immune' if random() < g['immunity_proportion'] else 'healthy'
  state.set('health_status', status)
  state.set('color', 'green')
  
  if state.get('icu'):
    state.add_message('Hospital', 'recovered', {
      'msg': 'All Better!'
    })
    state.set('icu', False)
    state.set('destination', state.get('home')
    state.set('out', True)
```
{% endtab %}
{% endtabs %}

Finally, let's handle the message logic on the Hospitals side in the "test\_for\_virus" behavior:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
 const recovered_messages = context.messages().filter(m => m.type === "recovered");
 //Frees up a bed for each (recovered,severe) case
 recovered_messages.forEach(m => state.modify("icu_beds", icu => icu + 1));
```
{% endtab %}

{% tab title="Python" %}
```python
recovered_messages = list(filter(lambda m: m['type'] == 'recovered', context.messages()))

# Free up a bed for each (recovered and severe) case
icu_beds = state.get('icu_beds')

for msg in recovered_messages:
    icu_beds += 1

state.set('icu_beds', icu_beds)
```
{% endtab %}
{% endtabs %}

Congratulations! You've added a hospital and some basic behaviors to your simulation. You should now be starting to see how adding agents and behaviors can quickly create models that mirror the real world.

{% hint style="info" %}
**Coming soon:** creating plots in this epidemic model. In the meantime, learn more about HASH's [analysis tools](https://docs.hash.ai/core/analysis) in general terms.
{% endhint %}

