<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Samuel Mendes | Co-fundador</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Segoe UI', sans-serif;
    }

    body {
      height: 100vh;
      background: #050505;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
      color: white;
    }

    canvas {
      position: absolute;
      width: 100%;
      height: 100%;
      z-index: 0;
    }

    .container {
      position: relative;
      z-index: 10;
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      background: rgba(255,255,255,0.05);
      padding: 45px;
      border-radius: 25px;
      backdrop-filter: blur(20px);
      border: 1px solid rgba(255,255,255,0.1);
      width: 340px;
    }

    .btn {
      width: 100%;
      padding: 14px;
      border-radius: 14px;
      text-decoration: none;
      font-weight: bold;
      color: white;
      margin: 10px 0;
      text-align: center;
      transition: transform 0.2s;
    }

    .btn:hover {
      transform: scale(1.02);
    }

    .pessoal {
      background: linear-gradient(45deg, #833ab4, #fd1d1d);
    }

    .filho {
      background: linear-gradient(45deg, #1d976c, #93f9b9);
    }
  </style>
</head>
<body>

<canvas id="bg"></canvas>

<audio id="audio" loop>
  <source src="skyfall.mp3" type="audio/mpeg">
</audio>

<div class="container">
  <h1>Samuel Mendes</h1>
  <h2 style="font-weight: 300; font-size: 1.1rem; opacity: 0.8; margin-bottom: 20px;">Co-fundador</h2>

  <a class="btn pessoal" href="https://www.instagram.com/samuelmendes0" target="_blank">
    Samuel
  </a>

  <a class="btn filho" href="https://www.instagram.com/savio_ss_?igsh=MW5hbHJpN25yZWY5cA==" target="_blank">
    Filho
  </a>
</div>

<script>
  const audio = document.getElementById("audio");

  // Função para dar play
  const startAudio = () => {
    audio.play().catch(e => console.log("Aguardando interação para tocar áudio."));
    // Remove o listener após a primeira interação para não rodar toda vez que clicar
    window.removeEventListener("click", startAudio);
  };

  // Tenta tocar assim que carregar (raramente funciona sem clique)
  window.addEventListener("load", () => {
    audio.play().catch(() => {
      // Se falhar, toca no primeiro clique que o usuário der em qualquer lugar da página
      window.addEventListener("click", startAudio);
    });
  });

  // --- Lógica do Fundo Interativo (Mantida) ---
  const canvas = document.getElementById("bg");
  const ctx = canvas.getContext("2d");

  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  let mouse = { x: null, y: null };

  window.addEventListener("mousemove", e => {
    mouse.x = e.x;
    mouse.y = e.y;
  });

  let particles = [];
  for (let i = 0; i < 80; i++) {
    particles.push({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height,
      dx: (Math.random() - 0.5) * 1.5,
      dy: (Math.random() - 0.5) * 1.5,
      r: Math.random() * 2 + 1
    });
  }

  function draw() {
    ctx.clearRect(0,0,canvas.width,canvas.height);
    particles.forEach(p => {
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
      ctx.fillStyle = "white";
      ctx.fill();

      let dx = p.x - mouse.x;
      let dy = p.y - mouse.y;
      let dist = Math.sqrt(dx*dx + dy*dy);

      if (dist < 100) {
        p.x += dx * 0.03;
        p.y += dy * 0.03;
      }
      p.x += p.dx;
      p.y += p.dy;
      if (p.x < 0 || p.x > canvas.width) p.dx *= -1;
      if (p.y < 0 || p.y > canvas.height) p.dy *= -1;
    });
    requestAnimationFrame(draw);
  }
  draw();
</script>

</body>
</html>
