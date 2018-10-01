+++
title = "Baxter Multimaster"
date = 2018-09-29T15:32:15-06:00
draft = false
weight=20

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = []

# Project summary to display on homepage.
summary = "A fork of the multimaster package which I augmented to allow teleoperation of a slave Baxter robot by moving the limbs of master Baxter robot in gravity compensation mode."

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

This package is a fork of the [multimaster package](http://wiki.ros.org/multimaster) adapted to allow you to control one Baxter robot by moving the limbs of another Baxter robot. The purpose is to use one Baxter as a master teleoperation device to control a slave Baxter, where the slave is either a physical Baxter robot or a simulation. The joint states of the master Baxter are interpreted as commands to the slave Baxter, with appropriate namespacing so there is no conflict between the two.

The code for this package is hosted on the LL4MA Lab's bitbucket page [here](https://bitbucket.org/robot-learning/baxter_multimaster).