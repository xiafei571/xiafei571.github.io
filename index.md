---
layout: home
title: Latest Posts
---

{% if site.posts.size > 0 %}
  <ul class="post-list">
    {% for post in site.posts limit:5 %}
      <li>
        <span class="post-meta">{{ post.date | date: "%B %-d, %Y" }}</span>
        <h3>
          <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
        </h3>
        {{ post.excerpt }}
      </li>
    {% endfor %}
  </ul>
{% endif %}
