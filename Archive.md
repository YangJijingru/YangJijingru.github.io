---
layout: page
title: archive
header-img: "img/arch.jpg"
description: "时光是一支开弓后的箭，只向前，不后退"
---

## Blogs

{% assign count = 1 %}
{% for post in site.posts reversed %}
    {% assign year = post.date | date: '%m' %}
    {% assign nyear = post.next.date | date: '%m' %}
    {% if year != nyear %}
        {% assign count = count | append: ', ' %}
        {% assign counts = counts | append: count %}
        {% assign count = 1 %}
    {% else %}
        {% assign count = count | plus: 1 %}
    {% endif %}
{% endfor %}

{% assign counts = counts | split: ', ' | reverse %}
{% assign i = 0 %}

{% for post in site.posts %}
    {% assign year = post.date | date: '%m' %}
    {% assign nyear = post.next.date | date: '%m' %}
    {% if year != nyear %}
## {{ post.date | date: '%B , %Y' }} ({{ counts[i] }})
<hr>
{:.archive-title}
        {% assign i = i | plus: 1 %}
    {% endif %}
<div class="post-preview">

  <font color="#4078c0">{{ post.date | date: "%B %-d, %Y" }} -> &nbsp;&nbsp;
  <a color="#4078c0" target="_blank" href="{{ post.url | prepend: site.baseurl }}">  {{ post.title }}
  </a>
  </font>

</div>
<hr>
{% endfor %}
