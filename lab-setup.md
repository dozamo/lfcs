---
layout: default
title: Lab Environment Setup
nav_order: 2
has_children: true
#permalink: /lab-setup/
---

# Lab Environment Setup

This section details the process of setting up the virtual lab environment...



## Guides in this Section

<div class="child-page-cards">
{%- for page in site.pages -%}
  {%- if page.parent == "Lab Environment Setup" -%}
    <div class="child-card">
      <h3><a href="{{ page.url | relative_url }}">{{ page.title }}</a></h3>
      {%- if page.description -%}
        <p>{{ page.description }}</p>
      {%- endif -%}
    </div>
  {%- endif -%}
{%- endfor -%}
</div>
