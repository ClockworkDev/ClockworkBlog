---
layout: page
title: Tutorials
permalink: /tutorials/
---

These are some tutorials you can follow to learn how to create your own awesome games!

<ul>
  {% for post in site.posts %}
  {% if post.categories contains "tutorial" %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endif %}
  {% endfor %}
</ul>