# Customer Reply

Now that Business agents are communicating with Customers, we need the Customers to send a reply back. To do this, let’s look at customer.js. Everytime a Customer agent receives a batch of messages, it will:

* Run the message data through a cost function to determine the change with the lowest cost for each Business
* Notify each Business where they would shop \(in relation to just that Business\)
* “Purchase” from the Business with the overall lowest cost return

The cost function will be as follows, where price is the Business’s item\_price, position is the Business’s position, and D is the distance function \(linear euclidean\) :

$$
C(B_i)=price_i+D(position_i)
$$

$$
D(p) = \sqrt{p_x^2+p_y^2}
$$

Create the function calculate\_cost\(\) in customer.js.

{% tabs %}
{% tab title="customer.js" %}
```javascript
const behavior = (state, context) => {
    // Function to determine cost --> Business price * distance from Business
    const calculate_cost = (position, price) => {
        const state_position = state.position;
        return price + Math.sqrt(Math.pow((state_position[0] - position[0]), 2) + Math.pow((state_position[1] - position[1]), 2));
    }
}
```
{% endtab %}
{% endtabs %}

Next, we are going to collect and store all the messages sent by the Business agents into a dictionary. This will make for easy access and iteration when we compare. Place this code underneath calculate\_cost\(\).

```javascript
const collect_business_data = (messages) => {
   let shops = {};
   messages.filter((message) => message.type === "business_movement")
     .forEach((message) => {
       const agent_id = message.from;
 
       if (agent_id in shops) {
         shops[agent_id].data.push([message.data.position, message.data.price, message.data.rgb]);
       } else {
         shops[agent_id] = {
           data: [[message.data.position, message.data.price, message.data.rgb]]
         }
       }
     });
 
   return shops;
 }
```

Now that all the Business data is stored successfully, we need to iterate through each key \(or Business\) and determine which position and item\_price combination yields the lowest cost for the individual Business as well as the overall. 

Define a find\_min\(\) like the one below.

```javascript
 const find_min = (businesses) => {
   let overall_min = {
     cost: null,
     agent_id: "",
     position: [],
     price: 0,
     rgb: null
   };
 
   Object.keys(businesses).forEach((shop) => {
     let individual_min = {
       cost: null,
       agent_id: "",
       position: [],
       price: 0,
       rgb: null
     }
 
     // Find min cost for each business
     businesses[shop].data.forEach((business_change) => {
       const position = business_change[0]
       const price = business_change[1]
       const rgb = business_change[2]
 
       const cost = calculate_cost(position, price)
     })
   })
 }
```

{% hint style="info" %}
* **overall\_min →** store the data for the business position/price combination that yields the lowest cost across all businesses
* **Individual\_min →** store the data for the business position/price combination that yields the lowest cost across for each individual business
{% endhint %}

Let’s add the cost comparisons for both the individual Business and overall. Place the following code below your cost calculation \(at line 53 within your businesses\[shop\].data.forEach\(\) call\). 

```javascript
// Check min for individual business
 if (cost < individual_min.cost || individual_min.cost === null) {
 // TODO: Update individual_min
 } else if (cost === individual_min.cost) {
   if (Math.random() < 0.5) {
     // TODO: Update individual_min
   }
 }

 // Check min for all business
 if (cost < overall_min.cost || overall_min.cost === null) {
   // TODO: Update overall_min
 } else if (cost === overall_min.cost) {
   // 50% chance of picking another business if cost is the same or will pick the shop they shopped at last tick
   if (Math.random() < 0.5 || JSON.stringify(state.rgb) === JSON.stringify(rgb)) {
     // TODO: Update overall_min
   }
 }
```

Notice how many times you need to update overall\_min and individual\_min. Since both objects have the same structure and to save lines of code, let’s create another function in customer.js called update\_min\(\). \(Add it before find\_min\(\)\).

```javascript
 // Update properties of 'type' object (individual_min or overall_min)
 const update_min = (type, cost, id, position, price, rgb) => {
   type.cost = cost
   type.agent_id = id
   type.position = position
   type.price = price
   type.rgb = rgb
 }
```

