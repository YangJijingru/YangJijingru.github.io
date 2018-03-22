---
layout: page
title: archive
header-img: "img/twitter.jpg"
description: "时光是一支开弓后的箭，只向前，不后退"
---

### Blogs
<hr>

{% for post in site.posts %}
<div class="post-preview">

  <font color="#4078c0">{{ post.date | date: "%B %-d, %Y" }} -> &nbsp;&nbsp;
  <a color="#4078c0" target="_blank" href="{{ post.url | prepend: site.baseurl }}">  {{ post.title }}
  </a>
  </font>

</div>
<hr>
{% endfor %}

{% for post in site.posts %}
    {% assign year = post.date | date: '%m' %}
    {% assign nyear = post.next.date | date: '%m' %}
    {% if year != nyear %}
## {{ post.date | date: '%m' }}
{:.archive-title}
    {% endif %}
  <font color="#4078c0">{{ post.date | date: "%B %-d, %Y" }} -> &nbsp;&nbsp;
  <a color="#4078c0" target="_blank" href="{{ post.url | prepend: site.baseurl }}">  {{ post.title }}
  </a>
  </font>
{% endfor %}
