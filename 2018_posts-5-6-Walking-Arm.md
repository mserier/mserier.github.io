---
layout: post
title: NEAT Walking Arm
categories: [blog, project]
tags: [machine learning, c++, java, arduino, hardware]
published: true
---

WIP

The walking arm was one of my first machine learning projects. Because of this I tried to design the machine in such way that the learning is as easy as possible for the algorithm. I chose to base my algorithm on NEAT.

[Explanation of NEAT]

---

Because of its simplicity and direct connections between the system (the walking arm and external inputs) and the network it is easy to look at an active network and  comprehent what it is doing and if it's healthy.

[Insert example here (maybe Sethblings mario)]

---

With having the algorithm work on a real life machine there are some difficulties that are trivial in a simulated environment. Such as:
* resetting the machine before each test
* making the tests consistant
* determining an accurate fitness (how well the machine is doing)
* actually testing a new evolution of the algorithm (you can only test one at the time)

[insert diagram of machine here]

I designed this machine around the parts that I simply already had laying around. The parts that I used where:
* Stepper motor for resetting the arm
* Switch for tracking the distance travelled of the arm (for the fitness)
* 2 Servos for the arm
* 2x Servo u brackets for the arm
* Spool of wire for resetting the arm
* Some mdf wood to make a stable base

The estimated costs for the parts that I used are around $45,00 but this could be way lower since the servos and stepper motor are way overpowered.

[insert photo of machine here]

[insert video of machine resetting here]

---

This is how the program looks without any learning. You can see the inputs, nodes and outputs without any connections. The amount of nodes is arbitrary and can increase (or decrease) when the program is evolving.
...
