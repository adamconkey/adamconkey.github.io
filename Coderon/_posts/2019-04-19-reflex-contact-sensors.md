---
layout: post
title: Simulated Contact Sensors for ReFlex Robot Hand
description: Augmented robot model for the Gazebo simulator enabling simulation of distributed contact sensors.
date: 2019-04-19
image: '/images/reflex_contact_sensors/reflex_cover.png'
video_embed: 'https://www.youtube.com/embed/oEuwmzLlE84?si=7t2awd7ltj4Bu13F'
tags: [Perception]
tags_color: '#e8ab02'
featured: true
---

The ReFlex TakkTile hand was a three-fingered robotic hand with distributed barometric sensors embedded along the fingers. The sensors could detect localized pressure when objects were being manipulated by the hand or when contact was made with the environment. We used this hand in our lab for several of our robot experiments. We had a desire to simulate these sensors in order to get more realistic testing done in simulation and help bridge the gap for sim-to-real learning. So I gave it a go!

## Approach

I was able to achieve this using only URDF and xacro macros, no other code! Have a look at the [reflex.urdf.xacro](https://bitbucket.org/robot-learning/ll4ma_robots_description/src/main/urdf/reflex/reflex.urdf.xacro) file for the complete implementation, this was an initial clone of the [URDF from RightHand Robotics](https://github.com/RightHandRobotics/reflex-ros-pkg/blob/master/reflex_visualizer/urdf/full_reflex_model.urdf.xacro) but _heavily_ refactored by yours truly to be easier to specify and support physics simulation in Gazebo (their model was only used for visualization purposes).   

### URDF Link Creation

The first step was to create links for each of the contact sensors. These would serve as visual representations of the sensors in Gazebo, but more importantly they provide distinct links to associate the contact sensing functionality to. It was essential that this was written as a macro, because each finger had 9 sensors on it, and there were three fingers -- adding 27 links without a macro wasn't an option! [This](https://bitbucket.org/robot-learning/ll4ma_robots_description/src/52cd3d8ce04847840e1a569b4fcc9aaa695b5ac4/urdf/reflex/reflex.urdf.xacro#lines-289:334) is the macro I came up with:

```xml
<!-- ======================================================================================= -->
<!--   TACTILE MACROS                                                                        -->
<!-- ======================================================================================= -->

<!-- MACRO touch sensor for contact/pressure sensor plugins -->
<xacro:macro name="touch_sensor" params="location finger_id sensor_id">
  <!-- Small sensor link for Gazebo sensor plugin to connect to. Collision/inertia values are
       bogus and visual defines approximation of sensor size. Mass/inertia need to be low
       enough not to interfere with hand dynamics but not so small the physics engine chokes
       on it (e.g. 1e-8 is too small and the links disappear). -->
  <link name="${hand_name}_${location}_${finger_id}_sensor_${sensor_id}">
    <visual>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <geometry>
        <box size="0.006 0.006 0.0003"/>
      </geometry>
    </visual>
    <inertial>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <mass value="1e-5"/>
      <xacro:cuboid_inertia m="1e-5" x="0.006" y="0.006" z="0.0003"/>
    </inertial>
    <collision>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <geometry>
        <box size="0.006 0.006 0.0003"/>
      </geometry>
    </collision>
  </link>
  <!-- Joint connecting sensor link to pad -->
  <joint name="${hand_name}_${location}_${finger_id}_to_sensor_${sensor_id}" type="fixed">
    <xacro:if value="${location == 'proximal'}">
      <xacro:property name="x_offset" value="0.012"/>
      <xacro:property name="x_gap"    value="0.008"/>
      <xacro:property name="z_offset" value="0.014"/>
    </xacro:if>
    <xacro:if value="${location == 'distal'}">
      <xacro:property name="x_offset" value="0.008"/>
      <xacro:property name="x_gap"    value="0.008"/>
      <xacro:property name="z_offset" value="0.01"/>
    </xacro:if>
    <origin xyz="${x_offset + x_gap * sensor_id} 0 ${z_offset}" rpy="0 0 0"/>
    <parent link="${location}_${finger_id}"/>
    <child link="${location}_${finger_id}_sensor_${sensor_id}"/>
  </joint>
</xacro:macro>
```

A few things of note here:
- The links _have_ to have some mass and inertia defined for them in order to take part in the physics simulation in Gazebo. Some care had to be taken to specify these, since if the values were too large then they would impact the dynamics of the hand during grasping, and if they were too small then the simulator would have numerical issues and refuse to even render the links! I opted for some small made up values that enabled the links to be rendered while not impacting the dynamics in a noticeable way.
- Each finger had a proximal and distal component, loosely mimicking the structure of a human finger. You'll notice in the `joint` block there is a switch on whether the sensor link is to be attached to the proximal or distal finger link, and it sets some appropriate XYZ position offsets for them respectively. These offsets were manually measured with a caliper in the lab and/or fussed around with to get the spacing right.

The macro was utilized within the finger macro [here](https://bitbucket.org/robot-learning/ll4ma_robots_description/src/52cd3d8ce04847840e1a569b4fcc9aaa695b5ac4/urdf/reflex/reflex.urdf.xacro#lines-430:441) to create links for all of the sensors and incorporate them into the kinematic chain:

```xml
<!-- Include contact/pressure sensors if specified -->
<xacro:if value="${sensor_type == 'contact' or sensor_type == 'pressure'}">
  <xacro:touch_sensor location="proximal" finger_id="${id}" sensor_id="1"/>
  <xacro:touch_sensor location="proximal" finger_id="${id}" sensor_id="2"/>
  <xacro:touch_sensor location="proximal" finger_id="${id}" sensor_id="3"/>
  <xacro:touch_sensor location="proximal" finger_id="${id}" sensor_id="4"/>
  <xacro:touch_sensor location="proximal" finger_id="${id}" sensor_id="5"/>
  <xacro:touch_sensor location="distal"   finger_id="${id}" sensor_id="1"/>
  <xacro:touch_sensor location="distal"   finger_id="${id}" sensor_id="2"/>
  <xacro:touch_sensor location="distal"   finger_id="${id}" sensor_id="3"/>
  <xacro:touch_sensor location="distal"   finger_id="${id}" sensor_id="4"/>
</xacro:if>
```

When `xacro` is executed to generate the final URDF, if the `sensor_type` arg is specified as either `contact` or `pressure` then it will include these sensor links! Note the two different modes, where `pressure` is a continuous value of pressure and `contact` is just 1 (contact) or 0 (no-contact). These two modes are supported on the real hardware, so we wanted them also in simulation. Luckily, Gazebo already had support of these kinds of sensors!

## Contact/Pressure Sensing in Gazebo

In order to get the actual sensing capabilities associated with the sensor links, I utilized the pressure and contact sensing plugins provided by Gazebo which is simple to specify:
```xml
<!-- MACRO touch sensor plugin for Gazebo -->
<xacro:macro name="gazebo_touch_sensor" params="type location finger_id sensor_id">
  <gazebo reference="${hand_name}_${location}_${finger_id}_sensor_${sensor_id}">
    <xacro:property
        name="sensor_name"
    value="reflex/${hand_name}_${location}_${finger_id}_${type}_sensor_${sensor_id}"/>
    <sensor name="${sensor_name}" type="contact">
  <plugin name="${sensor_name}_plugin" filename="lib${type}_sensor_plugin.so">	
    <updateRate>100</updateRate> <!-- In range [0, 1000] -->
  </plugin>
  <contact>
    <collision>${sensor_name}_collision</collision> <!-- Auto-determined by SDF -->
  </contact>
    </sensor>
    <material>Gazebo/GreenTransparent</material>
  </gazebo>
</xacro:macro>
```

This macro was utilized within the `gazebo_finger` macro in order to incorporate the sensor capabilities into the final generated URDF. 

## Demo

I created a simple demo where I would randomly spawn cubes into a simulation scene where they were setup to drop onto the ReFlex hand and hit the contact sensors. You could then see the various contact sensor values echoed to the terminal as the objects bounces around on the different sensors. 

<p><iframe src="https://www.youtube.com/embed/oEuwmzLlE84?si=7t2awd7ltj4Bu13F" loading="lazy" frameborder="0" allowfullscreen></iframe></p>

## Limitations

This was an initial pass at simulating the sensors in Gazebo and was not without its limitations. The main one is that it wasn't well-calibrated to reality, in the sense that the pressure values you would get in the simulation did not match very closely to what the real sensors were reporting. This was largely a matter of introducing a scaling factor into the readings in simulation, but it would have required implementing a custom sensor plugin which was more effort than I could afford at the time. We ultimately wanted to do some machine learning to learn a sensor model for our real robot hand that we could plug into the simulation so it would _really_ be similar to the real thing. However, that was an ambitious task that I never found the time to take on.

We ultimately didn't do much with the simulation since a bigger limitation was that the real hand was underactuated and flexed at the joints, while ours was modeled as rigid bodies connected by simple rotary joints. The dynamics was too different from the real deal. In any event, adding the contact sensing was great fun, and it was a joy seeing them respond in a semi-realistic way.
