---
layout: default
title: Home
---

<section class="hero">
  <p class="eyebrow brand">3P</p>

  <h1>
    <span class="minor">Physics</span>
    <span class="minor">Psyche</span>
    <span class="major">Python</span>
  </h1>

  <p class="lede">
    Exploring data science, machine learning, psychology, physics, and the ideas
    that emerge when they intersect.
  </p>


  <div class="hero-links">
    <a href="/about/">About</a>
    <a href="/projects/">Projects</a>
    <a href="https://github.com/Ailya-Shah">GitHub</a>
    <a href="/contact/">Contact</a>
  </div>
</section>

<section class="section">
  <h2>Featured Writing</h2>
  <ul class="post-list">
    {% for post in site.posts %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
        <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
        {% if post.excerpt %}
          <p class="post-excerpt">{{ post.excerpt | strip_html | truncate: 160 }}</p>
        {% endif %}
      </li>
    {% endfor %}
  </ul>
</section>

<section class="section intro-grid">
  <div>
    <h2>What this site is</h2>
    <p class="post-excerpt">
      A digital notebook documenting my journey through data science,
      machine learning, psychology, physics, and programming.
    </p>
  </div>

  <div>
    <h2>What to expect</h2>
    <p class="post-excerpt">
      Technical notes, project write-ups, learning logs, essays,
      experiments, and occasional attempts to connect ideas across
      disciplines.
    </p>
  </div>
</section>
