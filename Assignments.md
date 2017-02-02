---
layout: page
title: 698 Porfolio
date: 2017-01-17
categories:
---
# Weekly Reports
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
