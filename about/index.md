---
layout: default
title:  "About Me"
date:   2015-07-08
category: about
---

<section>
  My name is André "Andy" Onuki and I am a Java web programmer. I am not
  a good designer, as you can see through this page, but I know my way
  around HTML, Javascript, CSS and some of the related frameworks.
</section>

{% assign items = site.data.about.places %}
{% include items.html %}
