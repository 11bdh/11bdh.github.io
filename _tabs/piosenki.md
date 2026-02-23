---
layout: page
title: Piosenki
icon: fas fa-music
order: 99
---

<style>
  /* Klasa wymuszająca ukrycie, której Chirpy nie zignoruje */
  .hide-now {
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

<div id="search-area" style="margin-bottom: 2rem; position: sticky; top: 1rem; z-index: 1000; background: var(--main-bg);">
  <input type="text" id="song-search" placeholder="🔍 Szukaj piosenki..." autocomplete="off">
  <div id="search-stats" style="display:none; font-size: 0.85rem; color: var(--link-color); margin: 8px 0 0 15px; font-weight: bold;"></div>
</div>

{% assign sorted_songs = site.piosenki | sort: "title" %}

<div id="all-songs">
  {% assign current_letter = "" %}
  {% for song in sorted_songs %}
    {% capture first_letter %}{{ song.title | slice: 0 | upcase }}{% endcapture %}
    {% if first_letter != current_letter %}
      <h2 class="letter-header" style="padding-top: 1.5rem; border-bottom: 1px solid var(--main-border-color); margin-bottom: 1rem;">{{ first_letter }}</h2>
      {% assign current_letter = first_letter %}
    {% endif %}
    <div class="song-row" style="margin-bottom: 0.5rem;">
      <a href="{{ song.url | relative_url }}" style="font-weight: 600; text-decoration: none; display: block; padding: 5px 0;">{{ song.title }}</a>
    </div>
  {% endfor %}
</div>

<div id="desert-msg" style="display:none; margin-top: 20px; border-radius: 15px; overflow: hidden; position: relative; min-height: 300px;">
  <div id="desert-bg" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; background-size: cover; background-position: center;"></div>
  <div style="position: relative; z-index: 1; padding: 60px 20px; text-align: center; color: white; text-shadow: 2px 2px 10px rgba(0,0,0,0.9); background: rgba(0,0,0,0.3); height: 100%; display: flex; flex-direction: column; justify-content: center; align-items: center;">
    <h3 style="margin-bottom: 10px;">Pusto tutaj... 🌵</h3>
    <button onclick="const i = document.getElementById('song-search'); i.value=''; i.dispatchEvent(new Event('input'));" 
            style="margin-top: 20px; padding: 10px 25px; border-radius: 20px; border: none; background: var(--link-color); color: white; font-weight: bold; cursor: pointer;">
      Wyczyść
    </button>
  </div>
</div>

<script>
  // Ta funkcja zawiera całą logikę
  var desertSearch = function() {
    var input = document.getElementById('song-search');
    if (!input || input.dataset.hooked) return;
    
    input.dataset.hooked = "true";
    var songs = document.querySelectorAll('.song-row');
    var heads = document.querySelectorAll('.letter-header');
    var stats = document.getElementById('search-stats');
    var msg = document.getElementById('desert-msg');
    var bg = document.getElementById('desert-bg');

    input.addEventListener('input', function() {
      var term = input.value.toLowerCase().trim();
      var found = 0;

      songs.forEach(function(s) {
        if (s.innerText.toLowerCase().indexOf(term) > -1) {
          s.classList.remove('hide-now');
          found++;
        } else {
          s.classList.add('hide-now');
        }
      });

      heads.forEach(function(h) {
        var visible = false;
        var next = h.nextElementSibling;
        while (next && next.classList.contains('song-row')) {
          if (!next.classList.contains('hide-now')) { visible = true; break; }
          next = next.nextElementSibling;
        }
        h.classList.toggle('hide-now', !visible);
      });

      if (term.length > 0) {
        stats.style.display = 'block';
        stats.innerText = 'Znaleziono: ' + found;
        if (found === 0) {
          var dark = document.documentElement.getAttribute('data-mode') === 'dark';
          var file = dark ? 'pustynia-noc.png' : 'pustynia-dzien.png';
          bg.style.backgroundImage = "url('{{ "/assets/zdjecia/" | relative_url }}" + file + "')";
          msg.style.display = 'block';
        } else {
          msg.style.display = 'none';
        }
      } else {
        stats.style.display = 'none';
        msg.style.display = 'none';
      }
    });
  };

  // TO JEST KLUCZ: Uruchamiaj skrypt co chwilę, dopóki nie znajdzie wyszukiwarki
  // Działa to świetnie z Pjaxem w Chirpy Starter
  var searchInterval = setInterval(function() {
    if (document.getElementById('song-search')) {
      desertSearch();
    }
  }, 500);

  // Zabezpieczenie przy zmianie stron
  document.addEventListener('pjax:complete', desertSearch);
</script>
