---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<style>
  /* Klasa do ukrywania piosenek */
  .hidden-song { display: none !important; }
  
  #song-search {
    width: 100%; padding: 12px 20px; border: 2px solid var(--link-color);
    border-radius: 25px; background: var(--main-bg); color: var(--text-color);
    outline: none; box-shadow: 0 4px 10px rgba(0,0,0,0.1); margin-bottom: 15px;
  }

  /* Kontener na pustynię */
  #empty-view {
    display: none; border-radius: 15px; overflow: hidden; 
    position: relative; min-height: 400px; margin-top: 20px;
  }

  #empty-img {
    position: absolute; top: 0; left: 0; width: 100%; height: 100%;
    background-size: cover; background-position: center; z-index: 1;
  }

  .empty-txt {
    position: relative; z-index: 2; background: rgba(0,0,0,0.4);
    height: 400px; display: flex; flex-direction: column;
    justify-content: center; align-items: center; text-align: center;
    color: white !important; text-shadow: 2px 2px 10px rgba(0,0,0,0.8);
    padding: 20px;
  }
</style>

<div style="position: sticky; top: 1rem; z-index: 1000; background: var(--main-bg); padding-bottom: 10px;">
  <input type="text" id="song-search" placeholder="🔍 Szukaj piosenki..." 
    oninput="
      const q = this.value.toLowerCase().trim();
      const songs = document.querySelectorAll('.song-unit');
      const heads = document.querySelectorAll('.letter-head');
      const empty = document.getElementById('empty-view');
      const img = document.getElementById('empty-img');
      let found = 0;

      // 1. Filtrowanie piosenek
      songs.forEach(s => {
        const has = s.innerText.toLowerCase().includes(q);
        s.classList.toggle('hidden-song', !has);
        if(has) found++;
      });

      // 2. Filtrowanie nagłówków (A, B, C...)
      heads.forEach(h => {
        let showHead = false;
        let next = h.nextElementSibling;
        while(next && next.classList.contains('song-unit')) {
          if(!next.classList.contains('hidden-song')) { showHead = true; break; }
          next = next.nextElementSibling;
        }
        h.classList.toggle('hidden-song', !showHead);
      });

      // 3. Obsługa widoku pustyni
      if(q.length > 0 && found === 0) {
        const isDark = document.documentElement.getAttribute('data-mode') === 'dark';
        const pic = isDark ? img.dataset.night : img.dataset.day;
        img.style.backgroundImage = 'url(' + pic + ')';
        empty.style.display = 'block';
      } else {
        empty.style.display = 'none';
      }
    ">
</div>

{% assign sorted = site.piosenki | sort: "title" %}

<div id="list-container">
  {% assign current_l = "" %}
  {% for song in sorted %}
    {% capture first_l %}{{ song.title | slice: 0 | upcase }}{% endcapture %}
    {% if first_l != current_l %}
      <h2 class="letter-head" style="margin-top: 1.5rem; border-bottom: 1px solid var(--main-border-color);">{{ first_l }}</h2>
      {% assign current_l = first_l %}
    {% endif %}
    <div class="song-unit" style="padding: 5px 0;">
      <a href="{{ song.url | relative_url }}" style="font-weight: 600; text-decoration: none;">{{ song.title }}</a>
    </div>
  {% endfor %}
</div>

<div id="empty-view">
  <div id="empty-img" 
       data-day="{{ '/assets/zdjecia/pustynia-dzien.png' | relative_url }}" 
       data-night="{{ '/assets/zdjecia/pustynia-noc.png' | relative_url }}">
  </div>
  <div class="empty-txt">
    <h3 style="color: white;">Pusto tutaj... 🌵</h3>
    <p>Nie znaleźliśmy piosenki o tej nazwie.</p>
    <button onclick="const i=document.getElementById('song-search'); i.value=''; i.dispatchEvent(new Event('input'));" 
            style="margin-top: 15px; padding: 10px 30px; border-radius: 20px; border: none; background: #007bff; color: white; cursor: pointer; font-weight: bold;">
      Wyczyść szukanie
    </button>
  </div>
</div>
