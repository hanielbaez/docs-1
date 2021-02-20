# Designing with Process Models

Process models allow you to represent systems or organizations which perform sequences of tasks. By chaining together delays, resource usages, and other blocks representing real-life tasks and events, you can simulate processes such as assembly lines, company workflows, and supply chains.

HASH provides the [Process Modeling Library](https://hash.ai/@hash/process) to make it easy for you to build your models. The library is easily extensible with custom behaviors that you can write to accurately model your system.

Building process models allows you to answer questions about your system. They can help you improve resource allocation, reduce cycle-time, and identify other optimizations that can be made to your organization or system. This is especially effective when used alongside HASH's Experiments to explore multiple scenarios and configurations.

### The Process Model

Process models are composed of blocks, of which there are currently nine types. These blocks are published behaviors that you use in the `behaviors` array just like any others, and they execute in sequence. **Objects** are passed through the different blocks, representing a product on an assembly line, a bill being processed, etc. Each blocks in the `behaviors` array has a queue linked to it, where those objects are stored.

Each block operates in three parts:

1. The block retrieves all objects in its queue.
2. The block then performs a function, such as modifying on object property or seizing a resource from the agent.
3. The block puts modified objects back into its queue, or into the queue of the following block.

{% hint style="warning" %}
The only exception to this is the Sink block. Since it represents the end of a process, it does not send objects into a new queue.
{% endhint %}

