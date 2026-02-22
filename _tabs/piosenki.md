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
      <h2 class="pt-4 border-bottom letter-group" data-letter="{{ first_letter }}">{{ first_letter }}</h2>
      {% assign current_letter = first_letter %}
    {% endif %}

    <div class="mb-2 song-item">
      <a href="{{ song.url | relative_url }}" class="font-weight-bold">{{ song.title }}</a>
    </div>
  {% endfor %}
</div>

<div id="no-results" class="text-muted d-none">Nie znaleziono piosenki o takim tytule.</div>

<script>
document.getElementById('song-search').addEventListener('keyup', function() {
  const query = this.value.toLowerCase();
  const songs = document.querySelectorAll('.song-item');
  const letters = document.querySelectorAll('.letter-group');
  let hasResults = false;

  songs.forEach(song => {
    const title = song.textContent.toLowerCase();
    if (title.includes(query)) {
      song.classList.remove('d-none');
      hasResults = true;
    } else {
      song.classList.add('d-none');
    }
  });

  // Ukrywanie liter (A, B, C...), jeśli nie ma pod nimi wyników
  letters.forEach(letter => {
    const nextSongs = [];
    let nextEl = letter.nextElementSibling;
    while (nextEl && nextEl.classList.contains('song-item')) {
      if (!nextEl.classList.contains('d-none')) nextSongs.push(nextEl);
      nextEl = nextEl.nextElementSibling;
    }
    
    if (nextSongs.length > 0) {
      letter.classList.remove('d-none');
    } else {
      letter.classList.add('d-none');
    }
  });

  const noResults = document.getElementById('no-results');
  if (hasResults) {
    noResults.classList.add('d-none');
  } else {
    noResults.classList.remove('d-none');
  }
});
</script>
