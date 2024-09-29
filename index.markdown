---
layout: default
---

{% for post in site.posts %}
<h2><a href="{{ post.url | absolute_url }}">{{ post.title }}</a></h2>
  <p>{{ post.excerpt }}</p>
{% endfor %}