---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<style>
  /* Wymuszamy ukrywanie elementów, gdy skrypt nada klasę 'hidden-song' */
  .hidden-song {
    display: none !important;
  }
  #song-search {
    width: 100%;
    padding: 12px 20px;
    border: 2px solid var(--link-color);
    border-radius: 25px;
    background: var(--main-bg);
    color: var(--text-color);
    outline: none;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  }
</style>

<div id="search-container" style="margin-bottom: 2rem; position: sticky; top: 1rem; z-index: 1000; background: var(--main-bg);">
  <input type="text" id="song-search" placeholder="🔍 Szukaj piosenki..." autocomplete="off">
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

<div id="no-results" style="display:none; margin-top: 20px; border-radius: 15px; overflow: hidden; position: relative; min-height: 300px;">
  <div id="no-results-bg" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; background-size: cover; background-position: center;"></div>
  <div style="position: relative; z-index: 1; padding: 60px 20px; text-align: center; color: white; text-shadow: 2px 2px 10px rgba(0,0,0,0.9); background: rgba(0,0,0,0.2); height: 100%; display: flex; flex-direction: column; justify-content: center; align-items: center;">
    <h3 style="margin-bottom: 10px;">Pusto tutaj...</h3>
    <p>Nie znaleźliśmy takiej piosenki. Spróbuj wpisać coś innego! 🌵</p>
    <button id="clear-search-btn" style="margin-top: 20px; padding: 10px 25px; border-radius: 20px; border: none; background: var(--link-color); color: white; font-weight: bold; cursor: pointer;">
      Wyczyść szukanie
    </button>
  </div>
</div>

<script>
  function applySearch() {
    const searchInput = document.getElementById('song-search');
    const songItems = document.querySelectorAll('.song-item');
    const letters = document.querySelectorAll('.letter-group');
    const stats = document.getElementById('search-stats');
    const noResults = document.getElementById('no-results');
    const noResultsBg = document.getElementById('no-results-bg');

    if (!searchInput) return;

    searchInput.addEventListener('input', function() {
      const val = this.value.toLowerCase().trim();
      let count = 0;

      // 1. Filtrujemy piosenki
      songItems.forEach(item => {
        const text = item.innerText.toLowerCase();
        if (text.includes(val)) {
          item.classList.remove('hidden-song');
          count++;
        } else {
          item.classList.add('hidden-song');
        }
      });

      // 2. Filtrujemy litery alfabetu
      letters.forEach(letter => {
        let hasVisible = false;
        let next = letter.nextElementSibling;
        while (next && next.classList.contains('song-item')) {
          if (!next.classList.contains('hidden-song')) {
            hasVisible = true;
            break;
          }
          next = next.nextElementSibling;
        }
        if (hasVisible) letter.classList.remove('hidden-song');
        else letter.classList.add('hidden-song');
      });

      // 3. Obsługa pustyni i statystyk
      if (val === '') {
        stats.style.display = 'none';
        noResults.style.display = 'none';
      } else {
        stats.style.display = 'block';
        stats.innerText = 'Znaleziono: ' + count;
        
        if (count === 0) {
          const mode = document.documentElement.getAttribute('data-mode');
          const img = mode === 'dark' ? 'pustynia-noc.png' : 'pustynia-dzien.png';
          noResultsBg.style.backgroundImage = "url('{{ '/assets/zdjecia/' | relative_url }}" + img + "')";
          noResults.style.display = 'block';
        } else {
          noResults.style.display = 'none';
        }
      }
    });

    // Przycisk wyczyść
    document.getElementById('clear-search-btn').onclick = function() {
      searchInput.value = '';
      searchInput.dispatchEvent(new Event('input'));
    };
  }

  // Chirpy Fix: Uruchamiaj przy każdym przeładowaniu strony/taba
  document.addEventListener('DOMContentLoaded', applySearch);
  // Jeśli strona już załadowana (zmiana taba)
  if (document.readyState !== 'loading') applySearch();
  // Backup na wypadek wolnego renderowania
  setTimeout(applySearch, 1000);
</script>
