---
layout: page
title: Contact
permalink: /contact/
---

<p class="post-excerpt">The quickest way to reach me is through one of the profiles below.</p>

<ul class="link-list">
  {% for item in site.social %}
    <li><a href="{{ item.url }}">{{ item.name }}</a></li>
  {% endfor %}
  <li><a href="mailto:{{ site.author.email }}">Email</a></li>
  <li><a href="https://substack.com/@ailyaa">Substack</a></li>
</ul>
