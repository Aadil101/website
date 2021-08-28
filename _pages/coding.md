---
layout: archive
permalink: /coding/
title: "Coding"
author_profile: true
---

Here, you can learn about some of my research interests and personal projects 📚 You'll probably notice a bunch of natural language processing tasks scattered about here, which is a field in AI/ML that I'm especially passionate about!

<ul class="taxonomy__index">
  {% assign postsInYear = site.categories.coding | where_exp: "item", "item.hidden != true" | group_by_exp: 'post', 'post.date | date: "%Y"' %}
  {% for year in postsInYear %}
    <li>
      <a href="#{{ year.name }}">
        <strong>{{ year.name }}</strong> <span class="taxonomy__count">{{ year.items | size }}</span>
      </a>
    </li>
  {% endfor %}
</ul>

{% assign entries_layout = page.entries_layout | default: 'list' %}
{% assign postsByYear = site.categories.coding | where_exp: "item", "item.hidden != true" | group_by_exp: 'post', 'post.date | date: "%Y"' %}
{% for year in postsByYear %}
  <section id="{{ year.name }}" class="taxonomy__section">
    <h2 class="archive__subtitle">{{ year.name }}</h2>
    <div class="entries-{{ entries_layout }}">
      {% for post in year.items %}
        {% include archive-single.html type=entries_layout %}
      {% endfor %}
    </div>
  </section>
{% endfor %}