# Analysis

### What is Simulation Analysis?

When performing a simulation using HASH, you describe agents that interact with each other within a given environment. The result of the simulation is determined by many factors:

* the starting conditions
* randomness
* the behavior code assigned to each agent

While the simulation is running, and when it is done, you want to obtain insights on what is happening. Is a certain variable converging to a value? Are there interesting emergent patterns? How do starting conditions and randomness affect the simulation?

You can answer these questions with HASH's simulation analysis! It allows you to define simulation "outputs" and display them as  plots using the state of the simulation at a given timestep.

HASH gives you a hand by creating some default outputs and plots for you, which you can then manually tweak. The analysis file is organized as a JSON object with two major properties, outputs and plots. 

### Outputs

Outputs is a collection of JSON objects of the form

```javascript
"outputs": {
    "feature": [
        {
            Operation
        },
        {
            Operation
        }
        ...
    ]
}
```

The “feature” is a constructed feature of your simulation. What do you want to know about your analysis? For example if you have a collection of agents with an age attribute, you might want to count the number over 50. 

You derive the value by defining a series of operations to perform that will output the value. So in our 

```javascript
   "over_fifty": [
   {
       "op": "filter",
       "field": "age",
       "comparison": "gte",
       "value": "50"
     },
     { "op": "count"}
   ],
```

Operations are either comparison operators or aggregator operators. Aggregation operations are usually placed after comparison operations. Like many data pipelines you first filter your data to the set you're interested in, and then aggregate it into a final metric.

| Operation | Type |
| :--- | :--- |
| eq | comparison |
| neq | comparison |
| lt | comparison |
| lte | comparison |
| gt | comparison |
| gte | comparison |
| count | aggregator |
| sum | aggregator |
| min | aggregator |
| max | aggregator |
| mean | aggregator |
| movingAverage | aggregator |
| get | aggregator |
| filter | aggregator |

### Plots

Plots is an array containing a set of javascript objects which define the plots that visualize the outputs. 

The basic configuration of a chart:

```javascript
  {
     "title": "title",
     "layout": {
       "width": "% of analysis view width",
       "height": "% of analysis view height%"
     },
     "position": {
     //top left corner of plot
       "x": "0%",
       "y": "0%"
     },
     //type of chart
     "type": "type of chart",
     "data": [
       {
         "y": "component_1",
         "name": "component_1_name"
       },
       {
         "y": "component_2_name",
         "name": "component_2_name"
       }
     ]
   },
```

 

HASH implements Plotly plots. By default the x axis reflects the current timestep. You can use line,  bar, or area charts.

For more details on the layout and config fields, check out the [Plotly.js docs](https://plot.ly/javascript/reference/). Since HASH uses Plotly behind the scenes, HASH plots support any valid value for layout and config that Plotly supports.

  
  
  


