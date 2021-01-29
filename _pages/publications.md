---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}


<h3>Year of Publication</h3>
<ul style="padding-left: 1em;">
{% for post in site.publications reversed  %}
  {% if post.type != "manuscript" %}
  {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
  {% capture next_year %}{{ post.next.date | date: "%Y" }}{% endcapture %}
  {% if this_year != next_year %}
<li style="display: inline; float:left; list-style-type: none; margin-right: 1em; margin-bottom: 0em;"><strong><a href="#{{this_year}}">{{this_year}}</a></strong></li>
  {% endif %}
  {% endif %}
{% endfor %}
<li style="display: inline; float:left; list-style-type: none; margin-right: 1em; margin-bottom: 0em;"><strong><a href="#preprints">preprints</a></strong></li>
</ul>
<div style="clear: both;"></div>

{% for post in site.publications reversed  %}
  {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
  {% capture next_year %}{{ post.previous.date | date: "%Y" }}{% endcapture %}

  {% if forloop.first %}
  <h2 id="{{this_year}}">{{this_year}}</h2>
  <ul class="publications">
  {% endif %}

  {% if post.type != "manuscript" %}
  {% include publication-item.html %}
  {% endif %}

  {% if forloop.last %}
  </ul>
  {% else %}
  {% if this_year != next_year %}
  </ul>
  <h2 id="{{next_year}}">{{next_year}}</h2>
  <ul>
  {% endif %}
  {% endif %}
{% endfor %}


<h2 id="preprints">preprints and workshops</h2>
{% for post in site.publications reversed  %}
  {% if post.type == "manuscript" %}


  <ul>
  {% include publication-item.html %}
  </ul>

  {% endif %}
{% endfor %}
