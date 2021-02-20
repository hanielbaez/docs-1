# Add heterogeneity

One thing that sets agent-based modeling apart from analytic techniques or system dynamic models is the ease with which you can add 'heterogeneity'. This means that our population of agents can have different demographic features, for example in HASH you can easily create a distribution of age by adding a property and assigning different age values to different agents.

These different values can allow our agents to all behave in slightly different ways, or cause other agents to interact with them differently.

In our Getting Started model \(\[Python \]\([https://hash.ai/@hash/getting-started-base-pythonor](https://hash.ai/@hash/getting-started-base-pythonor) [Javascript](https://hash.ai/@hash/getting-started-base\), we'll start by providing our agents with an 'at\_risk' property. Different people agents will have different chances of having a severe response to getting sick. In the "create\_people" behavior, new agents are defined in the `person` variable. In that block of code, add a line for the property "at\_risk":

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// line 26
agents["people"].push({
    // ...
    at_risk: Math.random() < 0.5 ? true : false
}
```
{% endtab %}

{% tab title="Python" %}
```python
# line 18
person = {
    # ...
    'at_risk': True if random() < 0.5 else False
}
```
{% endtab %}
{% endtabs %}

In the "infection" behavior, when a **Person** agent gets infected, let's add logic to determine the severity of the infection:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// line 81
const severe_chance = state.at_risk ? at_risk_chance_of_severe : chance_of_severe;

if ((state.severity === "moderate") && (Math.random() < severe_chance)) {
    state.severity = "severe";
}
```
{% endtab %}

{% tab title="Python" %}
```python
# line 69
severe_chance = g['at_risk_chance_of_severe'] if state.get('at_risk') else g['chance_of_severe']

if (state["severity"]) == 'moderate') and (random() < severe_chance)) {
    state["severity"] = 'severe';
}
```
{% endtab %}
{% endtabs %}

When we instantiate a Person currently, they'll have a 50% chance of being at\_risk from the virus, and if they're infected, they'll have a different likelihoods of getting a severe infection. These likelihoods are defined in the "globals.json" file.

Of course during different scenarios and experiments we might want to vary the likelihood that someone will be at risk. Instead of hardcoding the likelihood, we will add a property called **at\_risk\_percent** to "globals.json". Here we set it to 5%:

```javascript
{
// ...
"at_risk_percent": 0.05
}
```

And in the "create\_people" behavior, we substitute that value for the hardcoded one:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// line 26
agents["people"].push({
    // ...
    at_risk: Math.random() < at_risk_percent ? true : false,
}
```
{% endtab %}

{% tab title="Python" %}
```python
# line 18
person = {
    # ...
    'at_risk': True if random() < g['at_risk_percent'] else False
}
```
{% endtab %}
{% endtabs %}

