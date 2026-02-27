---
layout: page
title: Szyfry
icon: fas fa-user-secret
order: 5
---

<div style="background: var(--main-bg); padding: 20px; border-radius: 15px; border: 1px solid var(--main-border-color);">
  <label style="font-weight: bold; display: block; margin-bottom: 10px;">Wpisz tekst do zaszyfrowania:</label>
  <textarea id="cipher-input" rows="3" placeholder="Np. Jajko" style="width: 100%; padding: 12px; border-radius: 10px; border: 2px solid var(--link-color); background: var(--main-bg); color: var(--text-color); outline: none; font-family: inherit; margin-bottom: 15px;"></textarea>
  
  <button id="btn-cipher" style="width: 100%; padding: 15px; background: #007bff; color: white; border: none; border-radius: 10px; cursor: pointer; font-weight: bold; font-size: 1rem;"
    onclick="
      var t = document.getElementById('cipher-input').value.toUpperCase();
      var g = {'G':'A','A':'G','D':'E','E':'D','R':'Y','Y':'R','P':'O','O':'P','L':'U','U':'L','K':'I','I':'K'};
      var m = {'A': '.-', 'B': '-...', 'C': '-.-.', 'D': '-..', 'E': '.', 'F': '..-.', 'G': '--.', 'H': '....', 'I': '..', 'J': '.---', 'K': '-.-', 'L': '.-..', 'M': '--', 'N': '-.', 'O': '---', 'P': '.--.', 'Q': '--.-', 'R': '.-.', 'S': '...', 'T': '-', 'U': '..-', 'V': '...-', 'W': '.--', 'X': '-..-', 'Y': '-.--', 'Z': '--..', '1': '.----', '2': '..---', '3': '...--', '4': '....-', '5': '.....', '6': '-....', '7': '--...', '8': '---..', '9': '----.', '0': '-----', ' ': '/'};
      var c = {'A':'_|','B':'|_|','C':'|_','D':'￣|','E':'|￣|','F':'|￣','G':'_|·','H':'|_|·','I':'|_·','J':'￣|·','K':'|￣|·','L':'|￣·','M':'_|:','N':'|_|:','O':'|_:','P':'￣|:','Q':'|￣|:','R':'|￣:','S':'V','T':'<','U':'>','W':'^','X':'X','Y':'Y','Z':'Z'};
      
      var resG = ''; for(var i=0; i<t.length; i++) { resG += g[t[i]] || t[i]; }
      var resM = ''; for(var i=0; i<t.length; i++) { resM += (m[t[i]] || t[i]) + ' '; }
      var resC = ''; for(var i=0; i<t.length; i++) { resC += (c[t[i]] || t[i]) + '  '; }
      
      document.getElementById('out-gadery').innerText = resG;
      document.getElementById('out-choc').innerText = resC;
      document.getElementById('out-morse').innerText = resM;
    ">
    ZASZYFRUJ WIADOMOŚĆ
  </button>
</div>

<div style="margin-top: 25px; display: grid; gap: 15px;">
  <div style="padding: 15px; border-radius: 10px; background: rgba(0, 123, 255, 0.1); border-left: 5px solid #007bff;">
    <strong style="color: #007bff;">GADERYPOLUKI:</strong>
    <div id="out-gadery" style="margin-top: 5px; font-family: monospace; min-height: 1.5em; color: var(--text-color); font-size: 1.2rem; word-break: break-all;"></div>
  </div>

  <div style="padding: 15px; border-radius: 10px; background: rgba(163, 101, 56, 0.1); border-left: 5px solid #a36538;">
    <strong style="color: #a36538;">CZEKOLADKA (Znakowa):</strong>
    <div id="out-choc" style="margin-top: 5px; font-family: monospace; min-height: 1.5em; color: var(--text-color); font-size: 1.5rem; letter-spacing: 3px;"></div>
  </div>

  <div style="padding: 15px; border-radius: 10px; background: rgba(255, 193, 7, 0.1); border-left: 5px solid #ffc107;">
    <strong style="color: #d39e00;">ALFABET MORSE'A:</strong>
    <div id="out-morse" style="margin-top: 5px; font-family: monospace; min-height: 1.5em; color: var(--text-color); font-size: 1.2rem; word-break: break-all;"></div>
  </div>
</div>
