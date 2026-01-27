---
layout: muzyka
icon: fas fa-tags
order: 5
layout: page
---

{% assign songs = site.categories.piosenki | sort: "date" | reverse %}

{% for post in songs %}
  {% include post-preview.html post=post %}
{% endfor %}
