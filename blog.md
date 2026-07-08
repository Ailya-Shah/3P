---
layout: default
title: Writing
permalink: /blog/
---

<section class="hero" data-reveal style="margin-bottom:2.5rem">
  <p class="eyebrow brand">Writing</p>
  <h1 style="font-size:clamp(2.4rem,7vw,4rem);font-weight:600;letter-spacing:-0.03em;line-height:1;margin:0 0 1rem">
    Notes &amp; write-ups
  </h1>
  <p class="lede">
    Project breakdowns, learning logs, and essays sitting at the intersection of
    physics, psychology, and Python.
  </p>
</section>

<section class="section" data-reveal>
  {% assign sorted = site.posts | sort: "date" | reverse %}
{% assign posts_by_year = sorted | group_by_exp: "post", "post.date | date: '%Y'" %}
  {% for year in posts_by_year %}
    <h2>{{ year.name }}</h2>
    <ul class="post-list" style="margin-bottom:2.5rem">
      {% for post in year.items %}
        <li>
          <span class="post-meta">{{ post.date | date: "%b %-d" }}</span>
          <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
          {% if post.excerpt %}
            <p class="post-excerpt">{{ post.excerpt | strip_html | truncate: 150 }}</p>
          {% endif %}
          {% include chips.html disc=post.disc %}
        </li>
      {% endfor %}
    </ul>
  {% endfor %}
</section>
