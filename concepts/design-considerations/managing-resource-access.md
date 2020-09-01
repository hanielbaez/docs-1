# Managing Resource Access

Managing shared resource access is a common software design challenge. For instance [mutexes](https://en.wikipedia.org/wiki/Lock_%28computer_science%29) and similar locking mechanisms might be implemented to ensure only one process is modifying the resource at once.

By default HASH manages state through the actor model, with each individual actor controlling its own state. This avoids the common state management bugs that crop up in other paradigms, but it can make it tricky to plan access to shared resources. For instance, if two agents, \(**A**\) and \(**B**\), in time step 1 want something from a third agent \(**C**\), they might both message the request to the third agent. Since agents execute in parallel, they will both send their message on the time step 1, and two messages will arrive to **C.** 

Agent **C**  now needs to determine which agent should receive the resources, sending a message to the winning agent and a message to the losing agent. 

An example of this type of pattern is in [Sugarscape](https://hash.ai/index/5df7b0c0c36ba4aaa14f4a4e/sugarscape), in which agents search for and collect “sugar”. When an agent moves to a patch of sugar it sends a message to the patch, requesting sugar \(in 37 in **sugar\_agent.js\)**. The patch then sends sugar to only one of the agents who made a request \(in 20-30 in **sugar\_patch.js** \). This prevents multiple agents from “grabbing” the limited resource .

{% hint style="info" %}
Build checks for multiple of the same type requests into message handlers, to account for multiple agents requesting the same resource on a time step.
{% endhint %}

One problem that can arise is managing timescales. In the example above it will take two timesteps before agents **A** and **B** know which will have access to the resource from agent **C**, and since one of the agents didn't get access, that agent needs to spend more time either waiting \(and pinging the agent\) or going to alternative resources providers. This can be tedious if, for instance, you're trying to match N agents to N resource providers, and each agent is independently messaging providers, in the worst case every agent will message the same provider, N-1 will be rejected and then all message the same next provider, etc. A costly and lengthy operation. 

{% hint style="info" %}
We discuss similar situations in [Designing for Different Timescales](../designing-for-different-timescales.md).
{% endhint %}

The solution is to leverage manager agents to help resolve these conflicts. A manager agent, acting as a matcher, could receive requests from every requesting agent and every provider, loop through and match request to provider, and message each the agent\_id of their counterpart. 

