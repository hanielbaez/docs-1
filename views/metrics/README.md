# Metrics

## What is Simulation Analysis?

When running a simulation using HASH, you describe a system of agents that interact with each other in a given environment. The result of the simulation is determined by many factors:

* the initial conditions
* the behaviors assigned to each agent
* randomness

While the simulation is running you may be able to glean insight from visually observing your model. To supplement this, HASH provides you with a **Plots** view that can allow you to learn more about your simulation. For instance:

* Is a certain variable converging upon a value? 
* What emergent phenomena are appearing? 
* How do stochasticity and the initial conditions affect the simulation run?

These are the kinds of questions you can answer with HASH's analysis capabilities. It allows you to define Metrics, and then use them to create Plots.

{% embed url="https://youtu.be/1ILw6dEbWoE" %}

## Metrics

Your first step is to define Metrics that you are interested in plotting.  Each Metric is an output of your simulation, represented as an array of data. Metrics are defined as a series of Operations which transform your simulation data into an array of specific data, typically by filtering for specific agents, then retrieving a certain value from each agent. The available Operations are listed below.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Operator Name</th>
      <th style="text-align:left">Additional Arguments</th>
      <th style="text-align:left">Operator Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>&quot;filter&quot;</code>
      </td>
      <td style="text-align:left">
        <p><code>&quot;field&quot;</code>  <code>&quot;comparison&quot;</code>
        </p>
        <p><code>&quot;value&quot;</code> 
        </p>
      </td>
      <td style="text-align:left">Filter the current output with the given <em>comparison</em> and <em>value</em> on
        the given <em>field</em> of each element</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>&quot;count&quot;</code>
      </td>
      <td style="text-align:left">n/a</td>
      <td style="text-align:left">Count the number of agents in the current output</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>&quot;get&quot;</code>
      </td>
      <td style="text-align:left"><code>&quot;field&quot;</code>
      </td>
      <td style="text-align:left">Retrieve the <em>field</em> value from each agent in the current output</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>&quot;sum&quot;</code>
      </td>
      <td style="text-align:left">n/a</td>
      <td style="text-align:left">Sum over the elements of the current output</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>&quot;min&quot;</code>
      </td>
      <td style="text-align:left">n/a</td>
      <td style="text-align:left">Return the minimum of the elements in the current output</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>&quot;max&quot;</code>
      </td>
      <td style="text-align:left">n/a</td>
      <td style="text-align:left">Return the maximum of the elements in the current output</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>&quot;mean&quot;</code>
      </td>
      <td style="text-align:left">n/a</td>
      <td style="text-align:left">Return the mean of the elements in the current output</td>
    </tr>
  </tbody>
</table>

Many of these operations are aggregators: they will reduce the current output array to a single value. 

Like many data pipelines you first filter your data to the set you're interested in, and then aggregate it into a final metric.

The Metrics Wizard will help you construct your metrics and fill in the appropriate parameters. 

![The location of the New Metric and New Plot buttons](../../.gitbook/assets/image%20%2835%29.png)

For example, if you have a collection of agents with an age attribute in your simulation, you might want to count the number over 50. You will chain together operations like so:

![](../../.gitbook/assets/image%20%2836%29.png)

It's likely that the most common operation you'll use is "filter". You can filter with numeric, boolean, and string values. The valid comparisons are listed below:

| Comparison Name | Comparison Description |
| :--- | :--- |
| eq | Equal to \(===\) |
| neq | Not equal to \(!==\) |
| lt | Less than \(&lt;\) |
| lte | Less than or equal to \(&lt;=\) |
| gt | Greater than \(&gt;\) |
| gte | Greater than or equal to \(&gt;=\) |

