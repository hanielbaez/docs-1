---
description: Configure your simulations for optimal performance and functionality
---

# Configuration

Imagine sitting down in front of a blank Python file and yet again configuring your environment, importing _Numpy_, _Matplotlib_, and _SciPy_, just to start plotting some results. We aim to eliminate the need to set up and configure environments from scratch with [hCore](https://hash.ai/platform/core).

hCore is extremely flexible right out of the box, filled with optimizations and tools that make developing simulation models easier than ever.

With the default configuration tools available you can:

* Set [global variables](basic-properties.md) which capture truths or assumptions about the state of your world
* Define the extents of the world using [bounds and wrapping](topology/bounds-and-wrapping.md)
* Change how agents interact with the borders \(if any\) of spatial simulations using [wrapping presets and flags](topology/wrapping-presets-and-flags.md)
* Configure how [distance functions](topology/distance-functions.md) work in different topologies

{% hint style="info" %}
Play around with simulations in hIndex to get a feel for how these flags change simulation dynamics
{% endhint %}

