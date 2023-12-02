---
layout: page
title: Swarming of Robotics
project_title: Network Robotics Control
description: Simulate the control of 6 wheeled robots in PyBullet physics simulator.
img: assets/img/project4/swarm.png
importance: 5
# category: Projects
giscus_comments: false
---

## Swarm Robotics

Swarming of robots refers to the collective behavior exhibited by a group of robots or agents that work together in a coordinated manner, often emulating the behavior seen in natural swarms such as bird flocks, fish schools, or insect colonies. Swarming involves individual robots following simple rules or behaviors that, when combined, result in emergent complex behaviors at the group level. This was a phd level course which I took in my masters in mechanical engineeirng, where at the end of the course we had to simulate six robots in pybullet engine to undergo controlled geometrical formation while avoiding the obstacles to reach the final destination.

## Project Objective:

The goal is to simulate the control of 6 wheeled robots in PyBullet (a physics simulator) that need to navigate an environment. The robots should:

- Create a square formation in the original arena.
- Switch to a different formation to cross the narrow passages. In all formations, keep at least 1m between each robot.
- Navigate through the environment, avoid obstacles and reach the goal.
- Create a triangle formation once the goal has been reached.

## Target

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project4/target.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Target
</div>

## Description of the Controller:

A combination of the consensus algorithm and the rigidity-based control technique is employed to establish the formation and control of robots. To achieve the desired motion, a proportional-derivative control law is utilized to propel the formation forward. The equation are as shown below.

$$
F_{\text{formation},i} = \begin{bmatrix}
K_f \cdot (Dz_{(x, \text{desired})} - L_x)_i + D_f \cdot (\dot{z}_{(x,\text{desired})} - L_{\dot{x}})_i \\
K_f \cdot (Dz_{(y, \text{desired})} - L_y)_i + D_f \cdot (\dot{z}_{(y,\text{desired})} - L_{\dot{y}})_i \\
\end{bmatrix}
$$

Where 𝐾𝑓 and 𝐷𝑓 and gains, L is the graph Laplacian.

$$
F_{target,i} = \begin{bmatrix}
K_{T}*min(y_{\text{goal}} - y_i) + D_{T}\dot{x}_{i}\\
K_{T}*min(y_{\text{goal}} - y_i) + D_{T}\dot{y}_{i}\\
\end{bmatrix}
$$

## Code

The python code for the project is on my [Github](https://github.com/kkratos/Network-Controlled-Robotics-ME7974){:target="_blank"}repository.

## Result

In the project, I effectively utilized the pybullet physics engine to simulate a cohesive swarm of robots. As a result, I successfully achieved the designated project objective.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project4/square.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Square Formation
</div>