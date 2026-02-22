---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<div class="mb-4" style="position: relative;">
  <input type="text" id="song-search" class="form-control" placeholder="🔍 Szukaj piosenki..." autocomplete="off" style="padding-right: 35px;">
  <span id="clear-search" style="display:none; position: absolute; right: 10px; top: 50%; transform: translateY(-50%); cursor: pointer; font-size: 22px; color: #888; line-height: 1; user-select: none; z-index: 10;">&times;</span>
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
    const clearBtn = document.getElementById('clear-search');
    if (!input || !clearBtn) return;

    function filter() {
      const query = input.value.toLowerCase().trim();
      const items = document.querySelectorAll('.song-item');
      const letters = document.querySelectorAll('.letter-group');
      let found = false;

      // Pokaż X tylko jeśli coś jest wpisane
      clearBtn.style.display = query.length > 0 ? 'block' : 'none';

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
    }

    input.addEventListener('input', filter);
    
    // Obsługa kliknięcia w krzyżyk
    clearBtn.addEventListener('click', function() {
      input.value = '';
      filter();
      input.focus();
    });
  }

  // Odpalenie skryptu
  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', runSongSearch);
  } else {
    runSongSearch();
  }
  
  // Bezpiecznik dla motywu Chirpy
  setTimeout(runSongSearch, 500);
</script>