Now we can replace the ‘TODO’ comments with a call to update\_min\(\) with the proper parameters. 

```javascript
// Check min for individual business
 if (cost < individual_min.cost || individual_min.cost === null) {
   update_min(individual_min, cost, shop, position, price, rgb)
 } else if (cost === individual_min.cost) {
   if (Math.random() < 0.5) {
     update_min(individual_min, cost, shop, position, price, rgb)
   }
 }

 // Check min for all business
 if (cost < overall_min.cost || overall_min.cost === null) {
   update_min(overall_min, cost, shop, position, price, rgb)
 } else if (cost === overall_min.cost) {
   // 50% chance of picking another business if cost is the same or will pick the shop they shopped at last tick
   if (Math.random() < 0.5 || JSON.stringify(state.rgb) === JSON.stringify(rgb)) {
     update_min(overall_min, cost, shop, position, price, rgb)
   }
 }
```

After all the comparisons have been made, we want to notify that particular Business where this Customer would prefer to shop. Let's send a message just below the businesses\[shop\].data.forEach\(\) call within the key iterator.

```javascript
state.addMessage(individual_min.agent_id, "customer_cost", {
    cost: individual_min.cost,
    position: individual_min.position,
    price: individual_min.price
});
```

All that’s left to do now for customer.js is to update the agent’s color to the overall\_min’s color. This signifies that the Customer decided it would want to purchase from that particular business. Set the agent’s color after the closing of the object key iterator \(on line 89\).  

```javascript
// Only update color if min cost was determined during this time step
 if (overall_min.rgb !== null) {
   state.rgb = overall_min.rgb;
 }
```

Finally, call find\_min\(\) below your call to collect\_business\_data\(\) and pass in the const variable businesses.

```javascript
const businesses = collect_business_data(context.messages());
find_min(businesses);
```

The behavior customer.js is finally complete!

{% hint style="success" %}
To see **customer.js** in full, navigate to bottom of this section or click on ‘**Phase 1** **Final Code**’ in the sidebar.
{% endhint %}

{% hint style="danger" %}
Before we can see the code in action, we first need to add a counter to Business agents. Since Business agents are sending around 100 \(neighbors\) x 6 \(positions\) x 3 \(prices\) messages at one time, we don’t want this to occur every time step.

1. Add the HASH shared behavior **Counter** \(shortname: @hash/counter/counter.rs\) to your simulation and add the counter behavior to your business agents BEFORE your behavior business.js. \(You want the counter to increment before business.js is called\)
2. In init.json give your Business agents three more variables:

* counter: 0
* counter\_reset\_at: 2
* counter\_reset\_to: 0

    ****3. Wrap your query\_customers\(\) call in the following if statement:
{% endhint %}

```javascript
if (state.counter === 0) {
     query_customers(context.neighbors(), state.position);
}
```

{% hint style="danger" %}
The behavior **Counter** adds a counter variable to every business agent that will automatically increment at each time step. This ensures that query\_customers\(\) is only called every 3 time steps. 
{% endhint %}

Reset and run!

If you followed all the steps above, run the simulation a couple times and you should see the customer agents change color based on their decision after a couple time steps. 

![](../../.gitbook/assets/lc_p2_customers.gif)

