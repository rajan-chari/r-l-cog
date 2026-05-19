---
layout: default
title: YouTube
permalink: /youtube/
---

<div id="home">
  <h1>YouTube</h1>
  <ul class="posts">
    {% assign sorted = site.youtube | sort: 'date' | reverse %}
    {% for vid in sorted %}
    <li>
      {% if vid.date %}<span class="date">{{ vid.date | date: "%d %b %Y" }}</span>{% endif %}
      &raquo;
      <a href="{{ vid.url | relative_url }}">{{ vid.title }}</a>
    </li>
    {% endfor %}
  </ul>
</div>
