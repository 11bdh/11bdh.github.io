---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<div id="search-container" style="margin-bottom: 2rem; position: sticky; top: 1rem; z-index: 1000; background: var(--main-bg);">
  <input type="text" id="song-search" 
         style="width: 100%; padding: 12px 20px; border: 2px solid var(--link-color); border-radius: 25px; background: var(--main-bg); color: var(--text-color); outline: none; box-shadow: 0 4px 10px rgba(0,0,0,0.1);" 
         placeholder="🔍 Zacznij pisać, aby szukać..." autocomplete="off">
  <div id="search-stats" style="display:none; font-size: 0.85rem; color: var(--link-color); margin: 8px 0 0 15px; font-weight: bold;"></div>
</div>

{% assign sorted_songs = site.piosenki | sort: "title" %}

<div id="song-list">
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

<div id="no-results" style="display:none; padding: 20px; background: var(--main-border-color); border-radius: 10px; text-align: center; margin-top: 20px;">
  Nie znaleziono piosenek o tej nazwie. 🎸
</div>

<script>
  function runSongSearch() {
    const input = document.getElementById('song-search');
    const stats = document.getElementById('search-stats');
    const noResults = document.getElementById('no-results');
    
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

      if (query.length > 0) {
        stats.style.display = 'block';
        stats.textContent = 'Znaleziono: ' + foundCount;
        noResults.style.display = foundCount > 0 ? 'none' : 'block';
      } else {
        stats.style.display = 'none';
        noResults.style.display = 'none';
      }
    });
  }

  document.addEventListener('DOMContentLoaded', runSongSearch);
  if (document.readyState !== 'loading') runSongSearch();
  setTimeout(runSongSearch, 500);
</script>
