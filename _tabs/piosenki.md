---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<div style="position: sticky; top: 1rem; z-index: 1000; background: var(--main-bg); padding-bottom: 10px;">
  <input type="text" id="song-search" placeholder="🔍 Szukaj piosenki..." autocomplete="off" 
    style="width: 100%; padding: 12px 20px; border: 2px solid var(--link-color); border-radius: 25px; background: var(--main-bg); color: var(--text-color); outline: none;"
    oninput="
      var val = this.value.toLowerCase().trim();
      var items = document.querySelectorAll('.song-item');
      var heads = document.querySelectorAll('.letter-header');
      var noRes = document.getElementById('no-results');
      var stats = document.getElementById('search-stats');
      var found = 0;

      items.forEach(function(item) {
        var match = item.innerText.toLowerCase().includes(val);
        item.style.setProperty('display', match ? 'block' : 'none', 'important');
        if (match) found++;
      });

      heads.forEach(function(h) {
        var hasVisible = false;
        var next = h.nextElementSibling;
        while (next && next.classList.contains('song-item')) {
          if (next.style.display !== 'none') { hasVisible = true; break; }
          next = next.nextElementSibling;
        }
        h.style.setProperty('display', hasVisible ? 'block' : 'none', 'important');
      });

      if (val.length > 0) {
        stats.style.display = 'block';
        stats.querySelector('span').innerText = found;
        noRes.style.display = (found === 0) ? 'block' : 'none';
      } else {
        stats.style.display = 'none';
        noRes.style.display = 'none';
        items.forEach(function(i) { i.style.display = 'block'; });
        heads.forEach(function(h) { h.style.display = 'block'; });
      }
    ">
  
  <div id="search-stats" style="display:none; font-size: 0.85rem; color: var(--link-color); font-weight: bold; margin: 8px 0 0 15px;">
    Znaleziono: <span>0</span>
  </div>
</div>

<div id="songs-list">
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

<div id="no-results" style="display:none; text-align: center; padding: 40px 20px; border: 1px dashed var(--main-border-color); border-radius: 15px; margin-top: 20px;">
  <p style="font-size: 1.1rem; color: var(--text-muted);">Nie znaleziono piosenek pasujących do zapytania. 🔍</p>
  <button onclick="var i=document.getElementById('song-search'); i.value=''; i.dispatchEvent(new Event('input'));" 
          style="background: none; border: 1px solid var(--link-color); color: var(--link-color); padding: 5px 15px; border-radius: 15px; cursor: pointer; font-size: 0.9rem;">
    Pokaż wszystkie
  </button>
</div>
