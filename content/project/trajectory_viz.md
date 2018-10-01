+++
title = "rViz Trajectory Visualization"
date = 2018-09-29T15:30:58-06:00
draft = false
weight=6

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = []

# Project summary to display on homepage.
summary = "Package for generating trajectory visualizations in rViz, including dynamic real-time visualizations showing a customizable trace of the robot's end-effector."

# Optional image to display on homepage.
image_preview = "kuka_viz.png"

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = false

# Does the project detail page use source code highlighting?
highlight = true

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = "kuka_viz.png"
caption = ""

+++

Currently this package primarily offers customizable traces for task space trajectories in rViz. It uses [Marker](http://wiki.ros.org/rviz/DisplayTypes/Marker) display types to show the end-effector position (or any other task space position) as it traverses through space. It's somewhat configurable in that you can choose the color and line style of the trace (solid or dotted). It also allows you to visualize many trajectories at once, e.g. if you have samples from a distribution of trajectories. I have also implemented visualization of a full pose so that a Cartesian frame shows the pose over time, and also a static trace of poses so the entire pose trajectory can be seen at once.

This package additionally has some action servers/clients (joint and task space) for commanding trajectories. These can be used to track the status of a trajectory being executed and publish interpolated waypoints to a low-level controller.

I will say the code in this repo is far from stable - many of the pieces came from different repos so they are not all fully integrated and there may easily be some bugs from that transition process. Please let me know if you find things that aren't working, I will be quick to fix them.

Here is a video of a couple different visualization styles:
{{< youtube yFKEfv2nIZA >}}


The code can be found on the LL4MA Lab's bitbucket page [here](https://bitbucket.org/robot-learning/ll4ma_trajectory_util).

