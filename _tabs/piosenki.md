---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

{% assign sorted_songs = site.piosenki | sort: "title" %}

<div class="pl-1 pr-1">
  {% assign current_letter = "" %}
  {% for song in sorted_songs %}
    {% capture first_letter %}{{ song.title | slice: 0 | upcase }}{% endcapture %}
    {% if first_letter != current_letter %}
      <h2 class="pt-4 border-bottom">{{ first_letter }}</h2>
      {% assign current_letter = first_letter %}
    {% endif %}
    <div class="mb-2">
      <a href="{{ song.url | relative_url }}">{{ song.title }}</a>
    </div>
  {% endfor %}
</div>
