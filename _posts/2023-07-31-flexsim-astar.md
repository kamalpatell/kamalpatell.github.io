---
layout: post
title: "Travel Paths Using A* In FlexSim" 
date: 2023-07-31 18:00:00-0400
description: A-Star Algorithm
tags: flexsim
categories: flexsim
giscus_comments: false
related_posts: false
toc:
  beginning: true
---

In Flexsim task executer, such as operators and transporters, travel in straight path between points, as it provides computational efficiency. However, this simplistic approach can lead to unrealistic scenario since real world operators cannot move through machines, conveyors, or other obstacles.

The use of straight-line paths not only hampers the visual realism of the simulation but also affects the accuracy of estimated system performance. To improve the realism and reliability of simulations, it is essential to implement object-avoidance algorithms and movement constraints that align with real-world constraints faced by operators and transporters.

### A-Star Algorithm

This is a popular path-finding algorithm that is commonly used in computer science and mathematics. It is specifically designed to find the shortest path from starting point to a goal point in a graph or a grid while considering the cost to reach the goal.

Its quite easy to implement A\* algorithm in Flexsim. The object associated with A\* algorithm are in the section of the object library below the visual section.

As shown in the figure below, create a simple model with operator transporting an item between Source and Sink. In between I have added some objects such as queue and processors that are not connected to any other resources.

1. Drag out a source, sink, operator and processor machines as show in the below figure.
2. Connect source to sink using A-connect.
3. Click source and in the properties section, under output click use transport.
4. Save the model as a-star
5. Reset and Run the model. you should see that the operator's path is through all the fixed object.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/blog/flexsim/astar/model.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Model
</div>

To avoid all the barriers and objects, drag A\* tool from the navigation section in the object library and place it in the modeling surface.

1. Double click the A\* object and you should see a properties tab as shown below.
2. In the members section, click the + button to add objects to the Traveler Members list. In this case select the operator1 in the operator section.
3. Similarly add FR(Fixed Resource) Members and select all the fixed objects such as queue and processors.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 text-center">
        {% include figure.html path="assets/img/blog/flexsim/astar/navigator.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Navigator Properties
</div>

Reset and Run the model. Now the operator avoids the objects and takes the shortest path between the Source and Sink.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/blog/flexsim/astar/model_run.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Model run
</div>

The blue box in the figure is an outline considered by A\*. It can he hidden by unchecking in the visual tab of the A\* Navigator properties box. A heat map shows the operator travel path frequency. So if you let the model run for a while you should see that the travel path in yellow to show high travel frequency on that path as opposed to green. To show heat map check the button in the visual tab of the A\* Star Navigator properties box and you can play around with other option in the behaviour and visual tabs.

An example of an You should a objects surrounding the sink and these are divider object. It s a set of line-segments in 3-dimensional space that creates either a one-way or two-barrier. The line segments are analogous to walls. These divider objects are in the same A\* Navigation section in the object library.

Drag divider from the A\* Star Navigation section and place it similar to the figure below around the sink. Reset the model and run. The Operator should identify and follow a path between the Source and Sink that avoids the Barrier and Divider

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/blog/flexsim/astar/astar_nodes.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
   A * Nodes
</div>


Using A\* in FlexSim can be helpful in various scenarios where you need to optimize movement paths for entities, such as material handling equipment, robots, or workers within the simulation environment.