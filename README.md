<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Roleta de Prêmios</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

    body {
      background: #f5f5f5;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      margin: 0;
    }

    .wheel-container {
      position: relative;
      width: 300px;
      height: 300px;
    }

    .wheel {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      border: 10px solid #222;
      position: relative;
      overflow: hidden;
      transition: transform 5s cubic-bezier(0.33, 1, 0.68, 1);
    }

    .wheel .segment {
      position: absolute;
      width: 50%;
      height: 50%;
      top: 50%;
      left: 50%;
      transform-origin: 0% 0%;
      background: #fdd835;
      border: 1px solid #fff;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 14px;
      font-weight: bold;
      text-align: center;
      padding: 10px;
      color: #222;
    }

    .pointer {
      width: 0;
      height: 0;
      border-left: 20px solid transparent;
      border-right: 20px solid transparent;
      border-bottom: 30px solid red;
      position: absolute;
      top: -30px;
      left: calc(50% - 20px);
      z-index: 10;
    }

    #spinBtn {
      margin-top: 20px;
      padding: 10px 20px;
      font-size: 18px;
      background: #0069d9;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    #result {
      margin-top: 20px;
      font-size: 18px;
      font-weight: bold;
      color: #333;
    }
  </style>
</head>
<body>

  <div class="wheel-container">
    <div class="pointer"></div>
    <div class="wheel" id="wheel">
      <!-- JS vai adicionar os segmentos aqui -->
    </div>
  </div>

  <button id="spinBtn">GIRAR 🎯</button>
  <div id="result"></div>

  <script>
    const prizes = [
      "5.000 Panfletos",
      "Não foi desta vez",
      "10% de desconto",
      "500 Cartões de Visita",
      "Não foi desta vez",
      "Agenda",
      "Não foi desta vez",
      "Caneca personalizada"
    ];

    const colors = [
      "#FFD700", "#FFF", "#B2FF59", "#40C4FF", "#FFF", "#FF8A65", "#FFF", "#BA68C8"
    ];

    const wheel = document.getElementById("wheel");
    const spinBtn = document.getElementById("spinBtn");
    const result = document.getElementById("result");

    const numSegments = prizes.length;
    const angle = 360 / numSegments;

    // Desenhar os segmentos
    prizes.forEach((text, i) => {
      const segment = document.createElement("div");
      segment.classList.add("segment");
      segment.style.transform = `rotate(${angle * i}deg) skewY(-${90 - angle}deg)`;
      segment.style.background = colors[i];
      segment.innerHTML = `<div style="transform: skewY(${90 - angle}deg) rotate(${angle / 2}deg);">${text}</div>`;
      wheel.appendChild(segment);
    });

    let isSpinning = false;

    spinBtn.addEventListener("click", () => {
      if (isSpinning) return;

      isSpinning = true;
      result.textContent = "";

      const randomDeg = Math.floor(Math.random() * 360 + 360 * 5); // multiple rotations
      wheel.style.transition = "transform 5s cubic-bezier(0.33, 1, 0.68, 1)";
      wheel.style.transform = `rotate(${randomDeg}deg)`;

      setTimeout(() => {
        const finalDeg = randomDeg % 360;
        const index = Math.floor((360 - finalDeg + angle / 2) % 360 / angle);
        result.textContent = `🎉 Resultado: ${prizes[index]}`;
        isSpinning = false;
      }, 5200);
    });
  </script>

</body>
</html>
