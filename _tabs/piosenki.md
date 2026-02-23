---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<div class="search-wrapper mb-4">
  <div class="input-group">
    <input type="text" id="song-search" class="form-control" placeholder="🔍 Szukaj tytułu lub fragmentu..." autocomplete="off">
  </div>
  <div id="search-stats" class="text-muted small mt-2" style="display:none;"></div>
</div>

{% assign sorted_songs = site.piosenki | sort: "title" %}

<div id="song-list">
  {% assign current_letter = "" %}
  {% for song in sorted_songs %}
    {% capture first_letter %}{{ song.title | slice: 0 | upcase }}{% endcapture %}
    
    {% if first_letter != current_letter %}
      <h2 class="pt-4 border-bottom letter-group">{{ first_letter }}</h2>
      {% assign current_letter = first_letter %}
    {% endif %}

    <div class="mb-2 song-item">
      <a href="{{ song.url | relative_url }}" class="font-weight-bold">{{ song.title }}</a>
    </div>
  {% endfor %}
</div>

<div id="no-results" style="display:none;" class="alert alert-light mt-3">
  🌵 Brak piosenek pasujących do wyszukiwania.
</div>

<style>
  .search-wrapper {
    position: sticky;
    top: 10px; /* Pasek będzie "płynął" za Tobą przy przewijaniu (opcjonalnie) */
    z-index: 100;
    background: var(--main-bg);
    padding-bottom: 10px;
  }
  #song-search {
    border-radius: 20px;
    border: 2px solid var(--main-border-color);
    padding: 10px 20px;
    transition: all 0.3s ease;
    box-shadow: 0 2px 5px rgba(0,0,0,0.05);
  }
  #song-search:focus {
    border-color: var(--link-color);
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    outline: none;
  }
  .song-item a {
    display: block;
    padding: 5px 10px;
    border-radius: 5px;
    transition: background 0.2s;
  }
  .song-item a:hover {
    background: var(--nav-item-hover-bg);
    text-decoration: none !important;
  }
</style>

<script>
  function runSongSearch() {
    const input = document.getElementById('song-search');
    const stats = document.getElementById('search-stats');
    if (!input) return;

    input.addEventListener('input', function() {
      const query = this.value.toLowerCase().trim();
      const items = document.querySelectorAll('.song-item');
      const letters = document.querySelectorAll('.letter-group');
      let foundCount = 0;

      items.forEach(item => {
        const text = item.textContent.toLowerCase();
        if (text.includes(query)) {
          item.style.setProperty('display', 'block', 'important');
          foundCount++;
        } else {
          item.style.setProperty('display', 'none', 'important');
        }
      });

      letters.forEach(letter => {
        let hasVisible = false;
        let next = letter.nextElementSibling;
        while (next && next.classList.contains('song-item')) {
          if (next.style.display !== 'none') {
            hasVisible = true;
            break;
          }
          next = next.nextElementSibling;
        }
        letter.style.setProperty('display', hasVisible ? 'block' : 'none', 'important');
      });

      // Obsługa statystyk i braku wyników
      if (query.length > 0) {
        stats.style.display = 'block';
        stats.textContent = `Znaleziono: ${foundCount}`;
        document.getElementById('no-results').style.display = foundCount > 0 ? 'none' : 'block';
      } else {
        stats.style.display = 'none';
        document.getElementById('no-results').style.display = 'none';
      }
    });
  }

  // Uruchomienie
  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', runSongSearch);
  } else {
    runSongSearch();
  }
  setTimeout(runSongSearch, 500);
</script>
