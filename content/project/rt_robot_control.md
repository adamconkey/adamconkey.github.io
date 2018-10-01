+++
title = "Real-Time Robot Control"
date = 2018-09-29T15:30:42-06:00
draft = false
weight=3

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = []

# Project summary to display on homepage.
summary = "Real-time Orocos controllers for the KUKA LBR4+ robot. Includes a controller switching framework for safely swapping controllers at runtime, and a simulated FRI (Fast Research Interface) component that allows Orocos components to be tested Gazebo."

# Optional image to display on homepage.
image_preview = "rt_control.png"

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = false

# Does the project detail page use source code highlighting?
highlight = true

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = "rt_control.png"
caption = ""

+++

We use the [Orocos Real-Time Toolkit](http://www.orocos.org/rtt) to control our KUKA LBR4+ arm, which ensures that our controllers meet the real-time requirements for the LBR4. The base implementation our lab started with comes from the [lwr_hardware](https://github.com/kuka-isir/lwr_hardware) package from the [Institut des Systèmes Intelligents et de Robotique (ISIR)](http://www.isir.upmc.fr/index.php?op=view_equipe&id=4&lang=fr).

I have added to this framework by writing a controller manager that allows safe swapping of controllers at runtime. I use this to rapidly record kinesthetic demonstrations on the robot by switching between a gravity compensation controller (for moving the arm kinesthetically), a high stiffness joint controller (for keeping the robot stationary), a joint PD controller (for moving the robot to a nominal starting position), and a task space inverse dynamics controller (for executing task space policies).

The LBR4+ comes with a [Fast Research Interface (FRI)](https://cs.stanford.edu/people/tkr/fri/html/), which allows us to run our custom controllers and read the robot state through ROS. Something I didn't like was having to work out bugs in my controllers and controller manager on the real robot. So, I created a simulated FRI so that I could run our Orocos controllers in Gazebo before going live on the real robot. The simulation FRI hooks into the Gazebo simulation directly to read the robot state, and accepts the torque command output from our controllers to interface with the ROS control hardware interfaces.

The code for this package is hosted on the LL4MA Lab's bitbucket page [here](https://bitbucket.org/robot-learning/lwr_hardware). You should look at the `adam_dev` branch, as that is where I develop my features until they are stable enough to merge to master.

## Future Direction
Some more work is needed for the simulated FRI component to make it truly replicate all the behavior of the actual FRI. I have only implemented what I needed, which is mainly exposing the joint state and torque commands. 