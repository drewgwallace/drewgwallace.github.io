---
layout: nav
title:  "1234"
permalink: /1234/
---

abcd 

{% assign page_depth = page.url | split: '/' | size %}{% assign page_parent = page.url | split: '/' | last %}{% for node in site.pages offset:1 %}{% if node.url == '/' %}{{ continue }}{% else %}{% assign split_path = node.url | split: "/" %}{% assign node_last = split_path | last %}{% assign node_parent = node.url | remove: node_last | split: '/' | last %}{% assign node_url = node.url %}{% for slug in split_path offset:1 %}{% assign slug = slug %}{% assign slug_depth = forloop.index %}{% endfor %}{% if slug_depth == page_depth and page_parent == node_parent %}<li><a href="{{ node_url }}">{{ slug }}</a></li>{% endif %}{% if slug_depth == 1 and page.url == '/' and slug != 'search.json' and slug != 'sitemap.xml' %}<li><a href="{{ node_url }}">{{{slug}}</a></li>{% endif %}{% endif %}{% endfor %}