---
layout: post
title: Planning under Uncertainty to Goal Distributions
description: New formalization of goals as distributions for robot planning under uncertainty.
date: 2022-04-29
image: '/images/goal_distributions/goal_distributions_cover.png'
video_embed: 'https://www.youtube.com/embed/wAG2wvpesjU?si=pLj_Bw41PZdMpxXT'
tags: [Robot Learning]
tags_color: '#4287f5'
featured: true
---

This was a paper I worked on during my Ph.D. I did not get it published, but it is up on [arXiv](https://arxiv.org/abs/2011.04782) and served as a culminating piece of my Ph.D. dissertation.

***

## Abstract

Goals for planning problems are typically conceived of as subsets of the state space. However, for many practical planning problems in robotics, we expect the robot to predict goals, e.g. from noisy sensors or by generalizing learned models to novel contexts. In these cases, sets with uncertainty naturally extend to probability distributions. While a few works have used probability distributions as goals for planning, surprisingly no systematic treatment of planning to goal distributions exists in the literature. This article serves to fill that gap. We argue that goal distributions are a more appropriate goal representation than deterministic sets for many robotics applications. We present a novel approach to planning under uncertainty to goal distributions, which we use to highlight several advantages of the goal distribution formulation. We build on previous results in the literature by formally framing our approach as an instance of planning as inference. We additionally derive reductions of several common planning objectives as special cases of our probabilistic planning framework. Our experiments demonstrate the flexibility of probability distributions as a goal representation on a variety of problems including planar navigation among obstacles, intercepting a moving target, rolling a ball to a target location, and a 7-DOF robot arm reaching to grasp an object.


Please see the [arXiv paper](https://arxiv.org/abs/2011.04782) for details on the research.

```
@article{conkey2020planning,
  title={Planning under Uncertainty to Goal Distributions},
  author={Conkey, Adam and Hermans, Tucker},
  journal={arXiv preprint arXiv:2011.04782},
  year={2020},
  url={https://arxiv.org/abs/2011.04782}
}
```

***

## Software Development

