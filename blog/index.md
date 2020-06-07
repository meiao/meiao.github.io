---
layout: blog
title:  "Blog"
date:   2020-05-31
category: blog
---

<section>
  Finally a page where each piece is not in a stupid fake window \o/.
</section>

<section class="post-list">
  {% for post in site.posts %}
  <div class="post-list-item">
    <header>
    <a href="{{post["id"]}}">{{post["title"]}}</a>
    <span class="category">[{{post["categories"] | join: ", " | capitalize}}]</span>
    <span class="date">{{ post["date"] | date_to_string}}</span>
    </header>
    <section class="excerpt">
      {{post["excerpt"]}}
    </section>
  </div>
  {% endfor %}
</section>
