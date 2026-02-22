---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<div class="mb-4" style="position: relative;">
  <input type="text" id="song-search" class="form-control" placeholder="🔍 Szukaj piosenki..." autocomplete="off" style="padding-right: 35px;">
  <span id="clear-search" style="display:none; position: absolute; right: 10px; top: 50%; transform: translateY(-50%); cursor: pointer; font-size: 20px; font-weight: bold; color: #888; line-height: 1;">&times;</span>
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

<div id="no-results" style="display:none;" class="text-muted mt-3">Nie znaleziono piosenki o takim tytule.</div>

<script>
(function() {
  function initSearch() {
    const searchInput = document.getElementById('song-search');
    const clearBtn = document.getElementById('clear-search');
    const songs = document.querySelectorAll('.song-item');
    const letters = document.querySelectorAll('.letter-group');
    const noResults = document.getElementById('no-results');

    if (!searchInput || !clearBtn) return;

    function filter() {
      const val = searchInput.value.toLowerCase().trim();
      let hasAny = false;

      // Pokazywanie przycisku X
      clearBtn.style.display = val.length > 0 ? 'block' : 'none';

      // Filtrowanie piosenek
      songs.forEach(s => {
        const match = s.textContent.toLowerCase().includes(val);
        s.style.display = match ? 'block' : 'none';
        if (match) hasAny = true;
      });

      // Ukrywanie liter
      letters.forEach(l => {
        let hasVisible = false;
        let next = l.nextElementSibling;
        while (next && next.classList.contains('song-item')) {
          if (next.style.display !== 'none') {
            hasVisible = true;
            break;
          }
          next = next.nextElementSibling;
        }
        l.style.display = hasVisible ? 'block' : 'none';
      });

      noResults.style.display = hasAny ? 'none' : 'block';
    }

    searchInput.addEventListener('input', filter);
    
    clearBtn.addEventListener('click', function() {
      searchInput.value = '';
      filter();
      searchInput.focus();
    });
  }

  // Uruchomienie skryptu niezależnie od tego, jak Chirpy ładuje stronę
  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', initSearch);
  } else {
    initSearch();
  }
})();
</script>
