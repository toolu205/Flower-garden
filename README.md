<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Flower Garden</title>

<style>
html, body {
  margin: 0;
  padding: 0;
  overflow: hidden;
  background: black;
  touch-action: none;
  font-family: Arial, sans-serif;
}

canvas {
  display: block;
}

#overlay {
  position: absolute;
  inset: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

input {
  font-size: 26px;
  padding: 12px 18px;
  border-radius: 6px;
  border: none;
  outline: none;
}
</style>
</head>

<body>

<div id="overlay">
  <input id="nameInput" placeholder="Enter your name" />
</div>

<canvas id="gl"></canvas>

<script>
/* =========================
   BASIC SETUP
========================= */
const canvas = document.getElementById("gl");
const gl = canvas.getContext("webgl");
const input = document.getElementById("nameInput");
const overlay = document.getElementById("overlay");

if (!gl) {
  alert("WebGL not supported on this device");
}

function resize() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  gl.viewport(0, 0, canvas.width, canvas.height);
}
window.addEventListener("resize", resize);
resize();

/* =========================
   APP STATE
========================= */
let started = false;
let userName = "";
let music = null;

/* =========================
   NAME INPUT
========================= */
input.addEventListener("keydown", e => {
  if (e.key === "Enter" && input.value.trim() !== "") {
    userName = input.value.trim();
    overlay.style.display = "none";
    started = true;
    startMusic();
    drawBackground();
  }
});

/* =========================
   MUSIC (MOBILE SAFE)
========================= */
function startMusic() {
  music = new Audio("music.mp3");
  music.loop = true;
  music.volume = 0.6;
  music.play().catch(err => {
    console.log("Music blocked:", err);
  });
}

/* =========================
   BASIC RENDER (TEST)
========================= */
function drawBackground() {
  gl.clearColor(0.1, 0.0, 0.2, 1.0);
  gl.clear(gl.COLOR_BUFFER_BIT);
}

/* =========================
   TOUCH TEST
========================= */
canvas.addEventListener("touchstart", e => {
  if (!started) return;
  const t = e.touches[0];
  flashColor();
});

/* =========================
   VISUAL FEEDBACK
========================= */
function flashColor() {
  gl.clearColor(Math.random(), Math.random(), Math.random(), 1.0);
  gl.clear(gl.COLOR_BUFFER_BIT);
}
</script>

</body>
</html>
