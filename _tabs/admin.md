---
title: Admin
icon: fas fa-lock
order: 99
---

<div id="login-box" style="max-width:400px;margin:2rem auto;">
  <p><strong>Panel prywatny</strong></p>
  <p>Podaj hasło:</p>
  <input type="password" id="pwd" style="width:100%;padding:8px;">
  <button onclick="check()" style="margin-top:10px;padding:6px 12px;">OK</button>
</div>

<div id="admin-content" style="display:none;">

## 🧪 Prompt Lab

<iframe src="https://calendar.google.com/calendar/embed?height=600&wkst=1&ctz=Europe%2FWarsaw&showCalendars=0&showTz=0&src=ZXdld2VyYmFuQGdtYWlsLmNvbQ&color=%23039be5" style="border:solid 1px #777" width="800" height="600" frameborder="0" scrolling="no"></iframe>

<textarea placeholder="Prompt 2..." style="width:100%;height:120px;margin-top:10px;"></textarea>

<textarea placeholder="Prompt 3..." style="width:100%;height:120px;margin-top:10px;"></textarea>

<textarea placeholder="Prompt 4..." style="width:100%;height:120px;margin-top:10px;"></textarea>

</div>

<script>
function check() {
  const pass = "admin";   
  const input = document.getElementById("pwd").value;

  if (input === pass) {
    document.getElementById("login-box").style.display = "none";
    document.getElementById("admin-content").style.display = "block";
  } else {
    alert("Złe hasło");
  }
}
</script>
