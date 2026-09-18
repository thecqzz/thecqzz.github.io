---
layout: page
title: Autonomous rescue boat
description: A boat that finds heat and sound sources on its own
img: assets/img/projects/rescue_boat.jpg
importance: 5
category: engineering
---

**Main contributor**, University of California, Irvine · December 2021 to March 2022 · Advisor: Prof. Camilo Velez Cuervo

In a water rescue, the boat should go to the person, not the other way around. We designed and built a boat that navigates by itself to heat and sound sources, which stand in for people in the water.

- Buoyant chassis with infrared heat detectors and microphones.
- State-machine decision logic in C++ that tells heat and sound signatures apart and prioritizes the most urgent target.
- Won the best mechanical design award in the final competition.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/rescue_boat.jpg" title="Rescue boat in the pool" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/rescue_boat_build.jpg" title="Rescue boat before the test" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: the boat during the pool test. Right: the boat and its sensor mast before the test.
</div>
