---
description: Learn the basic terminology
---

# Agent-Based Modeling Basics

Agent-based modeling \(ABM\) is an incredibly powerful tool to simulate scenarios with complex deterministic behavior. 

In contrast with traditional simulation and modeling, agent-based modelers describe the behavior of _individual agents,_ rather than using formulas to describe the behavior or dynamics of the system as a whole. This allows for complex interactions to emerge from basic rules. On HASH, we call these rules _behaviors._ 

A popular example of an agent-based simulation is Conway's _Game of Life_ - a simulation performed on a grid, where each cell can be alive or dead. Here, the cells are _agents_, and the rules are _behaviors._ The behaviors for Conway's Game of Life are as follows:

> 1. Any live cell with two or three neighbors survives.
> 2. Any dead cell with three live neighbors becomes a live cell.
> 3. All other live cells die in the next generation. Similarly, all other dead cells stay dead.

These simple interactions produce what is called _emergent behavior_, where simple rules create complex results.

{% hint style="info" %}
Check out [Conway's Game of Life](https://hash.ai/index/5de93f49d4b4c15ea799dcd9/conway%27s-game-of-life) model in HASH
{% endhint %}

Try playing with the HASH model and modifying its initial state. Some arrangements of agents produce arrangements of agents known as "spaceships" that move across the grid as a group and can even generate new groups of agents.

Game of Life enthusiasts have spent years exploring the search space of the model and its possible creations. What took Conway weeks can now be done in seconds, and with HASH you can set up experiments, run analyses, and leverage the power of cloud computing directly in your browser.

