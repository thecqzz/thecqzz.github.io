---
layout: page
title: robotics research
permalink: /research/
description: Research projects in safe robot navigation and control.
nav: true
nav_order: 2
---

<!-- pages/research.md -->
<div class="projects">
{% assign sorted_projects = site.projects | where: "category", "research" | sort: "importance" %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>
