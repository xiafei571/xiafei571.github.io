## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/xiafei571/xiafei571.github.io/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/xiafei571/xiafei571.github.io/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.

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
