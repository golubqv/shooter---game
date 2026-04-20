# shooter---game
game
<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Shooter Game</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      background: #0b0f1a;
      font-family: Arial, sans-serif;
      color: white;
      text-align: center;
    }
    canvas {
      display: block;
      margin: 0 auto;
      background: #111827;
    }
    #hud {
      position: absolute;
      top: 10px;
      left: 10px;
      font-size: 18px;
    }
    #gameOver {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 32px;
      display: none;
    }
  </style>
</head>
<body>
<div id="hud">Score: 0</div>
<div id="gameOver">Game Over<br>Press R to Restart</div>
<canvas id="game" width="800" height="500"></canvas>
<script>
const canvas = document.getElementById('game');
const ctx = canvas.getContext('2d');let player = { x: 400, y: 450, w: 30, h: 30, speed: 6 }; let bullets = []; let enemies = []; let keys = {}; let score = 0; let gameRunning = true;

function drawPlayer() { ctx.fillStyle = 'cyan'; ctx.fillRect(player.x, player.y, player.w, player.h); }

function drawBullets() { ctx.fillStyle = 'yellow'; bullets.forEach(b => ctx.fillRect(b.x, b.y, 5, 10)); }

function drawEnemies() { ctx.fillStyle = 'red'; enemies.forEach(e => ctx.fillRect(e.x, e.y, 30, 30)); }

function movePlayer() { if (keys['ArrowLeft'] && player.x > 0) player.x -= player.speed; if (keys['ArrowRight'] && player.x < canvas.width - player.w) player.x += player.speed; }

function shoot() { bullets.push({ x: player.x + 12, y: player.y }); }

document.addEventListener('keydown', (e) => { keys[e.key] = true; if (e.key === ' ' && gameRunning) shoot(); if (e.key === 'r') resetGame(); });

document.addEventListener('keyup', (e) => { keys[e.key] = false; });

function updateBullets() { bullets.forEach(b => b.y -= 8); bullets = bullets.filter(b => b.y > 0); }

function spawnEnemies() { if (Math.random() < 0.02) { enemies.push({ x: Math.random() * 770, y: -30 }); } }

function updateEnemies() { enemies.forEach(e => e.y += 3); enemies.forEach(e => { if ( e.x < player.x + player.w && e.x + 30 > player.x && e.y < player.y + player.h && e.y + 30 > player.y ) { endGame(); } }); enemies = enemies.filter(e => e.y < 500); }

function checkCollisions() { bullets.forEach((b, bi) => { enemies.forEach((e, ei) => { if ( b.x < e.x + 30 && b.x + 5 > e.x && b.y < e.y + 30 && b.y + 10 > e.y ) { bullets.splice(bi, 1); enemies.splice(ei, 1); score += 10; document.getElementById('hud').innerText = 'Score: ' + score; } }); }); }

function endGame() { gameRunning = false; document.getElementById('gameOver').style.display = 'block'; }

function resetGame() { player.x = 400; bullets = []; enemies = []; score = 0; gameRunning = true; document.getElementById('hud').innerText = 'Score: 0'; document.getElementById('gameOver').style.display = 'none'; }

function loop() { ctx.clearRect(0, 0, canvas.width, canvas.height); if (gameRunning) { movePlayer(); spawnEnemies(); updateBullets(); updateEnemies(); checkCollisions(); } drawPlayer(); drawBullets(); drawEnemies(); requestAnimationFrame(loop); }

loop(); </script>

</body>
</html>
