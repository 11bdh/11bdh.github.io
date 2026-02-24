---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<style>
  /* Klasa wymuszająca ukrycie z najwyższym priorytetem */
  .s-hide { display: none !important; }
  
  #song-search {
    width: 100%; padding: 12px 20px; border: 2px solid var(--link-color);
    border-radius: 25px; background: var(--main-bg); color: var(--text-color);
    outline: none; box-shadow: 0 4px 10px rgba(0,0,0,0.1); margin-bottom: 15px;
  }

  #desert-box {
    display: none; border-radius: 15px; overflow: hidden; 
    position: relative; min-height: 400px; margin-top: 20px;
    background-size: cover; background-position: center;
  }

  .desert-overlay {
    background: rgba(0,0,0,0.4); min-height: 400px;
    display: flex; flex-direction: column; justify-content: center;
    align-items: center; text-align: center; color: white !important;
    text-shadow: 2px 2px 10px rgba(0,0,0,0.8); padding: 20px;
  }
</style>

<div style="position: sticky; top: 1rem; z-index: 1000; background: var(--main-bg); padding-bottom: 10px;">
  <input type="text" id="song-search" placeholder="🔍 Szukaj piosenki..." autocomplete="off">
  <div id="search-stats" style="display:none; font-size: 0.85rem; color: var(--link-color); font-weight: bold; margin-left: 15px; margin-top: 5px;"></div>
</div>

{% assign sorted = site.piosenki | sort: "title" %}

<div id="songs-list">
  {% assign current_l = "" %}
  {% for song in sorted %}
    {% capture first_l %}{{ song.title | slice: 0 | upcase }}{% endcapture %}
    {% if first_l != current_l %}
      <h2 class="s-header" style="margin-top: 1.5rem; border-bottom: 1px solid var(--main-border-color);">{{ first_l }}</h2>
      {% assign current_l = first_l %}
    {% endif %}
    <div class="s-item" style="padding: 5px 0;">
      <a href="{{ song.url | relative_url }}" style="font-weight: 600; text-decoration: none;">{{ song.title }}</a>
    </div>
  {% endfor %}
</div>

<div id="desert-box" 
     data-day="{{ '/assets/zdjecia/pustynia-dzien.png' | relative_url }}" 
     data-night="{{ '/assets/zdjecia/pustynia-noc.png' | relative_url }}">
  <div class="desert-overlay">
    <h3 style="color: white; font-size: 1.8rem;">Pusto tutaj... 🌵</h3>
    <p style="color: white;">Nie znaleźliśmy takiej piosenki w naszym śpiewniku.</p>
    <button id="clear-search-btn" style="margin-top: 15px; padding: 10px 30px; border-radius: 20px; border: none; background: #007bff; color: white; cursor: pointer; font-weight: bold;">
      Wyczyść szukanie
    </button>
  </div>
</div>

<script>
(function() {
  function applySongFilter() {
    const input = document.getElementById('song-search');
    if (!input) return;

    const val = input.value.toLowerCase().trim();
    const items = document.querySelectorAll('.s-item');
    const heads = document.querySelectorAll('.s-header');
    const desert = document.getElementById('desert-box');
    const stats = document.getElementById('search-stats');
    let found = 0;

    // Filtrowanie piosenek
    items.forEach(item => {
      const match = item.innerText.toLowerCase().includes(val);
      item.classList.toggle('s-hide', !match);
      if (match) found++;
    });

    // Ukrywanie nagłówków liter (A, B, C...)
    heads.forEach(h => {
      let hasVisible = false;
      let next = h.nextElementSibling;
      while (next && next.classList.contains('s-item')) {
        if (!next.classList.contains('s-hide')) { hasVisible = true; break; }
        next = next.nextElementSibling;
      }
      h.classList.toggle('s-hide', !hasVisible);
    });

    // Obsługa pustyni i statystyk
    if (val.length > 0) {
      stats.style.display = 'block';
      stats.innerText = 'Znaleziono: ' + found;
      if (found === 0) {
        const isDark = document.documentElement.getAttribute('data-mode') === 'dark';
        const bgUrl = isDark ? desert.dataset.night : desert.dataset.day;
        desert.style.backgroundImage = 'url(' + bgUrl + ')';
        desert.style.display = 'block';
      } else {
        desert.style.display = 'none';
      }
    } else {
      stats.style.display = 'none';
      desert.style.display = 'none';
      items.forEach(i => i.classList.remove('s-hide'));
      heads.forEach(h => h.classList.remove('s-hide'));
    }
  }

  // GLOBALNE NASŁUCHIWANIE (rozwiązanie problemu Pjax w Chirpy)
  document.addEventListener('input', function(e) {
    if (e.target && e.target.id === 'song-search') {
      applySongFilter();
    }
  });

  document.addEventListener('click', function(e) {
    if (e.target && e.target.id === 'clear-search-btn') {
      const input = document.getElementById('song-search');
      if (input) {
        input.value = '';
        applySongFilter();
      }
    }
  });

  // Inicjalizacja przy pierwszym załadowaniu
  applySongFilter();
})();
</script>
