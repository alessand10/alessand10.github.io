---
title: The G3D Frame Graph
tags: [Rendering, Optimization, Design]
style: outline
color: warning
description: Building a modular graph system to assemble render instructions.
---

### Summary
This article will discuss using a graph structure to build an extendable and efficient system for assembling frame-building instructions.

<br/>

### Background
One major goal of the G3D game engine is to be easily extendable. While the engine may use certain techniques and methods today, there is no way to know for certain how the field of 3D graphics will evolve. To ensure that the engine is ready for new additions and changes, modularity is a guiding principle of its design. Through modularity, we can split large tasks into isolated elements that seamlessly work together to form a final result. This approach allows us to test, improve, and replace system components with minimal difficulty. A particularly large system that can benefit from a modular approach is the assembly of rendering instructions in a 3D application. These are the motivations that gave rise to a system called the G3D frame graph.

<br/>

### The car analogy
Suppose changing the tire of a car requires you to open its engine. Every tire change would be an expensive and time-consuming nightmare. Similarly, if a game engine is built without concern for modularity, then making changes can be an expensive and time-consuming nightmare.

<br/>

### What is the G3D frame graph?
The G3D frame graph is a component of the G3D game engine that is responsible for assembling and executing the logic necessary to render a frame. It is a modular system that allows engine developers to leverage the benefits of encapsulation and graph theory to assemble and execute the instructions that render a 3D world. 

<br/>

### Frame graph nodes
The frame graph fundamentally starts with nodes. Each node may use a set of inputs and provides a set of outputs. 

<div style="width: 45%; margin: 0 auto 0 auto; padding: 0px 0px 15px 0px">
    <img src="{{site.baseurl}}/assets/images/IMG_1319.jpeg">
</div>

Various types of nodes exist, providing different behaviours when traversed:
- Action node: Runs a function bound to the node.
<div style="width: 25%; margin: 0 auto 0 auto; padding: 0px 0px 15px 0px">
    <img src="{{site.baseurl}}/assets/images/IMG_1322.jpeg">
</div>

- Repeat node: Repeats the "repeat" outflow a certain number of times, as specified by input. Provides the current iteration number as output.

<div style="width: 45%; margin: 0 auto 0 auto; padding: 0px 0px 25px 0px">
    <img src="{{site.baseurl}}/assets/images/IMG_1320.jpeg">
</div>

- Do once node: Only proceed to the next node once.

<div style="width: 25%; margin: 0 auto 0 auto; padding: 0px 0px 25px 0px">
    <img src="{{site.baseurl}}/assets/images/IMG_1321.jpeg">
</div>

- Sequence Node: Has multiple outflow nodes, and traverses each in order before proceeding. The outflow # output can be used to determine which outflow index is currently being traversed.

<div style="width: 40%; margin: 0 auto 0 auto; padding: 0px 0px 25px 0px">
    <img src="{{site.baseurl}}/assets/images/IMG_1323.jpeg">
</div>

<br/>

### Why use this system?

This system introduces complexity, but the benefit is that we now have the ability to encapsulate tasks as modular collections of nodes.

The following is a very simple example of how to use this system to draw a mesh for each light:

<div style="width: 60%; margin: 0 auto 0 auto; padding: 0px 0px 25px 0px">
    <img src="{{site.baseurl}}/assets/images/IMG_1324.jpeg">
</div>

If we combine these nodes into one compound node, then we now have a modular task that can be worked on in isolation from other nodes:

<div style="width: 60%; margin: 0 auto 0 auto; padding: 0px 0px 25px 0px">
    <img src="{{site.baseurl}}/assets/images/IMG_1326.jpeg">
</div>

We could have also encapsulated the set active light and draw mesh repeat/action nodes into their own separate modular nodes. 

<br/>

### Conclusions and future considerations

The G3D frame graph design allows us utilize the benefits of modularization and graph theory within render loops. As the logic of the G3D engine expands, the frame graph is well-suited to expand along with it.

As a future expansion to this system, we might consider breaking the graph into static sections and dynamic sections to ensure that command lists are only re-recorded when necessary.

My progress on this system can be followed by visiting the [G3D Engine]({{site.baseurl}}/projects/3-g3d-engine) project.




<br/>
<br/>
<br/>