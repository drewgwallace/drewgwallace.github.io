---
layout: page
title: About
permalink: /about/
---


{% for job in site.resume.experience %}
<div class="resume-item" itemscope itemprop="worksFor" itemtype="http://schema.org/Organization">
  <h3 class="resume-item-title" itemprop="name">{{ job.company }}</h3>
  <h4 class="resume-item-details" itemprop="description">{{ job.position }} &bull; {{ job.duration }}</h4>
  <p class="resume-item-copy">{{ job.summary }}</p>
</div><!-- end of resume-item -->
{% endfor %}