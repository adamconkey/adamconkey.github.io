---
layout: post
title: Active Learning of Probabilistic Movement Primitives
description: 
date: 2019-10-15
image: '/images/promp_preview_rviz.png'
video_embed: 'https://www.youtube.com/embed/na91UyidDvE?si=-vOdeKplyWey6Ite'
tags: [Publications]
tags_color: '#4287f5'
featured: true
---
This was a paper published during my Ph.D. which I presented at the [Humanoids 2019](http://humanoids2019.loria.fr) conference in Toronto, Canada.

***

## Abstract

A Probabilistic Movement Primitive (ProMP) defines a distribution over trajectories with an associated feedback policy. ProMPs are typically initialized from human demonstrations and achieve task generalization through probabilistic operations. However, there is currently no principled guidance in the literature to determine how many demonstrations a teacher should provide and what constitutes a “good” demonstration for promoting generalization. In this paper, we present an active learning approach to learning a library of ProMPs capable of task generalization over a given space. We utilize uncertainty sampling techniques to generate a task instance for which a teacher should provide a demonstration. The provided demonstration is incorporated into an existing ProMP if possible, or a new ProMP is created from the demonstration if it is determined that it is too dissimilar from existing demonstrations. We provide a qualitative comparison between common active learning metrics; motivated by this comparison we present a novel uncertainty sampling approach named “Greatest Mahalanobis Distance.” We perform grasping experiments on a real KUKA robot and show our novel active learning measure achieves better task generalization with fewer demonstrations than a random sampling over the space.

***

## Software Development

This project involved a few interesting software development efforts to implement the required tooling for recording human demonstrations on the robot, visualizing the environment to aid user engagement, and executing policies in an efficient way.

| Repository | Description |
|------------|-------------|
| [ll4ma_movement_primitives](https://bitbucket.org/robot-learning/ll4ma_movement_primitives/src/master/) | Custom implementation of ProMP policies and the infrastructure needed to execute them on a robot. |
| [ll4ma_robot_control](https://bitbucket.org/robot-learning/ll4ma_robot_control/src/main/) | Real-time control framework with controller switching capabilities. Note: the LBR4-specific code is unfortunately a private repo for the lab. |

### Gravity Compensation Controller 

I needed to record human demonstrations of the task on the robot using a technique call _kinesthetic teaching_. This is a simple technique where the human physically takes hold of the robot and guides its motion to complete the task at hand. It has its pros and cons, but for the simple task in this paper it was a good choice.  

It is common to use a _gravity compensation controller_ for the robot which is a model-based controller that computes the motor torques required to keep the robot from falling to the ground as it would otherwise tend to do under the influence of gravity. This is a fun control mode because it means the robot is free to move about when a human user grabs hold of its links and moves it around, it feels as if the robot is weightless in spite of being quite heavy! While several robots come with a gravity compensation mode (including the KUKA LBR4 robot I used), this was going to impractical for my purposes as I was going to have to constantly switch the pendant controller between giving a demonstration and having the robot execute trajectories autonomously. This was also going to require a software restart on our PC each time the autonomous controller was restarted. I wanted a better way, since I knew I was going to record many demonstrations in each session.

The control workflow I needed was:
```
1. Put robot in gravity compensation mode
2. Provide a kinesthetic demonstration of the task
3. Put the robot in autonomous mode
4. Execute a trajectory autonomously to return the robot to a home position
5. Either do another demonstration, or execute a policy autonomously
```

So, I wrote my own control framework! I built off some of our lab's existing control infrastructure written in C++ using the [Orocos Real-Time Toolchain](https://docs.orocos.org). This was a complete refactor to support multiple controller modes that could be switched between using a ROS service call. This enabled me to seamlessly switch between teaching and autonomous modes with a simple software hook.

Another fun aspect of this is the interface. I initially developed a makeshift GUI to control a session. It was not pretty, but it got the job done. However, I found myself going back and forth between the computer and the robot repeatedly, it would get dizzying doing a demonstration session. I decided to hook up an X-Box controller to the PC, and use the registered button presses to switch between the different robot control modes. I could:

```
1. Press a button to start recording data and enter gravity compensation mode
2. Provide a kinesthetic demonstration of the task
3. Press a button to stop recording and save the data, and enter autonomous mode
4. Press a button to have the robot return to a home position
```

It was a bit of a makeshift setup, but it worked amazingly and was actually fun to use.

<!---->
<!-- ![Games](/images/09-1.jpg) -->
<!-- *Photo by [Branden Skeli](https://unsplash.com/@branden_skeli) on [Unsplash](https://unsplash.com/)* -->
<!---->
<!-- <div class="gallery-box"> -->
<!--   <div class="gallery gallery-columns-2"> -->
<!--     {% include img.html src ="/images/09-2.jpg" alt="Games" caption="Take a break from reality and dive into a game." %} -->
<!--     {% include img.html src ="/images/09-3.jpg" alt="Games" caption="Games are the perfect partner for unwinding." %} -->
<!--   </div> -->
<!--   <p>A gallery of stunning shots</p> -->
<!-- </div> -->

