# Add heterogeneity

One thing that sets agent-based modeling apart from analytic techniques or system dynamics models is the ability to add heterogeneity. This means that our population of agents can have different demographic features. In HASH, this means different property values. These different values can allow our agents to all behave in slightly different ways, or cause other agents to interact with them differently.

We’ll start by providing our agents with an 'age' property. By introducing the \`age\` property, we can begin to model the virus in a more specific and accurate manner.

```text

```

Every person now has an age, which lets us tie certain conditions \(like probability of severe sickness\) to their unique characteristics. We can change the likelihood of transmission based on the age of the agent and the likelihood they’re at\_risk:

```javascript
{
...
at_risk: Math.random() < at_risk_percent ? true : false,
}
```

Now in the infection file

```javascript
 const severe_chance = state.at_risk ? at_risk_chance_of_severe : chance_of_severe;
   if ((state.severity === "moderate") && (Math.random() < severe_chance)) {
     state.severity = "severe";
   }
```

