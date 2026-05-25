---
layout: page
title: All Posts
permalink: /posts/
---

<div class="post-filters" aria-label="Post filters">
  <div class="filter-group">
    <div class="filter-heading">Categories</div>
    <div class="filter-options">
      {% assign sorted_categories = site.categories | sort %}
      {% for category in sorted_categories %}
        {% assign category_name = category[0] %}
        <button class="filter-chip" type="button" data-filter-type="categories" data-filter-value="{{ category_name | escape }}">
          {{ category_name | escape }}
        </button>
      {% endfor %}
    </div>
  </div>

  <div class="filter-group">
    <div class="filter-heading">Tags</div>
    <div class="filter-options">
      {% assign sorted_tags = site.tags | sort %}
      {% for tag in sorted_tags %}
        {% assign tag_name = tag[0] %}
        <button class="filter-chip" type="button" data-filter-type="tags" data-filter-value="{{ tag_name | escape }}">
          {{ tag_name | escape }}
        </button>
      {% endfor %}
    </div>
  </div>

  <button class="filter-clear" type="button">Clear filters</button>
</div>

<ul class="post-list">
  {% for post in site.posts %}
    <li class="post-item" data-categories="{{ post.categories | join: ',' | escape }}" data-tags="{{ post.tags | join: ',' | escape }}">
      <span class="post-meta">{{ post.date | date: "%B %d, %Y" }}</span>
      <h3 class="post-title">
        <a class="post-link filter-preserving-link" href="{{ post.url | relative_url }}">
          {{ post.title | escape }}
        </a>
      </h3>
    </li>
  {% endfor %}
</ul>
<p class="no-posts-message" hidden>No posts match the selected filters.</p>

<style>
  .post-filters {
    border-bottom: 1px solid #c9d8ea;
    margin-bottom: 1.25rem;
    padding-bottom: 1rem;
  }
  .filter-group {
    margin-bottom: 0.75rem;
  }
  .filter-heading {
    color: #2c3e50;
    font-size: 0.9rem;
    font-weight: 600;
    margin-bottom: 0.4rem;
  }
  .filter-options {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }
  .filter-chip,
  .filter-clear {
    background: #fff;
    border: 1px solid #9eb6d5;
    border-radius: 6px;
    color: #1e3a8a;
    cursor: pointer;
    font: inherit;
    font-size: 0.875rem;
    line-height: 1.2;
    padding: 0.35rem 0.6rem;
  }
  .filter-chip:hover,
  .filter-clear:hover {
    border-color: #1e3a8a;
  }
  .filter-chip.is-active {
    background: #1e3a8a;
    border-color: #1e3a8a;
    color: #fff;
  }
  .filter-clear {
    color: #555;
    margin-top: 0.25rem;
  }
  .post-list {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }
  .post-list .post-item {
    margin-bottom: 1rem;
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    position: relative;
    padding-left: 1.5em;
  }
  .post-list .post-item[hidden] {
    display: none;
  }
  .post-list .post-item::before {
    content: "•";
    position: absolute;
    left: 0;
    color: #1e3a8a;
    font-size: 1.5em;
    line-height: 1;
  }
  .post-list .post-meta {
    color: #666;
    font-size: 0.875rem;
    order: 2;
    white-space: nowrap;
  }
  .post-list .post-title {
    font-size: 1.1rem;
    margin: 0 1rem 0 0;
    order: 1;
    flex-grow: 1;
    font-weight: normal;
  }
  .post-link {
    color: #1e3a8a;
    text-decoration: none;
  }
  .post-link:hover {
    text-decoration: underline;
  }
  .no-posts-message {
    color: #666;
    font-size: 0.95rem;
  }
</style>

<script>
  document.addEventListener('DOMContentLoaded', () => {
    const selected = {
      categories: new Set(),
      tags: new Set()
    };
    const chips = Array.from(document.querySelectorAll('.filter-chip'));
    const posts = Array.from(document.querySelectorAll('.post-item'));
    const clearButton = document.querySelector('.filter-clear');
    const noPostsMessage = document.querySelector('.no-posts-message');
    const filterPreservingLinks = Array.from(document.querySelectorAll('.filter-preserving-link'));

    const splitValues = (value) => value.split(',').map((item) => item.trim()).filter(Boolean);
    const hasOverlap = (postValues, selectedValues) => selectedValues.length === 0 || selectedValues.some((value) => postValues.includes(value));

    const applyFilters = (updateUrl = true) => {
      chips.forEach((chip) => {
        const values = selected[chip.dataset.filterType];
        chip.classList.toggle('is-active', values.has(chip.dataset.filterValue));
      });

      let visibleCount = 0;
      const selectedCategories = Array.from(selected.categories);
      const selectedTags = Array.from(selected.tags);

      posts.forEach((post) => {
        const postCategories = splitValues(post.dataset.categories || '');
        const postTags = splitValues(post.dataset.tags || '');
        const isVisible = hasOverlap(postCategories, selectedCategories) && hasOverlap(postTags, selectedTags);
        post.hidden = !isVisible;
        if (isVisible) {
          visibleCount += 1;
        }
      });

      if (noPostsMessage) {
        noPostsMessage.hidden = visibleCount > 0;
      }

      filterPreservingLinks.forEach((link) => {
        const linkUrl = new URL(link.getAttribute('href'), window.location.origin);
        if (selectedCategories.length > 0) {
          linkUrl.searchParams.set('categories', selectedCategories.join(','));
        } else {
          linkUrl.searchParams.delete('categories');
        }
        if (selectedTags.length > 0) {
          linkUrl.searchParams.set('tags', selectedTags.join(','));
        } else {
          linkUrl.searchParams.delete('tags');
        }
        link.href = `${linkUrl.pathname}${linkUrl.search}${linkUrl.hash}`;
      });

      if (updateUrl) {
        const url = new URL(window.location.href);
        if (selectedCategories.length > 0) {
          url.searchParams.set('categories', selectedCategories.join(','));
        } else {
          url.searchParams.delete('categories');
        }
        if (selectedTags.length > 0) {
          url.searchParams.set('tags', selectedTags.join(','));
        } else {
          url.searchParams.delete('tags');
        }
        window.history.pushState({}, '', url);
      }
    };

    const loadFromUrl = () => {
      const params = new URLSearchParams(window.location.search);
      selected.categories = new Set(splitValues(params.get('categories') || ''));
      selected.tags = new Set(splitValues(params.get('tags') || ''));
      applyFilters(false);
    };

    chips.forEach((chip) => {
      chip.addEventListener('click', () => {
        const values = selected[chip.dataset.filterType];
        const value = chip.dataset.filterValue;
        if (values.has(value)) {
          values.delete(value);
        } else {
          values.add(value);
        }
        applyFilters();
      });
    });

    if (clearButton) {
      clearButton.addEventListener('click', () => {
        selected.categories.clear();
        selected.tags.clear();
        applyFilters();
      });
    }

    window.addEventListener('popstate', loadFromUrl);
    loadFromUrl();
  });
</script>
