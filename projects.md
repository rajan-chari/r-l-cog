---
layout: default
title: Projects
permalink: /projects/
---

<div id="home">
  <h1>Projects</h1>
  <ul class="posts">
    {% assign sorted = site.projects | sort: 'date' | reverse %}
    {% for proj in sorted %}
    <li>
      {% if proj.date %}<span class="date">{{ proj.date | date: "%Y" }}</span>{% endif %}
      &raquo;
      <a href="{{ proj.url | relative_url }}">{{ proj.title }}</a>
      {% if proj.tagline %} &mdash; <span style="color:#666;">{{ proj.tagline }}</span>{% endif %}
    </li>
    {% endfor %}
  </ul>
</div>
