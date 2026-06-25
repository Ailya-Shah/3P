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

{%- assign featured = site.posts | where: "featured", true | first -%}
{%- if featured -%}
<section class="section" data-reveal>
  <a class="featured-post" href="{{ featured.url | relative_url }}">
    <p class="featured-eyebrow">Featured</p>
    <h2 class="featured-title">{{ featured.title }}</h2>
    <p class="featured-lede">{{ featured.excerpt | strip_html | truncate: 220 }}</p>
    <div class="featured-foot">
      <span class="featured-meta">{{ featured.date | date: "%b %-d, %Y" }} · {{ featured.reading | default: "5 min read" }}</span>
      <span class="featured-cta">Read the vision →</span>
    </div>
  </a>
</section>
{%- endif -%}

<section class="section" data-reveal>
  <h2>Latest writing</h2>
  <ul class="post-list">
    {% assign count = 0 %}
    {% for post in site.posts %}
      {% unless post.featured %}
        {% if count < 4 %}
          <li>
            <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
            <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
            {% if post.excerpt %}
              <p class="post-excerpt">{{ post.excerpt | strip_html | truncate: 160 }}</p>
            {% endif %}
            {% include chips.html disc=post.disc %}
          </li>
          {% assign count = count | plus: 1 %}
        {% endif %}
      {% endunless %}
    {% endfor %}
  </ul>
  <p style="margin-top:1.5rem;font-family:var(--sans);font-size:0.9rem;font-weight:500">
    <a href="{{ '/blog/' | relative_url }}" style="text-decoration:none;color:var(--maroon-soft)">All writing →</a>
  </p>
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
      Project write-ups in plain language, learning logs, essays, and
      occasional attempts to connect ideas across disciplines.
    </p>
  </div>
</section>
