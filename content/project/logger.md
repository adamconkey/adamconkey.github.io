+++
title = "Data Logging"
date = 2018-09-29T15:31:38-06:00
draft = false
weight=4

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = []

# Project summary to display on homepage.
summary = "Two data logging frameworks for recording robot and sensor data. One uses HDF5 files with an h5py interface, and the other post-processes rosbag data into Pandas dataframes."

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

I created a logging framework for recording robot data (e.g. robot joint states, force/torque sensor readings, end-effector pose, annotations on the data, etc.). I originally developed a C++ logger for [HDF5 format](https://support.hdfgroup.org/HDF5/whatishdf5.html) and a Python interface to the database using [H5py](https://www.h5py.org/). However, I started having issues with data corruption/synchronization and sometimes my data would get completely wiped out. I therefore switched to using [rosbag](http://wiki.ros.org/rosbag) and wrote a rosbag converter to extract the data into a [Pandas dataframe](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) and [pickle](https://docs.python.org/3/library/pickle.html) the resulting dataframe. Both implemementations are still in the repository since there's some useful code in the HDF5 version and it took a lot of effort on my end, but the rosbag/Pandas version is the one I actively use.


The code is hosted on the LL4MA Lab's bitbucket page [here](https://bitbucket.org/robot-learning/ll4ma_logger).