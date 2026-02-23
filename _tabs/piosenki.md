---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<style>
  .search-hidden { display: none !important; }
  #song-search {
    width: 100%; padding: 12px 20px; border: 2px solid var(--link-color);
    border-radius: 25px; background: var(--main-bg); color: var(--text-color);
    outline: none; box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  }
  #no-results-bg {
    position: absolute; top: 0; left: 0; width: 100%; height: 100%; 
    background-size: cover; background-position: center; 
    transition: background-image 0.3s ease-in-out;
  }
</style>

<div id="search-container" style="margin-bottom: 2rem; position: sticky; top: 1rem; z-index: 1000; background: var(--main-bg);">
  <input type="text" id="song-search" placeholder="🔍 Szukaj piosenki..." autocomplete="off" 
         oninput="
           const val = this.value.toLowerCase().trim();
           const items = document.querySelectorAll('.song-item');
           const letters = document.querySelectorAll('.letter-group');
           const desert = document.getElementById('no-results');
           const stats = document.getElementById('search-stats');
           let found = 0;

           items.forEach(item => {
             const text = item.innerText.toLowerCase();
             if (text.includes(val)) {
               item.classList.remove('search-hidden');
               found++;
             } else {
               item.classList.add('search-hidden');
             }
           });

           letters.forEach(group => {
             let visible = false;
             let next = group.nextElementSibling;
             while (next && next.classList.contains('song-item')) {
               if (!next.classList.contains('search-hidden')) { visible = true; break; }
               next = next.nextElementSibling;
             }
             group.classList.toggle('search-hidden', !visible);
           });

           if (val.length > 0) {
             stats.style.display = 'block';
             stats.innerText = 'Znaleziono: ' + found;
             if (found === 0) {
               const dark = document.documentElement.getAttribute('data-mode') === 'dark';
               const img = dark ? 'pustynia-noc.png' : 'pustynia-dzien.png';
               const path = '{{ '/assets/zdjecia/' | relative_url }}' + img;
               document.getElementById('no-results-bg').style.backgroundImage = 'url(\'' + path + '\')';
               desert.style.display = 'block';
             } else { desert.style.display = 'none'; }
           } else {
             stats.style.display = 'none';
             desert.style.display = 'none';
           }
         ">
  <div id="search-stats" style="display:none; font-size: 0.85rem; color: var(--link-color); margin: 8px 0 0 15px; font-weight: bold;"></div>
</div>

{% assign sorted_songs = site.piosenki | sort: "title" %}

<div id="song-list-wrapper">
  {% assign current_letter = "" %}
  {% for song in sorted_songs %}
    {% capture first_letter %}{{ song.title | slice: 0 | upcase }}{% endcapture %}
    {% if first_letter != current_letter %}
      <h2 class="letter-group" style="padding-top: 1.5rem; border-bottom: 1px solid var(--main-border-color); margin-bottom: 1rem;">{{ first_letter }}</h2>
      {% assign current_letter = first_letter %}
    {% endif %}
    <div class="song-item" style="margin-bottom: 0.5rem;">
      <a href="{{ song.url | relative_url }}" style="font-weight: 600; text-decoration: none; display: block; padding: 5px 0;">{{ song.title }}</a>
    </div>
  {% endfor %}
</div>

<div id="no-results" style="display:none; margin-top: 20px; border-radius: 15px; overflow: hidden; position: relative; min-height: 400px; box-shadow: 0 10px 30px rgba(0,0,0,0.2);">
  <div id="no-results-bg"></div>
  <div style="position: relative; z-index: 1; padding: 80px 20px; text-align: center; color: white; text-shadow: 2px 2px 15px rgba(0,0,0,1); background: rgba(0,0,0,0.1); height: 100%; display: flex; flex-direction: column; justify-content: center; align-items: center;">
    <h3 style="margin-bottom: 10px; font-size: 2rem;">Pusto tutaj... 🌵</h3>
    <p style="font-weight: 500;">Nie znaleźliśmy takiej piosenki w naszym śpiewniku.</p>
    <button onclick="const s = document.getElementById('song-search'); s.value=''; s.dispatchEvent(new Event('input'));" 
            style="margin-top: 25px; padding: 12px 35px; border-radius: 25px; border: none; background: #007bff; color: white; font-weight: bold; cursor: pointer; transition: transform 0.2s;">
      Wyczyść szukanie
    </button>
  </div>
</div>
