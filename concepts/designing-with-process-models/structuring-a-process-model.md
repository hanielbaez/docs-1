# Structuring a Process Model

Creating a process model in HASH starts with importing the [Process Modeling Library](https://hash.ai/@hash/process) into your simulation. To make use of the published behaviors it contains, you'll need to structure your model in a specific yet extensible way.

## Structure of the Agent

Process models in HASH are built on a single agent, and there are three elements to their definition:

* The `behavior` array - just like any agent in HASH, you'll be filling this with a mix of published behaviors from the Process Modeling Library and custom behaviors you've written.
* A `process_labels` array - this allows you to add descriptive labels to each process, and link them to their parameters.
* Parameter field - most process behaviors will have corresponding parameters that you need to define in the agent's fields, and can be used to accurately describe the real-world system.

### Behavior Array

The `behavior` array of an agent running a process model must contain two specific starting and ending behaviors, and can then contain a mix of custom and published behaviors. The first in the agent's array must be[`@hash/age/age.rs`](https://hash.ai/@hash/age) , and the last must be `@hash/process/reset_behavior_ind.js`.

{% hint style="warning" %}
These behaviors provide the agent with functionality and fields that allows the other process behaviors to run. Without them the other Process Library behaviors will throw errors.
{% endhint %}

{% code title="init.json" %}
```javascript
{
    "behaviors": [
        "@hash/age/age.rs",
        // more process behaviors such as source, delay, etc... 
        "@hash/process/reset_behavior_ind.js"
    ],
    ...
}
```
{% endcode %}

### Process Labels

The agent must also contain an array of labels for each process behavior. The labels allow you to give a descriptive name to each block. Only the behaviors listed on the following Process Behaviors page require a label; all other published or custom behaviors should have a `""` placeholder string.

{% code title="init.json" %}
```javascript
{
    "behaviors": [
        "@hash/age/age.rs",
        "@hash/process/source.js", 
        "@hash/process/delay.js", 
        "@hash/process/sink.js", 
        "@hash/process/reset_behavior_ind.js"
    ],
    "process_labels": [
        "",
        "start_process",
        "perform_action",
        "end_process",
        ""
    ],
    ...
}
```
{% endcode %}

### Parameters

The parameters for each block must be specified in the agent's fields. Each parameter is keyed according to the label in the `process_labels` array. For example:

*  The first process block on the agent is a Source block
* Its corresponding label is `start_process`
* Its parameters will be a struct keyed to `"start_process"`

```javascript
{
    "behaviors": [
        "@hash/age/age.rs",
        "@hash/process/source.js", 
        "@hash/process/delay.js",
        "@hash/process/delay.js", 
        "@hash/process/sink.js", 
        "@hash/process/reset_behavior_ind.js"
    ],
    "process_labels": [
        "",
        "start_process",
        "perform_action",
        "verify_action",
        "end_process",
        ""
    ],
    "process_parameters" : {
        "start_process": {
            // parameters for the Source block
        },
        "perform_action": {
            // parameters for the first Delay block
        },
        "verify_action": {
            // parameters for the second Delay block
        },
        "end_process": {
            // parameters for the Sink block
        }
     }
    ...
}
```

On the next page you can learn more about the parameters needed for each type of **block**, and their specific functions.

