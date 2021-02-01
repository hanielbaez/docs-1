---
description: >-
  Capture truths or assumptions about the state of your world which can easily
  be varied
---

# Simulation Parameters

## Defining simulation parameters as global variables

Simulation parameters defined as part of a simulation's `globals.json` file are can be easily varied and subject to [experimentation](../experiments/). This makes globals an ideal place to define key assumptions about your simulated world.

If, for example, we wanted to cap the height of all trees in a [forest simulation](https://hash.ai/index/5e065650196c3fbd41d8bd43/forest), we might introduce the global variable `"maxTreeHeight"`. The `globals.json` file would contain something like:

```javascript
{
    "maxTreeHeight": 50,
    ...
}
```

The associated tree growth behavior would follow:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
function behavior(state, context) {
    if (state.height + growth <= context.globals()["maxTreeHeight"]) {
        growtree()
    }
}
```
{% endtab %}

{% tab title="Python" %}
```python
def behavior(state, context):   
    if state['height'] + growth <= context.globals()['maxTreeHeight']):
        growtree()

```
{% endtab %}
{% endtabs %}

## Visual Globals

Parameters defined in `globals.json` can be viewed and modified either in code-form, or through hCore's visual interface. Click the _toggle visual globals_ button at the top right of the code editor pane to switch between these views.

![Toggle between edit and input of globals](../.gitbook/assets/kapture-2020-12-09-at-11.52.28.gif)

{% hint style="info" %}
By default a non-signed in viewer of a simulation will see and interact with the visual globals view.
{% endhint %}

The type of field input for a simulation parameter can be varied by adding a "schema" property to globals. Currently you can use schemas to specify these types of interfaces be displayed in the _visual globals_ interface:

* Color Picker
* Slider

#### Color Picker

{% code title="globals.json" %}
```javascript
  // Adds a color picker selector to a property
  {
    "<property_name>": "#ff0000",
    // More properties
    
    "schema": {
      "properties": {
        "<property_name>": {
          "type": "string",
          "enum": "colors"
        }
      }
    }
  }
```
{% endcode %}

![A color selector in the visual globals pane](../.gitbook/assets/screen-shot-2020-12-09-at-12.06.10-pm.png)

#### Slider

```javascript
  // Adds a slider selector to a property
  {
    "<property_name>": 5,
    // More properties
    
    "schema": {
      "properties": {
        "<property_name>": {
          "type": "number",
          "minimum": 0, // The minimum value in the slider
          "maximum": 10, // The maximum value in the slider
          "multipleOf": 1 // The increment the slider moves by
        }
      }
    }
  }
```

![Sliders for number parameters in the visual globals pane](../.gitbook/assets/image%20%2832%29.png)

