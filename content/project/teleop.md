+++
title = "Teleoperation Mediated by Virtual Environment"
date = 2018-09-29T15:31:16-06:00
draft = false
weight=7

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = []

# Project summary to display on homepage.
summary = "A kinematic simulator with force rendering created with rViz to provide a training ground to learn robot policies from demonstration using a Phantom Omni haptic input device. The learned policy can then be executed on a real robot (or a simulated robot in Gazebo)."

# Optional image to display on homepage.
image_preview = "teleop.png"

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = false

# Does the project detail page use source code highlighting?
highlight = true

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = "teleop.png"
caption = ""

+++

An application for my research that I am very interested in pursuing is the problem of teleoperation over large communication latency (e.g. teleoperating a robot on Mars from your couch). I have set up a simple virtual environment for the purpose of testing some ideas. I use rViz for the visualization of the virtual environment, and a Phantom Omni (re-branded as the [Sensable Geomagic Touch](https://www.3dsystems.com/haptics-devices/touch)) haptic input device to control the robot. I track the relative change in pose of the haptic device stylus and interpret that as a change in pose in the robot's end-effector. I need joint angles to render the robot arm in rViz, so I use the pseudoinverse of the manipulator Jacobian to get the corresponding joint angles. I then simply publish these to a topic that rViz picks up on to render the visualization - it is a purely kinematic simulator (i.e. no physics engine is active).

I created a simple GUI using [Tkinter](https://wiki.python.org/moin/TkInter) to enable/disable task space dimensions selectively. Sliders are used to scale how sensitive movement is in each dimension, which is useful if you want to move a large amount or you want finer control when you're close to an object. I have created a more advanced version of this GUI that allows you to give multiple demonstrations, learn a policy (e.g. a Dynamic Movement Primitive), and execute that policy on a real robot or a robot in a Gazebo simulation. I've also done direct trajectory playback, i.e. just record a demo in the rViz simulator and then send it directly to a Gazebo robot. However, I have disabled these features for now as the packages it relied on changed significantly since I was using this package, and I need to do some re-integration to make it work again.

I have also set up force rendering in this environment by using the force feedback of the haptic device and rendering forces as a function of the penetration depth the end-effector has with an object. By setting the parameters differently, you can make objects feel "squishy" or very stiff. I have to admit, it's amazingly fun feeling like you're physically touching an object that is purely virtual. This feedback can be enabled/disabled via the GUI as seen in the video:

{{< youtube S0RFufFrDa0 >}}
<p>

I have also used this purely in Gazebo (without being mediated through the kinematic simulator). Here is a video of it here I've augmented the GUI to display the forces being applied in each dimension as registered by a simulated force sensor (using Gazebo's force sensor plugin):

{{< youtube SlULZfQPwRg >}}
<p>

The code for this package is hosted on the LL4MA Lab's bitbucket page [here](https://bitbucket.org/robot-learning/ll4ma_teleop).

## Future Directions
I would like to move away from Tkinter for the GUIs. They are nice for workshopping ideas quickly, but since I am already using rViz for the visualizations, I would really like to create an integrated environment with an rViz plugin. I have played around with the plugins a bit, and they are not so hard to create, so I will be porting to a better implementation at some point.