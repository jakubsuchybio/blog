---
layout: default
---




{% for post in site.posts %}
<h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
<p>{{ site.url }} - Site URL<br></p>
<p>{{ site.baseurl }} - Site Baseurl<br></p>
<p>{{ post.url }} - Post URL<br></p>
<p>{{ post.id }} - Post ID<br></p>
<p>{{ post.path }} - Post Path<br></p>
<p>{{ post.relative_path }} - Post Relative Path<br></p>
<p>{{ post.relative_url }} - Post Relative URL<br></p>
<p>{{ post.url | relative_url }} - Post URL with relative_url filter<br></p>
<p>{{ post.url | absolute_url }} - Post URL with absolute_url filter<br></p>
  <p>{{ post.excerpt }}</p>
{% endfor %}