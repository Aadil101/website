---
layout: archive
permalink: /writing/
title: "Writing"
author_profile: true
---

Writing isn't my profession, but communication skills are obviously pretty important 📣 I'm thankful for towards my liberal arts education as it has made me a more confident writer and orator! Here's some of what I've been up to over the years:

<ul class="taxonomy__index">
  {% assign postsInYear = site.categories.writing | where_exp: "item", "item.hidden != true" | group_by_exp: 'post', 'post.date | date: "%Y"' %}
  {% for year in postsInYear %}
    <li>
      <a href="#{{ year.name }}">
        <strong>{{ year.name }}</strong> <span class="taxonomy__count">{{ year.items | size }}</span>
      </a>
    </li>
  {% endfor %}
</ul>

{% assign entries_layout = page.entries_layout | default: 'list' %}
{% assign postsByYear = site.categories.writing | where_exp: "item", "item.hidden != true" | group_by_exp: 'post', 'post.date | date: "%Y"' %}
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