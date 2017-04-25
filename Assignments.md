---
layout: default
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

<div class="cv block" style="margin-top:40px"></div>
---

<!-- <a id="fn1"><sup>1</sup></a>  -->
This are in reverse date order because I'm still using the Jekyll posts feature to auto-generate this page.
It has something to do with Liquid.
