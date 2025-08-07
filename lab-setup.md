---
title: Lab Environment Setup
permalink: /lab-setup/
---

# Lab Environment Setup

This section details the process of setting up the virtual lab environment...

## Guides in this Section

<div class="feature__row">
{% for item in site.data.navigation.main %}
  {% if item.title == "Lab Environment Setup" %}
    {% for child in item.children %}
      <div class="archive__item">
        <h3 class="archive__item-title">
          <a href="{{ child.url | relative_url }}" rel="permalink">{{ child.title }}</a>
        </h3>
        <!-- Para la descripción, tendrías que añadirla también en _data/navigation.yml -->
        <!-- <p class="archive__item-excerpt">{{ child.description }}</p> -->
      </div>
    {% endfor %}
  {% endif %}
{% endfor %}
</div>
