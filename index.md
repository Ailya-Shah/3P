---
layout: default
title: Home
---

<section class="hero" data-reveal>
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
    <a href="{{ '/blog/' | relative_url }}">Read the writing</a>
    <a href="{{ '/projects/' | relative_url }}">See projects</a>
    <a href="https://github.com/Ailya-Shah">GitHub</a>
    <a href="{{ '/about/' | relative_url }}">About</a>
  </div>
</section>

<section class="section" data-reveal>
  <h2>Featured writing</h2>
  <ul class="post-list">
    {% for post in site.posts limit:3 %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
        <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
        {% if post.excerpt %}
          <p class="post-excerpt">{{ post.excerpt | strip_html | truncate: 160 }}</p>
        {% endif %}
        {% if post.disc %}<span class="chip chip--{{ post.disc }}">{{ post.disc }}</span>{% endif %}
      </li>
    {% endfor %}
  </ul>
  {% if site.posts.size > 3 %}
  <p style="margin-top:1.5rem;font-family:var(--sans);font-size:0.9rem;font-weight:500">
    <a href="{{ '/blog/' | relative_url }}" style="text-decoration:none;color:var(--physics-ink)">All writing →</a>
  </p>
  {% endif %}
</section>

<section class="section intro-grid" data-reveal>
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
