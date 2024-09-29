---
layout: default
---

{{ site.url }} - Site URL<br>
{{ site.baseurl }} - Site Baseurl<br>
{{ post.url }} - Post URL<br>
{{ post.id }} - Post ID<br>
{{ post.path }} - Post Path<br>
{{ post.relative_path }} - Post Relative Path<br>
{{ post.relative_url }} - Post Relative URL<br>
{{ post.url | relative_url }} - Post URL with relative_url filter<br>
{{ post.url | absolute_url }} - Post URL with absolute_url filter<br>


{% for post in site.posts %}
<h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
  <p>{{ post.excerpt }}</p>
{% endfor %}