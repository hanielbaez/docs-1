# Query Customers

{% hint style="info" %}
Businesses will perform two actions during this simulation: query customers and collect customer responses to update their position and item price. 

* Query customers → Businesses will send their neighbors every possible position and item\_price change combination.

  * Position changes = \[ \[-1, 0\], \[0, 0\], \[1, 0\], \[0, -1\], \[0, 1\] \]
  * Item\_price changes = item\_price + 1, item\_price, item\_price - 1

* Collect customer responses → Businesses will collect and store all the customer responses to determine the position and item\_price combination with the largest profit
{% endhint %}

This action can be split into three separate functions within business.js: _send\_message_, _price\_messaging_, _movement\_messaging_.

Let’s start with send\_message. Send\_message\(\) will add messages to state.messages with a neighbor agent\_id, position, item\_price, and rgb. Add this function to business.js. \(Rgb is sent so we can visually see which business a customer chooses to shop at\)

{% tabs %}
{% tab title="business.js" %}
```javascript
const behavior = (state, context) => {
    const send_message = (agent_id, position, price) => {
        state.addMessage(agent_id, "business_movement", {
           position,
           price,
           rgb: state.get("rgb") 
        });
    }
}
```
{% endtab %}
{% endtabs %}

The next step is to create the price\_messaging\(\) function. This function will receive the neighbor agent\_id and position change, iterate through the possible item\_price values, and call send\_message to notify all possible neighbors.

```javascript
const price_messaging = (agent_id, position) => {
    const item_price = state.get("item_price");
    send_message(agent_id, position, item_price);
    send_message(agent_id, position, item_price + 1);
    if (item_price > 1) {
        send_message(agent_id, position, item_price - 1);
    }
}
```

Notice here how we check if the item\_price is greater than 1. This is important as we don’t want the item\_price to drop below 0.

The final function we are going to create for querying Customers is movement messaging; however, since this will be our top level function we are going to name it query\_customers\(\). In this function, business agents are going to iterate through their Customer neighbors and send them all possible position and item\_price change combinations.

```javascript
const query_customers = (neighbors, state_position) => {
    const possible_movement = [[-1, 0], [0, 0], [1, 0], [0, -1], [0, 1]];
    neighbors.filter((neighbor) => neighbor.behaviors.includes("customer.js"))
        .forEach((neighbor) => {
            possible_movement.forEach((movement) => {
                const new_position = [state_position[0] + movement[0], state_position[1] + movement[1], 0];
                price_messaging(neighbor.agent_id, new_position);
            })
        })
}
```

You can test your new code by placing the following snippet underneath your function declarations \(at line 30\) and looking at the Raw Output view tab.

```text
query_customers(context.neighbors(), state.get("position"));
```

Find the messages field for a Business agent and it should be filled with “business\_movement” type messages.

{% tabs %}
{% tab title="business.js" %}
```javascript
const behavior = (state, context) => {
 const send_message = (agent_id, position, price) => {
   state.addMessage(agent_id, "business_movement", {
     position,
     price,
     rgb: state.get("rgb")
   });
 }
 
 const price_messaging = (agent_id, position) => {
   const item_price = state.get("item_price");
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
 
 query_customers(context.neighbors(), state.get("position"));
}

```
{% endtab %}
{% endtabs %}