{% tabs %}
{% tab title="customer.js" %}
```javascript
const behavior = (state, context) => {
 // Function to determine cost --> business price + distance from business
 const calculate_cost = (position, price) => {
   const state_position = state.position;
   return price + Math.sqrt(Math.pow((state_position[0] - position[0]), 2) + Math.pow((state_position[1] - position[1]), 2))
 }
 
 const collect_business_data = (messages) => {
   let shops = {};
   messages.filter((message) => message.type === "business_movement")
     .forEach((message) => {
       const agent_id = message.from;
 
       if (agent_id in shops) {
         shops[agent_id].data.push([message.data.position, message.data.price, message.data.rgb]);
       } else {
         shops[agent_id] = {
           data: [[message.data.position, message.data.price, message.data.rgb]]
         }
       }
     });
 
   return shops;
 }
 
 // Update properties of 'type' object (individual_min or overall_min)
 const update_min = (type, cost, id, position, price, rgb) => {
   type.cost = cost
   type.agent_id = id
   type.position = position
   type.price = price
   type.rgb = rgb
 }
 
 const find_min = (businesses) => {
   let overall_min = {
     cost: null,
     agent_id: "",
     position: [],
     price: 0,
     rgb: null
   };
 
   Object.keys(businesses).forEach((shop) => {
     let individual_min = {
       cost: null,
       agent_id: "",
       position: [],
       price: 0,
       rgb: null
     }
 
     // Find min cost for each business
     businesses[shop].data.forEach((business_change) => {
       const position = business_change[0]
       const price = business_change[1]
       const rgb = business_change[2]
 
       const cost = calculate_cost(position, price)
 
       // Check min for individual business
       if (cost < individual_min.cost || individual_min.cost === null) {
         update_min(individual_min, cost, shop, position, price, rgb)
       } else if (cost === individual_min.cost) {
         if (Math.random() < 0.5) {
           update_min(individual_min, cost, shop, position, price, rgb)
         }
       }
 
       // Check min for all business
       if (cost < overall_min.cost || overall_min.cost === null) {
         update_min(overall_min, cost, shop, position, price, rgb)
       } else if (cost === overall_min.cost) {
         // 50% chance of picking another business if cost is the same or will pick the shop they shopped at last tick
         if (Math.random() < 0.5 || JSON.stringify(state.rgb) === JSON.stringify(rgb)) {
           update_min(overall_min, cost, shop, position, price, rgb)
         }
       }
     })
     
     state.addMessage(individual_min.agent_id, "customer_cost", {
         cost: individual_min.cost,
         position: individual_min.position,
         price: individual_min.price
       });
   })
 
   // Only update color if min cost was determined during this time step
   if (overall_min.rgb !== null) {
     state.rgb = overall_min.rgb;
   }
 }
 
 const businesses = collect_business_data(context.messages());
 find_min(businesses);
}
```
{% endtab %}

{% tab title="init.json" %}
```
[
 {
   "behaviors": [
     "@hash/create-grids/create_grids.js",
     "@hash/create-scatters/create_scatters.js",
     "update_businesses.js",
     "@hash/create-agents/create_agents.js",
     "@hash/remove-self/remove_self.js"
   ],
   "agents": {},
   "grid_templates": [
     {
       "template_name": "grid",
       "height": 1,
       "rgb": [
         255,
         255,
         255
       ],
       "behaviors": [
         "customer.js"
       ]
     }
   ],
   "scatter_templates": [
     {
       "template_name": "businesses",
       "template_count": 2,
       "height": 3,
       "item_price": 10,
       "counter": 0,
       "counter_reset_at": 2,
       "counter_reset_to": 0,
       "behaviors": [
         "@hash/counter/counter.rs",
         "business.js"
       ]
     }
   ]
 }
]

```
{% endtab %}

{% tab title="business.js" %}
```javascript
const behavior = (state, context) => {
 const send_message = (agent_id, position, price) => {
   state.addMessage(agent_id, "business_movement", {
     position,
     price,
     rgb: state.rgb
   });
 }
 
 const price_messaging = (agent_id, position) => {
   const item_price = state.item_price;
   send_message(agent_id, position, item_price);
   send_message(agent_id, position, item_price + 1);
   if (item_price > 1) {
     send_message(agent_id, position, item_price - 1);
   }
 }
 
 const query_customers = (neighbors, state_position) => {
   const possible_movement = [[-1, 0], [0, 0], [1, 0], [0, -1], [0, 1]];
   neighbors.filter((neighbor) => neighbor.behaviors.includes("customer.js"))
     .forEach((neighbor) => {
       possible_movement.forEach((movement) => {
         const new_position = [state_position[0] + movement[0], state_position[1] + movement[1]];
         price_messaging(neighbor.agent_id, new_position);
       })
     })
 }
 
 
 if (state.counter === 0) {
   query_customers(context.neighbors(), state.position);
 }
}

```
{% endtab %}
{% endtabs %}

