---
layout: midpoint
title:  "1234"
permalink: /1234/index.html
---

{% for item in site.apple %}
  <h2>{{ item.title }}</h2>
  <p>{{ item.description }}</p>
  <p><a href="{{ item.url }}">{{ item.title }}</a></p>
{% endfor %}