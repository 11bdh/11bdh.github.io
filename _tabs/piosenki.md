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
  function runSongSearch() {
    const input = document.getElementById('song-search');
    if (!input) return;

    input.addEventListener('input', function() {
      const query = this.value.toLowerCase().trim();
      const items = document.querySelectorAll('.song-item');
      const letters = document.querySelectorAll('.letter-group');
      let found = false;

      items.forEach(item => {
        const text = item.textContent.toLowerCase();
        if (text.includes(query)) {
          item.style.setProperty('display', 'block', 'important');
          found = true;
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

      document.getElementById('no-results').style.display = found ? 'none' : 'block';
    });
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', runSongSearch);
  } else {
    runSongSearch();
  }
  
  setTimeout(runSongSearch, 500);
</script>
