---
layout: page
title: News
permalink: /news/
---

The latest news related to the Clockwork platform:

<ul>
  {% for post in site.posts %}
  {% if post.categories contains "news" %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endif %}
  {% endfor %}
</ul>