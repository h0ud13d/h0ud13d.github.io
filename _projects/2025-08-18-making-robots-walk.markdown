---
layout: post
title: "Making Robots Walk"
date: 2025-08-18 14:14:04 -0400
---

3.5 weeks before my fall semester starts, and I had no fun things to work on. So I figured I would teach myself RL and make some humanoid robots walk. *(I was heavily inspired/guided by @jsuarez5341 on X, specifically [this](https://x.com/jsuarez5341/status/1943692998975402064) article)*

Started with Webots simulation. Set up a NAO model and trained a PPO policy for locomotion. Pretty standard stuff! Reward shaping for forward movement, balance, energy efficiency. The sim worked fine after some hyperparameter tuning.

![Training walking in Webots](../../images/nao_room.png "Training walking in Webots")


Deployed the policy to the actual NAO using NAOqi. Walking worked, turning worked. But there was this annoying problem where the sim-trained policy would work great for maybe 30 seconds, then start drifting and eventually face-plant.

Spent way too long thinking it was a domain randomization issue. Tried adding noise to the sim, different floor friction coefficients, nothing helped. Then I realized the problem was simpler; my policy was relying too heavily on perfect joint position control, but real servos have backlash and drift.

Fixed it by implementing kinematics-based balance control that didn't rely on the RL policy for stability. Used the joint angle feedback to estimate center of mass position and detect when the robot was about to fall. Added a simple PID controller that would microadjust the hip and ankle angles to keep the CoM over the support polygon.

Works pretty well now. The NAO can walk around for minutes without falling over, handles direction changes smoothly. Not the most elegant solution but it gets the job done without needing additional sensors.
