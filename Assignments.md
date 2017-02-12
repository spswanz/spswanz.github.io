---
layout: page
title: 698 Porfolio
date: 2017-01-17
categories:
---
# Check out my weekly reading responses here:
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
