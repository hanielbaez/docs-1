# Environment Setup

After opening up a new simulation in HASH, navigate to globals.json. First, we are going to set up the [**Topology**](https://docs.hash.ai/core/configuration/topology), or the environment, of the scene.

{% tabs %}
{% tab title="globals.json" %}
```text
{
    "topology": {
        "x_bounds": [
            0,
            10
        ],
        "y_bounds": [
            0,
            10
        ],
        "search_radius": 10
    }
}
```
{% endtab %}
{% endtabs %}

Setting the x\_bounds and y\_bounds creates a 10x10 boundary within the environment which will be used to generate the customer and business agents. It also sets the search\_radius of all agents within the simulation to 10, which will automatically update an agent’s neighbor list to include all agents within a distance of 10.  


