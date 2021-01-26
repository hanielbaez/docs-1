# Process Blocks

### Source

_@hash/process/source.js_

The source behavior is the entry point to a process model. It can generate objects at various intervals and inject them into the process model.

{% code title="parameters" %}
```javascript
<block_name>: {
    // REQUIRED - the number of time steps between each new object being generated
    "frequency": number,
    // REQUIRED - the definition for each object sent through the process model
    "template": struct,
    // OPTIONAL - specify the block objects will be sent to next, 
    // instead of the subsequent one in the behaviors array 
    "next_block": string 
}
```
{% endcode %}

### Sink

_@hash/process/sink.js_

The sink behavior is generally the endpoint of a process model. It disposes of objects and records data.

{% code title="parameters" %}
```javascript
<block_name>: {
    // OPTIONAL - If true, counts the number of objects that arrive
    // and stores in a "<block_name>_count" field.
    "count": boolean,
    // OPTIONAl - If present, records an array of through-times for 
    // objects under an agent field named <string>.
    "time": string
    // OPTIONAL - If present, records the times objects waited for 
    // resources during the process model, in an agent field named "wait_times".
    "wait_times": boolean
}
```
{% endcode %}

### Delay

_@hash/process/delay.js_

The delay behavior causes objects in the process model to wait a certain amount of time before moving on to the next behavior.

{% code title="parameters" %}
```javascript
<block_name>: {
    // REQUIRED - the time an object will wait in the delay queue
    "time": number,
    // OPTIONAL - specify the block objects will be sent to next, 
    // instead of the subsequent one in the behaviors array 
    "next_block": string 
}
```
{% endcode %}

### Seize

_@hash/process/seize.js_

The seize behavior reserves and attaches resources to the object. Resource quantities are specified as numeric values on the agent containing the process model.

The name of the resource being seized should match that of a resource recovered by a Release block.

{% code title="parameters" %}
```javascript
<block_name>: {
    // REQUIRED - the name of the agent field which tracks the number of available 
    // resources. The <string> field on the agent must contain a number.
    "resources": string,
    // OPTIONAL - if true, objects will track the amount of time they wait for a
    // resources to become available.
    "track_wait": boolean,
    // OPTIONAL - specify the block objects will be sent to next, 
    // instead of the subsequent one in the behaviors array 
    "next_block": string 
}
```
{% endcode %}

### Release

_@hash/process/release.js_

The release behavior removes resources from the object and returns them to the agent.

The name of the resource being seized **must** match that of a resource reserved by a Seize block.

{% code title="parameters" %}
```javascript
<block_name>: {
    // REQUIRED - the name of the agent field which tracks the number of available 
    // resources. The <string> field on the agent must contain a number.
    "resource": string,
    // OPTIONAL - specify the block objects will be sent to next, 
    // instead of the subsequent one in the behaviors array 
    "next_block": string 
}
```
{% endcode %}

### Service

_@hash/process/service.js_

The service behavior seizes resources, delays the object, and then releases the resources, functioning as a composite of those three behaviors \(Seize, Delay, Release\).

{% code title="parameters" %}
```javascript
<block_name>: {
    // REQUIRED - the time an object will wait in the delay queue
    "time": number,
    // REQUIRED - the name of the agent field which tracks the number of available 
    // resources. The <string> field on the agent must contain a number.
    "resource": string,
    // OPTIONAL - if true, objects will track the amount of time they wait for a
    // resources to become available.
    "track_wait": boolean,
    // OPTIONAL - specify the block objects will be sent to next, 
    // instead of the subsequent one in the behaviors array.
    "next_block": string 
}
```
{% endcode %}

### Select Output

_@hash/process/select\_output.js_

This behavior allows a process to branch along two different paths, based on a conditional. There are two different ways to specify this conditional:

* based on a whether a field on the object is `true` or not
* based on a likelihood or rate

After the Select Output block, you should specify the blocks that make up the rest of the "true" path, then the blocks that make up the "false" path. If the two paths eventually rejoin, specify the rest of the blocks after the "false" path.

On the final block of the "true" path, specify the first block where the two paths rejoin using the `next_block` parameter.

{% code title="parameters" %}
```javascript
<block_name>: {
    // ONE OF condition_field OR a_chance IS REQUIRED
    
    // Checks whether the <string> field on the object is true or false to 
    // determine which path it will take.
    "condition_field": string
    // Randomly determines whether an object will be sent along the
    // "true" path. Other objects are sent to the false_block.
    "a_chance": number,
    // REQUIRED - specify the block that objects failing the condition check
    // will be sent to.
    "false_block": string,
    // OPTIONAL - specify the block objects will be sent to next, 
    // instead of the subsequent one in the behaviors array.
    "true_block": string 
}
```
{% endcode %}

### Time Measure Start

_@hash/process/time\_measure\_start.js_

This behavior records the time an object reached it, to enable calculating the elapsed time until the agent reaches the corresponding Time Measure End behavior.

{% code title="parameters" %}
```javascript
<block_name>: {
    // OPTIONAL - specify the block objects will be sent to next, 
    // instead of the subsequent one in the behaviors array.
    "next_block": string 
}
```
{% endcode %}

### Time Measure End

_@hash/process/time\_measure\_end.js_

This behavior determines the elapsed time it took an object to travel from the corresponding Time Measure Start behavior, and records that value. 

The process label of this behavior must match that of its corresponding Time Measure Start behavior.

{% code title="parameters" %}
```javascript
// Block name must match the time_measure_start
<block_name>: {
    // OPTIONAL - specify the block objects will be sent to next, 
    // instead of the subsequent one in the behaviors array.
    "next_block": string 
}
```
{% endcode %}

