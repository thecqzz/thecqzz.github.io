---
layout: page
title: CPNAV
description: Calibrated vision-language navigation for finding objects in places a robot has never seen
img: assets/img/projects/cpnav/cpnav.png
importance: 1
category: research
related_publications: true
---

**Qizhao Chen**, [Yuanhong Zeng](https://practice-lab-ucla.github.io/people/yuanhong_zeng/), [Shoh Nishino](https://practice-lab-ucla.github.io/people/Shoh_Nishino/), and [Anushri Dixit](https://practice-lab-ucla.github.io/people/anushri_dixit/) · UCLA PRACTICE Lab · 2026

[Paper (PDF)]({{ '/assets/pdf/chen2026cpnav.pdf' | relative_url }})

{% include figure.liquid loading="eager" path="assets/img/projects/cpnav/cpnav.png" title="CPNAV overview" class="img-fluid rounded z-depth-1" %}

<div class="caption">
    CPNAV in one picture. (a) A VLM scores collision-free actions. (b) Conformal prediction keeps only the actions the robot can trust. (c) A search tree with backtracking explores them. (d) The robot we used.
</div>

## The problem

Asking a robot to find an object, say a plant, in a home it has never seen is a core challenge in embodied AI. Vision-language models (VLMs) understand scenes well, but their action choices are often overconfident and not calibrated. A robot that trusts every VLM answer will sometimes walk confidently in the wrong direction. This page summarizes our paper {% cite chen2026cpnav %}.

## How CPNAV works

CPNAV keeps the reasoning power of VLMs and adds a statistical layer that tells the robot which actions it can trust. It has three parts.

### 1. Propose safe actions and score them with VLMs

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/cpnav/action_proposal.png" title="Action proposal" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    From one RGB-D image, CPNAV builds a navigability map, proposes collision-free motion primitives, and draws them on the camera image. Green is newly discovered space and blue is space the robot already explored.
</div>

Two VLMs look at the image. A stopping VLM decides whether the goal object is in view and rates how promising the scene is. An action VLM gives each drawn action a score. The robot also keeps a voxel map of explored space so it prefers new areas.

<div class="row justify-content-sm-center">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/cpnav/vlm_prompts.png" title="VLM prompts" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The prompts for the stopping VLM and the action VLM, and an example answer.
</div>

### 2. Calibrate the scores with conformal prediction

Raw VLM scores are not reliable probabilities. Offline, we run a tree-structured exploration with backtracking on calibration episodes to find the oracle action at each step, and we record the score the VLM gave it. Conformal prediction turns these scores into a threshold with a finite-sample guarantee: at a risk level the user picks, the correct action is kept with at least that probability. Online, actions below the threshold are dropped. The layer is model-agnostic, so it can wrap any VLM policy.

### 3. Explore with a search tree and backtrack

The remaining actions are explored in a search tree. Each node is a place the robot visited and each edge is an action. If a branch does not lead to the goal, the robot goes back to the parent node and tries the next trusted action instead of wandering.

## Results

### Simulation

We test on two benchmarks in AI Habitat, HM3D v0.2 and MP3D, with 200 episodes each and a 200-step budget. We compare against the same pipeline without calibration, three heuristic ways of building the action set, and two recent VLM navigation methods.

| Policy         | HM3D SR (%) | HM3D SPL (%) | HM3D distance (m) | MP3D SR (%) | MP3D SPL (%) | MP3D distance (m) |
| -------------- | ----------: | -----------: | ----------------: | ----------: | -----------: | ----------------: |
| No Calibration |        81.0 |         40.3 |              35.4 |        68.5 |         43.0 |              36.4 |
| Simple Set     |        76.7 |         39.5 |              32.8 |        71.0 |         44.3 |              33.4 |
| Prompt Set     |        79.7 |         38.7 |              37.4 |        71.5 |         45.3 |              31.6 |
| Ensemble Set   |        74.0 |         39.1 |              28.0 |        66.5 |         41.8 |              28.2 |
| STRIVE         |        82.5 |         41.5 |              17.6 |        71.5 |         42.3 |              21.0 |
| WMNav          |        80.0 |         39.9 |              18.2 |        75.0 |         45.2 |              18.5 |
| **CPNAV**      |    **86.0** |     **44.9** |              29.2 |    **77.0** |     **46.8** |              32.1 |

Compared with no calibration, CPNAV raises success rate by 6.2% and SPL by 11.4% on HM3D, and by 12.4% and 8.8% on MP3D, while cutting travel distance on HM3D by 17.5%. It also beats the best heuristic threshold. CPNAV has slightly higher success and SPL than STRIVE and WMNav but travels farther, because full backtracking makes sure every trusted branch is eventually explored.

The advantage grows when the robot has fewer steps:

| Step budget      | No Calibration SR / SPL (%) | CPNAV SR / SPL (%) |
| ---------------- | --------------------------: | -----------------: |
| 50 steps (25%)   |                 60.3 / 37.7 |    **73.0 / 43.2** |
| 100 steps (50%)  |                 75.7 / 39.8 |    **80.0 / 44.4** |
| 200 steps (100%) |                 81.0 / 40.3 |    **86.0 / 44.9** |

### Real robot

We deployed CPNAV on a modified Hiwonder MentorPi M1 robot with a ZED 2i stereo camera, a Mecanum drivetrain, and SLAM Toolbox for state estimation. We ran 24 randomized object-goal episodes in two apartments.

| Policy         |   SR (%) |  SPL (%) | Mean distance (m) |
| -------------- | -------: | -------: | ----------------: |
| No Calibration |     66.7 |     42.0 |               9.0 |
| **CPNAV**      | **83.3** | **48.4** |           **6.3** |

Success rate went up by 24.9% and SPL by 15.2%, and the robot traveled 30.0% less distance.

{% include video.liquid path="assets/video/cpnav_hardware_demo.mp4" class="img-fluid rounded z-depth-1" controls=true %}

<div class="caption">
    Hardware demo comparing the uncalibrated baseline and CPNAV.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ '/assets/img/projects/cpnav/baseline_hardware.gif' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Baseline hardware rollout">
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <img src="{{ '/assets/img/projects/cpnav/cpnav_hardware.gif' | relative_url }}" class="img-fluid rounded z-depth-1" alt="CPNAV hardware rollout">
    </div>
</div>
<div class="caption">
    Left: the baseline commits to unreliable branches and runs out of steps. Right: CPNAV filters low-confidence actions and reaches the goal in 14 steps.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/cpnav/baseline_search_tree.png" title="Baseline search tree" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/cpnav/cpnav_search_tree.png" title="CPNAV search tree" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The same two runs as search trees. The baseline explores a wide, diffuse tree. CPNAV explores fewer branches and moves more directly toward the goal.
</div>

### What matters most

We changed one setting at a time from the default CPNAV setup (GPT-5o-nano, camera height 0.9 m, 131° horizontal field of view, voxel map on) and measured the effect on HM3D.

| Variant              |   SR (%) |  SPL (%) | Mean distance (m) |
| -------------------- | -------: | -------: | ----------------: |
| **CPNAV (default)**  | **86.0** | **44.9** |          **29.2** |
| No Calibration       |     81.0 |     40.3 |              35.4 |
| Camera height 0.45 m |     67.0 |     31.4 |              44.6 |
| Camera height 1.70 m |     79.3 |     40.5 |              30.4 |
| Field of view 69°    |     40.0 |     18.7 |              44.8 |
| Field of view 101°   |     72.7 |     30.5 |              40.9 |
| Model GPT-4o-mini    |     71.3 |     33.7 |              34.4 |
| No voxel map         |     66.0 |     36.4 |              36.5 |

A low camera or a narrow field of view hurts the most, because the robot sees less of the room. A smaller VLM also hurts, so calibration works best with a strong model. The voxel map helps by preventing repeated visits.

### Cost

Calibration does not add cost. On MP3D, CPNAV uses 12.9% fewer VLM tokens per episode than the uncalibrated baseline (201.6K versus 231.4K), and about 40 simulation episodes can run at once on a single GPU. On the robot, everything runs onboard without a separate compute server.
