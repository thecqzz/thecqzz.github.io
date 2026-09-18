---
layout: page
title: other projects
permalink: /projects/
description: Course, competition, internship, and side projects.
nav: true
nav_order: 3
---

<!-- pages/projects.md -->
<div class="projects">
{% assign sorted_projects = site.projects | where: "category", "engineering" | sort: "importance" %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>
