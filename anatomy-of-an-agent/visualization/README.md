# Visualization

Once you've developed and tested the logic and functioning of your model, it can be helpful to create a compelling visualization that clearly turns your model into an explanatory tool.

**Agents have some predefined properties that allow you to modify their display in hCore's 3D spatial view:**

* `"shape": string` - if provided overrides the default "box" visualization for the agent. 

  * There are built-in geometric shapes and 3D models. See the [Shapes ](shapes.md)page for more information

* `"height": i64` - if provided \(and agent is rendering on a 2d grid\), will set the display height of the agent.

* `"scale": [i64, i64, i64]` - if provided, will re-render the agent in 3d space with the new scale. The position of the agent will remain the same \(e.g. for neighbor calculations\).

* `"direction": [i64, i64, i64]` - if provided, will render the agent as a cone oriented along that unit vector

* `"color": string` - if provided, will color the agent.  Named colors \("red", "green", "blue", etc.\) are supported, alongside hex color codes \(`#223344`\) and RGB values \(`rgb(12,244,155)`\).

* `"rgb": [i64, i64, i64]` - an alternate way to color an agent by providing RGB values from 0 - 255.

  * This will be overridden if the "color" field is also set on an agent.

* `"hidden": bool` - if provided and true, the agent will not be rendered on the 3d viewer. 
* `"direction": [i64, i64, i64]` - if an agent's shape is a 3D model, or a non-symmetric, the direction of an agent will affect its visualization.

In the [hIndex](https://hash.ai/index/search?query=display&sort=relevance&page=1) you can find published behaviors which have been created to help with more complicated visualization tasks.

{% hint style="info" %}
Not all models lend themselves well to spatial representation, and when viewing the results of multiple simulation runs at once \(as part of an [experiment](../../experiments/)\), it can be more useful to create [analysis charts and graphs](../../analysis.md).
{% endhint %}

