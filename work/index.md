---
layout: default
title:  "Professional Experience"
date:   2015-06-17
category: work
---

{% assign work_places = site.data.work.places %}

{% for place in work_places %}
  <div id="{{place.id}}" class="panel panel-primary">
    <header class="panel-heading">
      <span class="pull-right">
        <button class="btn btn-danger disabled">
          <span class="glyphicon glyphicon-remove"></span>
        </button>
      </span>
      <h2>{{place.name}}</h2>
    </header>
    <div class="panel-body">
      {{place.description}}
    </div>
  </div>
{% endfor %}
