---
layout: home
title: Home
---

Welcome to my blog!

{% for post in site.posts limit:10 %}
<article class="post-preview">
  <h2 class="post-title">
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
  </h2>
  <p class="post-meta">
    {{ post.date | date: "%B %d, %Y" }}
  </p>
  <div class="post-entry">
    {{ post.excerpt }}
  </div>
  <a href="{{ post.url | relative_url }}" class="read-more">Continue Reading →</a>
</article>
{% endfor %}

<div class="clearfix">
  <a class="btn btn-primary float-right" href="{{ '/blog' | relative_url }}">View All Posts &rarr;</a>
</div>
