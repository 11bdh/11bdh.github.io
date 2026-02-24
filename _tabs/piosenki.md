---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<style>
  /* Gwarantowane ukrywanie piosenek i liter */
  .hide-me { display: none !important; }
  
  #song-search {
    width: 100%; padding: 12px 20px; border: 2px solid var(--link-color);
    border-radius: 25px; background: var(--main-bg); color: var(--text-color);
    outline: none; box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  }

  #desert-box {
    display: none; margin-top: 20px; border-radius: 15px; 
    overflow: hidden; position: relative; min-height: 400px;
  }

  #desert-img {
    position: absolute; top: 0; left: 0; width: 100%; height: 100%;
    background-size: cover; background-position: center; z-index: 1;
  }

  .desert-overlay {
    position: relative; z-index: 2; padding: 60px 20px; text-align: center;
    color: white !important; text-shadow: 2px 2px 10px rgba(0,0,0,0.9);
    background: rgba(0,0,0,0.4); min-height: 400px;
    display: flex; flex-direction: column; justify-content: center; align-items: center;
  }
</style>

<div style="position: sticky; top: 1rem; z-index: 1000; background: var(--main-bg); padding-bottom: 15px;">
  <input type="text" id="song-search" placeholder="🔍 Szukaj piosenki..." autocomplete="off" 
         oninput="
           const query = this.value.toLowerCase().trim();
           const songs = document.querySelectorAll('.s-item');
           const heads = document.querySelectorAll('.s-head');
           const box = document.getElementById('desert-box');
           const imgDiv = document.getElementById('desert-img');
           const stats = document.getElementById('s-stats');
           let foundCount = 0;

           // 1. Filtruj piosenki
           songs.forEach(s => {
             const match = s.innerText.toLowerCase().includes(query);
             s.classList.toggle('hide-me', !match);
             if(match) foundCount++;
           });

           // 2. Filtruj litery (A, B, C...)
           heads.forEach(h => {
             let hasVisible = false;
             let next = h.nextElementSibling;
             while (next && next.classList.contains('s-item')) {
               if (!next.classList.contains('hide-me')) { hasVisible = true; break; }
               next = next.nextElementSibling;
             }
             h.classList.toggle('hide-me', !hasVisible);
           });

           // 3. Pokazuj pustynię jeśli nic nie znaleziono
           if (query.length > 0) {
             stats.style.display = 'block';
             stats.innerText = 'Znaleziono: ' + foundCount;
             if (foundCount === 0) {
               const isDark = document.documentElement.getAttribute('data-mode') === 'dark';
               // Pobieramy ścieżki zapisane wcześniej w atrybutach
               const imgUrl = isDark ? imgDiv.getAttribute('data-night') : imgDiv.getAttribute('data-day');
               imgDiv.style.backgroundImage = 'url(' + imgUrl + ')';
               box.style.display = 'block';
             } else { box.style.display = 'none'; }
           } else {
             stats.style.display = 'none';
             box.style.display = 'none';
             songs.forEach(s => s.classList.remove('hide-me'));
             heads.forEach(h => h.classList.remove('hide-me'));
           }
         ">
  <div id="s-stats" style="display:none; font-size: 0.85rem; color: var(--link-color); font-weight: bold; margin: 8px 0 0 15px;"></div>
</div>

{% assign sorted_songs = site.piosenki | sort: "title" %}

<div id="main-song-list">
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

<div id="desert-box">
  <div id="desert-img" 
       data-day="{{ '/assets/zdjecia/pustynia-dzien.png' | relative_url }}" 
       data-night="{{ '/assets/zdjecia/pustynia-noc.png' | relative_url }}">
  </div>
  <div class="desert-overlay">
    <h3 style="color: white; margin-bottom: 10px; font-size: 1.8rem;">Pusto tutaj... 🌵</h3>
    <p style="color: white;">Nie znaleźliśmy takiej piosenki w śpiewniku.</p>
    <button onclick="const i=document.getElementById('song-search'); i.value=''; i.dispatchEvent(new Event('input'));" 
            style="margin-top: 20px; padding: 12px 35px; border-radius: 25px; border: none; background: #007bff; color: white; font-weight: bold; cursor: pointer;">
      Wyczyść szukanie
    </button>
  </div>
</div>
