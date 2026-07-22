---
layout: default
title: 作品一覧
---

<h1>作品一覧</h1>

<ul>
{% assign order = site.data.works.order %}

{% for item in order %}
  {% assign work = site.works | where: "slug", item | first %}
  {% if work %}
    <li>
      <a href="{{ work.url | relative_url }}">{{ work.title }}</a>
      ({{ work.year }})
    </li>
  {% endif %}
{% endfor %}
</ul>