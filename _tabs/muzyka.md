---
title: Muzyka
icon: fas fa-music
order: 5
layout: page
---

{% assign songs = site.categories.piosenki | sort: "date" | reverse %}

{% if songs and songs.size > 0 %}
  {% for post in songs %}
    {% include post-card.html post=post %}
  {% endfor %}
{% else %}
  <p>Brak dodanych piosenek.</p>
{% endif %}
