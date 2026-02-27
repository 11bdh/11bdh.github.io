---
layout: page
title: Szyfry
icon: fas fa-user-secret
order: 5
---

<div style="background: var(--main-bg); padding: 20px; border-radius: 15px; border: 1px solid var(--main-border-color);">
  <label style="font-weight: bold; display: block; margin-bottom: 10px;">Wpisz tekst do zaszyfrowania:</label>
  <textarea id="cipher-input" rows="3" placeholder="Np. Czuwaj zastępie!" 
    style="width: 100%; padding: 12px; border-radius: 10px; border: 2px solid var(--link-color); background: var(--main-bg); color: var(--text-color); outline: none; font-family: inherit;"
    oninput="
      const text = this.value.toUpperCase();
      
      // Funkcja bazowa dla szyfrów podstawieniowych
      function substitute(str, key) {
        const map = {};
        for(let i=0; i<key.length; i+=2) {
          map[key[i]] = key[i+1];
          map[key[i+1]] = key[i];
        }
        return str.split('').map(char => map[char] || char).join('');
      }

      // Funkcja Morse'a
      const morseMap = {
        'A': '.-', 'B': '-...', 'C': '-.-.', 'D': '-..', 'E': '.', 'F': '..-.', 'G': '--.', 'H': '....',
        'I': '..', 'J': '.---', 'K': '-.-', 'L': '.-..', 'M': '--', 'N': '-.', 'O': '---', 'P': '.--.',
        'Q': '--.-', 'R': '.-.', 'S': '...', 'T': '-', 'U': '..-', 'V': '...-', 'W': '.--', 'X': '-..-',
        'Y': '-.--', 'Z': '--..', '1': '.----', '2': '..---', '3': '...--', '4': '....-', '5': '.....',
        '6': '-....', '7': '--...', '8': '---..', '9': '----.', '0': '-----', ' ': '/'
      };

      // Wyniki
      document.getElementById('out-gadery').innerText = substitute(text, 'GA DE RY PO LU KI');
      document.getElementById('out-polityka').innerText = substitute(text, 'PO LI TY KA RE NU');
      document.getElementById('out-morse').innerText = text.split('').map(c => morseMap[c] || c).join(' ');
    "></textarea>
</div>

<div style="margin-top: 20px; display: grid; gap: 15px;">

  <div style="padding: 15px; border-radius: 10px; background: rgba(0, 123, 255, 0.1); border-left: 5px solid #007bff;">
    <strong style="color: var(--link-color);">GADERYPOLUKI:</strong>
    <div id="out-gadery" style="margin-top: 5px; font-family: monospace; letter-spacing: 1px; word-break: break-all;">...</div>
  </div>

  <div style="padding: 15px; border-radius: 10px; background: rgba(40, 167, 69, 0.1); border-left: 5px solid #28a745;">
    <strong style="color: #28a745;">POLITYKARENU:</strong>
    <div id="out-polityka" style="margin-top: 5px; font-family: monospace; letter-spacing: 1px; word-break: break-all;">...</div>
  </div>

  <div style="padding: 15px; border-radius: 10px; background: rgba(255, 193, 7, 0.1); border-left: 5px solid #ffc107;">
    <strong style="color: #d39e00;">ALFABET MORSE'A:</strong>
    <div id="out-morse" style="margin-top: 5px; font-family: monospace; word-break: break-all;">...</div>
  </div>

</div>

<div style="margin-top: 30px; font-size: 0.9rem; opacity: 0.8; text-align: center;">
  💡 Wskazówka: Możesz skopiować wynik i wkleić go bezpośrednio do rozkazu!
</div>
