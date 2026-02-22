---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<div class="mb-4">
  <input type="text" id="song-search" class="form-control" placeholder="🔍 Szukaj piosenki..." autocomplete="off">
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

<div id="no-results" style="display:none;" class="text-muted mt-3">Nie znaleziono piosenki.</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const searchInput = document.getElementById('song-search');
  
  searchInput.addEventListener('input', function() {
    const query = this.value.toLowerCase().trim();
    const songs = document.querySelectorAll('.song-item');
    const letters = document.querySelectorAll('.letter-group');
    let foundAny = false;

    songs.forEach(song => {
      const title = song.textContent.toLowerCase();
      if (title.includes(query)) {
        song.style.display = 'block';
        foundAny = true;
      } else {
        song.style.display = 'none';
      }
    });

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

    document.getElementById('no-results').style.display = foundAny ? 'none' : 'block';
  });
});
</script>
