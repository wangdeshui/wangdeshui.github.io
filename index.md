---
layout: page
title: 上善若水
tagline: 厚德载物
---
{% include JB/setup %}

<ul class="tag_box inline" style="margin-bottom: 20px;">
  {% assign categories_list = site.categories %}
  {% include JB/categories_list %}
</ul>

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>




