---
layout: default
title: "PeerLLM Blog"
---

<div class="hero">
  <h1>PeerLLM Blog</h1>
  <p>Exploring the future of decentralized AI networks and living intelligence.</p>
</div>

<div class="blog-list">
  {% for post in site.posts limit:5 %}
  <div class="post-item">
    <div class="post-title">
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </div>
    <div class="post-meta">
      {{ post.date | date: "%B %d, %Y" }}
    </div>
    <div class="post-excerpt">
      {{ post.excerpt }}
      <a href="{{ post.url | relative_url }}"> Read more →</a>
    </div>
  </div>
  {% endfor %}
</div>
