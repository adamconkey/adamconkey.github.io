+++
title = "Robot Control in Gazebo"
date = 2018-09-29T13:59:00-06:00
draft = false
weight=1

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = []

# Project summary to display on homepage.
summary = "Robot control framework for simulations in Gazebo. It is designed to be extensible, allowing easy addition of new controllers, and it works in conjunction with my robot interface package to decouple the controllers from the particular robot you're using."

# Optional image to display on homepage.
image_preview = "kuka_control.png"

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = false

# Does the project detail page use source code highlighting?
highlight = true

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = "kuka_control.png"
caption = ""
+++

I have developed a robot controller framework that uses my [robot interface package](/project/robot_interface) to abstract away the particular requirements of each robot platform. As such, the controllers in this package should be useful for controlling any robot that has communication needs satisfied by one of the interfaces in the interface package. These controllers work for Gazebo simulations, and are written to inherit from an interface that mimics the [Orocos RTT::TaskContext](http://www.orocos.org/stable/documentation/rtt/v2.x/api/html/classRTT_1_1TaskContext.html), with the intention that these controllers can be easily ported for real-time control on a physical robot (see our [real-time controller package](/project/rt_robot_control)).

The current supported controllers are a joint space PD controller and a task space inverse dynamics controller. I have also implemented an operational space hybrid force/position controller and used it both in simulation and on a real Baxter robot. However, I have deprecated the force controller for now as the code design changed sufficiently from when I was using that controller, and am waiting until it is needed to update.

Here is a video of the KUKA LBR4+ arm in Gazebo using a joint space controller:
{{< youtube THkC697oesQ >}}


Here is a video of the ReFlex hand in Gazebo:

{{< youtube 2HQql-W8Lys >}}
<p>

The code for this package is hosted on the LL4MA Lab's bitbucket page [here](https://bitbucket.org/robot-learning/ll4ma_robot_control).
 
## Future Direction
Between this package, the real-time robot control package, and the robot interface, I think there is a better structure to enforce, but it will take maybe more development than it's worth right now. I think the control laws themselves should be made more portable, such that they can just be passed to a simulation controller or a real-time controller, without all the overhead in the simulation version. These packages may be jointly revamped at some point.
