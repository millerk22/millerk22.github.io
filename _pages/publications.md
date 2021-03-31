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

{% for post in site.publications reversed  %}
  {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
  {% capture next_year %}{{ post.previous.date | date: "%Y" }}{% endcapture %}

  {% if forloop.first %}
  <h2 id="{{this_year}}">{{this_year}}</h2>
  <ul class="publications">
  {% endif %}

  
  {% include publication-item.html %}
  

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


<h2 id="preprints">Preprints</h2>
{% for post in site.preprints reversed  %}
  <ul>
  {% include publication-item.html %}
  </ul>
{% endfor %}

