---
layout: default
---

<div>
  <ul class="listing">
  {% for post in site.posts %}
  <article class="content">
    <section class="title">
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
    </section>
    <section class="meta">
    <span class="time">
      <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
    </span>
    {% if post.tags %}
    <span class="tags">
      {% for tag in post.tags %}
      <a href="/tags.html#{{ tag }}" title="{{ tag }}">#{{ tag }}</a>
      {% endfor %}
    </span>
    {% endif %}
    </section>
    <section class="post">
    {{ post.excerpt | markdownify }}
    </section>
    </article>
    <div class="divider"></div>
  {% endfor %}
  </ul>
  
    <a href="/archive.html">Check the archive</a>
</div>
