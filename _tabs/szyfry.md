---
layout: page
title: Szyfry
icon: fas fa-user-secret
order: 5
---

<div style="background: var(--main-bg); padding: 20px; border-radius: 15px; border: 1px solid var(--main-border-color);">
  <label style="font-weight: bold; display: block; margin-bottom: 10px;">Wpisz tekst do zaszyfrowania:</label>
  <textarea id="cipher-input" rows="3" placeholder="Np. Czuwaj!" 
    style="width: 100%; padding: 12px; border-radius: 10px; border: 2px solid var(--link-color); background: var(--main-bg); color: var(--text-color); outline: none; font-family: inherit;"
    oninput="
      const text = this.value.toUpperCase();
      
      // GADERYPOLUKI
      const gadeKey = {'G':'A','A':'G','D':'E','E':'D','R':'Y','Y':'R','P':'O','O':'P','L':'U','U':'L','K':'I','I':'K'};
      document.getElementById('out-gadery').innerText = text.split('').map(c => gadeKey[c] || c).join('');

      // MORSE
      const morseMap = {'A': '.-', 'B': '-...', 'C': '-.-.', 'D': '-..', 'E': '.', 'F': '..-.', 'G': '--.', 'H': '....', 'I': '..', 'J': '.---', 'K': '-.-', 'L': '.-..', 'M': '--', 'N': '-.', 'O': '---', 'P': '.--.', 'Q': '--.-', 'R': '.-.', 'S': '...', 'T': '-', 'U': '..-', 'V': '...-', 'W': '.--', 'X': '-..-', 'Y': '-.--', 'Z': '--..', '1': '.----', '2': '..---', '3': '...--', '4': '....-', '5': '.....', '6': '-....', '7': '--...', '8': '---..', '9': '----.', '0': '-----', ' ': '/'};
      document.getElementById('out-morse').innerText = text.split('').map(c => morseMap[c] || c).join(' ');

      // CZEKOLADKA (Szyfr ułamkowy)
      const choc = {'A':'1/1','B':'2/1','C':'3/1','D':'1/2','E':'2/2','F':'3/2','G':'1/3','H':'2/3','I':'3/3','J':'1/1.','K':'2/1.','L':'3/1.','M':'1/2.','N':'2/2.','O':'3/2.','P':'1/3.','Q':'2/3.','R':'3/3.','S':'1/1:','T':'2/1:','U':'3/1:','V':'1/2:','W':'2/2:','X':'3/2:','Y':'1/3:','Z':'2/3:'};
      document.getElementById('out-choc').innerText = text.split('').map(c => choc[c] || c).join(' ');
    "></textarea>
</div>

<div style="margin-top: 20px; display: grid; gap: 15px;">

  <div style="padding: 15px; border-radius: 10px; background: rgba(0, 123, 255, 0.1); border-left: 5px solid #007bff;">
    <strong style="color: var(--link-color);">GADERYPOLUKI:</strong>
    <div id="out-gadery" style="margin-top: 5px; font-family: monospace; letter-spacing: 1px; word-break: break-all;">...</div>
  </div>

  <div style="padding: 15px; border-radius: 10px; background: rgba(163, 101, 56, 0.1); border-left: 5px solid #a36538;">
    <strong style="color: #a36538;">CZEKOLADKA (Ułamkowy):</strong>
    <div id="out-choc" style="margin-top: 5px; font-family: monospace; letter-spacing: 1px; word-break: break-all;">...</div>
    <div style="font-size: 0.7rem; margin-top: 5px; opacity: 0.7;">Legenda: 1/2 to 1. kolumna, 2. wiersz. Kropki (. lub :) to numer kratki.</div>
  </div>

  <div style="padding: 15px; border-radius: 10px; background: rgba(255, 193, 7, 0.1); border-left: 5px solid #ffc107;">
    <strong style="color: #d39e00;">ALFABET MORSE'A:</strong>
    <div id="out-morse" style="margin-top: 5px; font-family: monospace; word-break: break-all;">...</div>
  </div>

</div>
