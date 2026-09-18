---
layout: page
title: Time-optimal trajectory planning for a robot arm
description: Solving a constrained time-optimal control problem fast enough to run in real time
importance: 4
category: engineering
---

**Main contributor**, Carnegie Mellon University · January to June 2023 · Advisor: Prof. Zac Manchester

Industrial robot arms should move as fast as possible without violating limits on velocity, acceleration, and jerk. This is a time-optimal trajectory planning (TOTP) problem, and it is hard to solve quickly.

- Reformulated the problem by switching the constraint domain, which makes the constraints easier to handle.
- Split the problem into several computation cycles so it can be solved in real time.
- Developed a control strategy based on domain mapping that satisfies third-order and other constraints.
