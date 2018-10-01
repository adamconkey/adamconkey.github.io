+++
title = "Baxter Demonstration Recorder"
date = 2018-09-29T15:31:32-06:00
draft = false
weight=5

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = []

# Project summary to display on homepage.
summary = "A demonstration recorder for the Baxter robot utilizing the button and display interfaces to make it easier to record demonstrations when operating Baxter in gravity compensation mode."

# Optional image to display on homepage.
image_preview = "baxter_demo.png"

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = false

# Does the project detail page use source code highlighting?
highlight = true

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = "baxter_demo.png"
caption = ""

+++

I created a demonstration recorder for the Baxter robot to make it easier to capture sessions for my research in learning from demonstration. Everything is controlled from the robot using the button and display interfaces, allowing the teacher to rapidly give demonstrations without having to go back and forth between the robot and the computer. Current features include:

* Starting/stopping recording of robot and sensor data (e.g. joint states, end-effector pose, force sensor readings, etc.) using my [logging framework](/project/logger).
* Moving arm back to a nominal starting position.
* Changing the nominal starting position.
* Zeroing out a mounted force/torque wrist sensor.
* Showing current status on the head display, and swiveling the head display to the left or right automatically depending on which arm is in use.

The code is hosted on the LL4MA Lab's bitbucket page [here](https://bitbucket.org/robot-learning/baxter_util). Here is a short video showing off the features:

{{< youtube weBPQOT4Ymg >}}
