---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<div class="mb-4 position-relative">
  <input type="text" id="song-search" class="form-control" placeholder="🔍 Szukaj piosenki..." autocomplete="off" style="padding-right: 40px;">
  <button id="clear-search" type="button" style="display:none; position: absolute; right: 10px; top: 50%; transform: translateY(-50%); border: none; background: transparent; cursor: pointer; color: var(--text-muted); font-size: 1.2rem;">
    <i class="fas fa-times"></i>
  </button>
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
document.addEventListener('DOMContentLoaded', function() {
  const searchInput = document.getElementById('song-search');
  const clearBtn = document.getElementById('clear-search');
  const songs = document.querySelectorAll('.song-item');
  const letters = document.querySelectorAll('.letter-group');
  const noResults = document.getElementById('no-results');

  function filterSongs() {
    const query = searchInput.value.toLowerCase().trim();
    let foundAny = false;

    // Pokaż/ukryj przycisk X
    clearBtn.style.display = query.length > 0 ? 'block' : 'none';

    // Filtrowanie piosenek
    songs.forEach(song => {
      const title = song.textContent.toLowerCase();
      if (title.includes(query)) {
        song.style.display = 'block';
        foundAny = true;
      } else {
        song.style.display = 'none';
      }
    });

    // Filtrowanie liter
    letters.forEach(letter => {
      let hasVisibleSongs = false;
      let nextEl = letter.nextElementSibling;
      while (nextEl && nextEl.classList.contains('song-item')) {
        if (nextEl.style.display !== 'none') {
          hasVisibleSongs = true;
          break;
        }
        nextEl = nextEl.nextElementSibling;
      }
      letter.style.display = hasVisibleSongs ? 'block' : 'none';
    });

    noResults.style.display = foundAny ? 'none' : 'block';
  }

  // Obsługa wpisywania
  searchInput.addEventListener('input', filterSongs);

  // Obsługa przycisku X
  clearBtn.addEventListener('click', function() {
    searchInput.value = '';
    filterSongs();
    searchInput.focus();
  });
});
</script>
