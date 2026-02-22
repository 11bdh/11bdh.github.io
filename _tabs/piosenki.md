---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<div class="mb-4">
  <input type="text" id="song-search" class="form-control" placeholder="🔍 Szukaj piosenki..." aria-label="Szukaj piosenki">
</div>

{% assign sorted_songs = site.piosenki | sort: "title" %}

<div id="song-list" class="pl-1 pr-1">
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

<div id="no-results" class="text-muted d-none mt-3">Nie znaleziono piosenki o takim tytule.</div>

<script>
document.getElementById('song-search').addEventListener('input', function() {
  const query = this.value.toLowerCase().trim();
  const songs = document.querySelectorAll('.song-item');
  const letters = document.querySelectorAll('.letter-group');
  let foundAny = false;

  songs.forEach(song => {
    const title = song.textContent.toLowerCase();
    // Sprawdza czy tekst jest gdziekolwiek w tytule
    if (title.includes(query)) {
      song.style.display = 'block';
      foundAny = true;
    } else {
      song.style.display = 'none';
    }
  });

  // Ukrywanie liter, jeśli nie ma pod nimi wyników
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

  const noResults = document.getElementById('no-results');
  if (foundAny) {
    noResults.classList.add('d-none');
  } else {
    noResults.classList.remove('d-none');
  }
});
</script>
