---
layout: page
title: Blog
permalink: /blog/
---

{% assign items = site.blog | sort: 'date' | reverse %}

{% if items.size == 0 %}
<p>No blogs yet.</p>
{% endif %}

{% for post in items %}
  <article style="margin-bottom:1.5rem;">
    <h3 style="margin-bottom:.25rem;"><a href="{{ post.url }}">{{ post.title }}</a></h3>
    <span class="date" style="display:block; opacity:.7; font-size:.9em;">{{ post.date | date: "%B %d, %Y" }}</span>
  </article>
{% endfor %}

