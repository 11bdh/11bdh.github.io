---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<style>
  /* Gwarancja ukrycia */
  .search-off { display: none !important; }
  
  #song-search {
    width: 100%; padding: 12px 20px; border: 2px solid var(--link-color);
    border-radius: 25px; background: var(--main-bg); color: var(--text-color);
    outline: none; box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  }

  .desert-box {
    display: none; margin-top: 20px; border-radius: 15px; 
    overflow: hidden; position: relative; min-height: 400px;
  }

  .desert-overlay {
    position: relative; z-index: 2; padding: 60px 20px; text-align: center;
    color: white !important; text-shadow: 2px 2px 10px rgba(0,0,0,0.9);
    background: rgba(0,0,0,0.4); min-height: 400px;
    display: flex; flex-direction: column; justify-content: center; align-items: center;
  }

  #desert-img {
    position: absolute; top: 0; left: 0; width: 100%; height: 100%;
    background-size: cover; background-position: center; z-index: 1;
  }
</style>

<div style="position: sticky; top: 1rem; z-index: 1000; background: var(--main-bg); padding-bottom: 15px;">
  <input type="text" id="song-search" placeholder="🔍 Szukaj piosenki..." autocomplete="off" 
         oninput="
           const val = this.value.toLowerCase().trim();
           const items = document.querySelectorAll('.s-item');
           const headers = document.querySelectorAll('.s-head');
           const desert = document.getElementById('desert-container');
           const stats = document.getElementById('s-stats');
           let count = 0;

           items.forEach(el => {
             const match = el.innerText.toLowerCase().includes(val);
             el.classList.toggle('search-off', !match);
             if(match) count++;
           });

           headers.forEach(h => {
             let hasVisible = false;
             let next = h.nextElementSibling;
             while (next && next.classList.contains('s-item')) {
               if (!next.classList.contains('search-off')) { hasVisible = true; break; }
               next = next.nextElementSibling;
             }
             h.classList.toggle('search-off', !hasVisible);
           });

           if (val.length > 0) {
             stats.style.display = 'block';
             stats.innerText = 'Znaleziono: ' + count;
             if (count === 0) {
               const dark = document.documentElement.getAttribute('data-mode') === 'dark';
               const img = dark ? 'pustynia-noc.png' : 'pustynia-dzien.png';
               document.getElementById('desert-img').style.backgroundImage = 'url({{ /assets/zdjecia/ | relative_url }}' + img + ')';
               desert.style.display = 'block';
             } else { desert.style.display = 'none'; }
           } else {
             stats.style.display = 'none';
             desert.style.display = 'none';
             items.forEach(el => el.classList.remove('search-off'));
             headers.forEach(h => h.classList.remove('search-off'));
           }
         ">
  <div id="s-stats" style="display:none; font-size: 0.85rem; color: var(--link-color); font-weight: bold; margin: 8px 0 0 15px;"></div>
</div>

{% assign sorted_songs = site.piosenki | sort: "title" %}

<div id="song-list">
  {% assign current_letter = "" %}
  {% for song in sorted_songs %}
    {% capture first_letter %}{{ song.title | slice: 0 | upcase }}{% endcapture %}
    {% if first_letter != current_letter %}
      <h2 class="s-head" style="padding-top: 1.5rem; border-bottom: 1px solid var(--main-border-color); margin-bottom: 1rem;">{{ first_letter }}</h2>
      {% assign current_letter = first_letter %}
    {% endif %}
    <div class="s-item" style="margin-bottom: 0.5rem;">
      <a href="{{ song.url | relative_url }}" style="font-weight: 600; text-decoration: none; display: block; padding: 5px 0;">{{ song.title }}</a>
    </div>
  {% endfor %}
</div>

<div id="desert-container" class="desert-box">
  <div id="desert-img"></div>
  <div class="desert-overlay">
    <h3 style="color: white; margin-bottom: 10px; font-size: 1.8rem;">Pusto tutaj... 🌵</h3>
    <p style="color: white;">Spróbuj wpisać inną nazwę piosenki.</p>
    <button onclick="const i=document.getElementById('song-search'); i.value=''; i.dispatchEvent(new Event('input'));" 
            style="margin-top: 20px; padding: 10px 25px; border-radius: 20px; border: none; background: #007bff; color: white; font-weight: bold; cursor: pointer;">
      Wyczyść szukanie
    </button>
  </div>
</div>
