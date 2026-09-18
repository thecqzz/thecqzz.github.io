---
layout: page
title: Tilt-rotor VTOL control
description: One model predictive controller for hover, cruise, and everything in between
img: assets/img/projects/vtol.png
importance: 2
category: research
related_publications: true
---

**Qizhao Chen**, Ziqi Hu, Junyi Geng, Dongwei Bai, Mohammadreza Mousaei, and Sebastian Scherer · AirLab, Carnegie Mellon University · 2022 to 2024

[arXiv](https://arxiv.org/abs/2402.07375) · [AIAA SciTech 2024 (DOI)](https://doi.org/10.2514/6.2024-2878)

{% include figure.liquid loading="eager" path="assets/img/projects/vtol.png" title="Tilt-rotor VTOL" class="img-fluid rounded z-depth-1" %}

## The problem

A tilt-rotor VTOL aircraft takes off like a helicopter and flies like a plane. Most controllers treat these as separate modes and switch between them. The switch is a weak point: the aerodynamics change quickly, and a hard hand-off between controllers can cause unsafe behavior. This page summarizes our AIAA SciTech 2024 paper {% cite chen2024unified %}.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        <img src="{{ '/assets/img/projects/vtol_cad.gif' | relative_url }}" class="img-fluid rounded z-depth-1" alt="CAD model of the tilt-rotor eVTOL">
    </div>
</div>
<div class="caption">
    CAD model of the tilt-rotor eVTOL. Each of the four rotors tilts on its own.
</div>

## What I did

- Designed and modeled a tilt-rotor eVTOL with four independently tilting rotors, and simulated it in Gazebo and in a custom MATLAB simulator that matches the real vehicle.
- Developed a unified model predictive control (MPC) strategy with solver-based control allocation. One controller handles hover, transition, and forward flight, so no mode switching is needed.
- Studied the aerodynamics during transition and compared options such as controller blending and allocation adaptation.

## What we found

The unified MPC outperforms tuned PID controllers while keeping a single control law. The aircraft can accelerate and decelerate smoothly, fly precise coordinated turns, and keep flying after an actuator failure because each tilt is controlled independently.
