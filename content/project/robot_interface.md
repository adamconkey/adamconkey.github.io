+++
title = "Robot Interface"
date = 2018-09-29T15:10:32-06:00
draft = false
weight=2

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = []

# Project summary to display on homepage.
summary = "Abstraction layer for controlling robots in a unified manner while accounting for robot platform-specific needs."

# Optional image to display on homepage.
image_preview = ""

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = false

# Does the project detail page use source code highlighting?
highlight = true

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = ""
caption = ""

+++
Different robots have different communication requirements for control and diagnostics. In ROS, this can mean different message types, different topic names, different interfaces for a robot in simulation versus reality, and so on. This can greatly complicate things as you end up having to create separate controllers and monitoring nodes for each platform. I created a robot interface abstraction layer to try to mitigate these issues. The idea is that each robot has its own interface that implements the requirements particular to that platform, and each interface inherits from a common interface. I have implemented a number of controllers in my [controller package](https://bitbucket.org/robot-learning/ll4ma_robot_control) that use the common interface, such that the controllers are agnostic to the robot being controlled, and they can be instantiated with any interface in this package.

I have implemented interfaces for the KUKA LBR4+ (simulation), Baxter (real and simulation), and the Righthand Robotics ReFlex hand (simulation). [Balakumar Sundaralingam](https://balakumar-s.github.io/) has implemented an interface for the real and simulated Allegro hand.


The code is hosted on the LL4MA Lab's bitbucket page [here](https://bitbucket.org/robot-learning/ll4ma_robot_interface).

## Future Direction
The goal is to build up these interfaces so that every platform can be seamlessly operated in both simulation and the real world. I have gotten away from the development a bit because making progress on research is more important right now. But, here are the current things to be added:

* Integrate the KUKA LBR4+ interface with our [real-time Orocos controllers](/project/rt_robot_control)
* Add real-robot support for the ReFlex interface