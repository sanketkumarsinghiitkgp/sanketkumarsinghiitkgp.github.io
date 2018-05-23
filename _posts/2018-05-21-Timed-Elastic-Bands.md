---
layout: post
title: Timed Elastic Bands (tldr for the original paper.)
---

This post is supposed to be a hand wavy introduction to timed elastic bands local planner without diving into into the complicated mathematics that the [original paper](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjs2uvVsJbbAhWHjJQKHbNXDF4QFggoMAA&url=http%3A%2F%2Fwww.rst.e-technik.tu-dortmund.de%2Flehrstuhl%2Fmitarbeiter%2Froesmann%2F2015_Roesmann_ECMR.PDF&usg=AOvVaw2lSpdiVTGzpFmxF9Yxi8dL) has.

I have skipped many key mathematical insights which may actually be indispensable to properly understand this planner. I'll try to go over them in a future post.


## What is it?

Timed Elastic Band(TEB) is an approach for online trajectory planning for mobile robots. It is a local planner, that is it produces an admissible trajectory that takes into account the kinematic constraints of the robot and sort of adheres to a coarse path decided by the global planner.
The real advantage of teb local planner over a conventional local planner like [DWA(dynamic window approach)](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwje9LvJxZbbAhUBHJQKHdsKAjUQFggtMAA&url=https%3A%2F%2Fpdfs.semanticscholar.org%2Fcd9e%2F0901a07262db18fb249efb82094e7c266527.pdf&usg=AOvVaw1qgxkHlBsGtH95BIbeoVAI) is that it doesn't get stuck in a locally optimum trajectory.

## Some important terms

### Elastic Bands

Elastic bands are formed by optimizing the global plan locally by minimizing the length of the path while keeping a distance from the obstacles(taking into account even the moving obstacles).
Suppose the path provided by the a planner is jagged and discontinuous. It may not be entirely feasible to move along this discontinuous path. 

To achieve a smoother path, we apply 'forces' on the path:

- **contraction force** along the path to remove any slack in the path

- **repulsive forces** from the obstacle

This deforms the path to produce a smooth trajectory.
An efficient implementation of this is achieved through creation of local subsets of free space called bubbles, which we will not discuss here.

### Trajectory Optimization

**x** denotes the state of the system. which is generally a column vector with postion and velocity information.
**X** is the search space that is the set of admissible values of **x**.

**J**is the cost function that is to be minimized for an optimum trajectory.

### Homotopic and Homologous Trajectories

In the context of mobile robot planning both can be considered the same.
Two trajectories are said to be homotopic if one can be deformed to form another without intersecting any obstacles in the process. A set of trajectories which are all homologous to each other is called  ***homology class***

### H-signature
H-signature is a function of trajectory. For a given starting point and ending point of a trajectory, when the H-signature is evaluated, it turns out that every trajectory in a homology class yields the same value. So, the H-signature can be used to uniquely identify a homology class.

## Discovery of Homology Classes

A set of Vertices ***V*** is formed. First of all the goal and starting points are added to this set.

### For circular obstacles of limited radius

Two waypoints per obstacle is added to ***V***. These point are such that the line passing through them passes through the centre of the obstace, is perpendicular to the line joining the goal and the position of bot, and the distance between them is slightly greater than the diameter of the obstacle.

### For non circular obstacles

Random points are sampled from a region between the bot position and the goal. Generally this region is taken to be the circle which has the goal and the bot position as diametrically opposite points.

Now, we connect the vertices with edges.

Two vertices are connected only if the direction of connection is forward oriented, that is it has a positive dot product with the the vector from bot position to the goal.

Moreover, the edge should not intersect any obstacle.


The goal in case of a local planner is a temporary one, as decided by the global planner.

Now a depth first search augmented by a visited list is performed for the graph.

For each path the H-signature is calculated. If no previously added candidate path has the same H-signature, then the H-signature is added to set of known signatures, and the path is added to the set of candidate paths. If the H-signature is already known then the path is ignored.	

## Final trajectory

Each trajectory which represents a homology class is then optimized using elastic bands. At each instant the shortest of these candidate trajectories is chosen.
