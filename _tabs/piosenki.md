---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<div style="position: sticky; top: 1rem; z-index: 1000; background: var(--main-bg); padding-bottom: 10px;">
  <input type="text" id="song-search" placeholder="🔍 Szukaj..." autocomplete="off" 
    style="width: 100%; padding: 12px 20px; border: 2px solid var(--link-color); border-radius: 25px; background: var(--main-bg); color: var(--text-color); outline: none;"
    oninput="
      var filter = this.value.toLowerCase().trim();
      var items = document.querySelectorAll('.song-item');
      var heads = document.querySelectorAll('.letter-header');
      
      items.forEach(function(item) {
        var text = item.innerText.toLowerCase();
        if (text.includes(filter)) {
          item.style.setProperty('display', 'block', 'important');
        } else {
          item.style.setProperty('display', 'none', 'important');
        }
      });

      heads.forEach(function(h) {
        var show = false;
        var next = h.nextElementSibling;
        while (next && next.classList.contains('song-item')) {
          if (next.style.display !== 'none') { show = true; break; }
          next = next.nextElementSibling;
        }
        h.style.setProperty('display', show ? 'block' : 'none', 'important');
      });
    ">
</div>

<div id="songs-wrapper">
  {% assign sorted = site.piosenki | sort: "title" %}
  {% assign current_l = "" %}
  {% for song in sorted %}
    {% capture first_l %}{{ song.title | slice: 0 | upcase }}{% endcapture %}
    {% if first_l != current_l %}
      <h2 class="letter-header" style="margin-top: 1.5rem; border-bottom: 1px solid var(--main-border-color);">{{ first_l }}</h2>
      {% assign current_l = first_l %}
    {% endif %}
    <div class="song-item" style="padding: 5px 0;">
      <a href="{{ song.url | relative_url }}" style="font-weight: 600; text-decoration: none;">{{ song.title }}</a>
    </div>
  {% endfor %}
</div>
