<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <title>Колесо Фортуны</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
      :root {
        --bg-dark: #1a1a1a;
        --panel: #2b2b2b;
        --metal: #4a4f5a;
        --rust: #8b5e3c;
        --wood: #5d4037;
        --accent: #f1c40f; /* тёплый «ламповый» цвет */
        --text: #f0f0f0;
        --border: #333;
      }

      body {
        margin: 0;
        padding: 0;
        background-color: var(--bg-dark);
        color: var(--text);
        font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
        display: flex;
        flex-direction: column;
        align-items: center;
        min-height: 100vh;
        overflow: hidden;
      }

      .container {
        padding: 16px;
        width: 100%;
        max-width: 420px;
        text-align: center;
      }

      h1 {
        margin: 0 0 8px 0;
        font-size: 22px;
        letter-spacing: 1px;
        text-transform: uppercase;
      }

      p {
        margin: 0 0 24px 0;
        color: #aaa;
        line-height: 1.4;
      }

      /* Колесо и рамка в «выживальческом» стиле */
      .wheel-wrapper {
        position: relative;
        width: 100%;
        aspect-ratio: 1 / 1;
        max-width: 320px;
        margin: 0 auto 24px auto;
        border: 6px solid var(--metal);
        border-radius: 50%;
        box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        background: radial-gradient(circle at center, #222 0%, #111 100%);
      }

      canvas#wheelCanvas {
        width: 100%;
        height: 100%;
        border-radius: 50%;
      }

      /* Стрелка «потрёпанная» */
      .pointer {
        position: absolute;
        top: -14px;
        left: 50%;
        transform: translateX(-50%);
        width: 0;
        height: 0;
        border-left: 14px solid transparent;
        border-right: 14px solid transparent;
        border-top: 30px solid var(--accent);
        filter: drop-shadow(0 4px 4px rgba(0,0,0,0.4));
        clip-path: polygon(0% 0%, 100% 0%, 50% 100%, 45% 90%, 55% 90%); /* чуть «погнутая» форма */
      }

      button#spinBtn {
        background: linear-gradient(180deg, var(--metal), #333);
        color: var(--accent);
        border: 2px solid var(--border);
        padding: 14px 24px;
        font-size: 18px;
        font-weight: bold;
        border-radius: 6px;
        cursor: pointer;
        transition: all 0.2s;
        text-transform: uppercase;
        letter-spacing: 0.5px;
      }

      button#spinBtn:active {
        transform: scale(0.96);
        background: #333;
      }

      button#spinBtn:disabled {
        background: #444;
        color: #666;
        cursor: not-allowed;
        border-color: #333;
      }

      .result-box {
        margin-top: 16px;
        padding: 16px;
        background: var(--panel);
        border: 1px solid var(--border);
        border-radius: 8px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.3);
        display: none;
      }

      .prize-name {
        font-size: 20px;
        font-weight: bold;
        color: var(--accent);
        margin: 0 0 6px 0;
      }

      .prize-desc {
        color: #ccc;
        font-size: 14px;
        margin: 0;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <h1>Колесо Фортуны</h1>
      <p>Крути и забирай лут.</p>

      <div class="wheel-wrapper">
        <div class="pointer"></div>
        <canvas id="wheelCanvas"></canvas>
      </div>

      <button id="spinBtn">КРУТИТЬ</button>

      <div id="resultBox" class="result-box">
        <div class="prize-name" id="resultText">Приз</div>
        <p class="prize-desc" id="resultDesc">Описание приза</p>
      </div>
    </div>

    <script>
      const tg = window.Telegram.WebApp;
      tg.ready();

      const canvas = document.getElementById('wheelCanvas');
      const ctx = canvas.getContext('2d');
      const spinBtn = document.getElementById('spinBtn');
      const resultBox = document.getElementById('resultBox');
      const resultText = document.getElementById('resultText');
      const resultDesc = document.getElementById('resultDesc');

      // Призы в «выживальческом» духе (Minecraft / 7 Days to Die)
      const prizes = [
        { prize: 'Деревянная кирка', desc: 'Базовый инструмент для добычи', color: '#8B5E3C' },
        { prize: 'Железный слиток', desc: 'Нужен для крафта брони и оружия', color: '#5e6b7a' },
        { prize: 'Семена пшеницы', desc: 'Для фермы и еды', color: '#7F9A55' },
        { prize: 'Зелье здоровья', desc: '+50 HP, восстанавливает раны', color: '#c12f2f' },
        { prize: 'Карта сокровищ', desc: 'Указывает на редкий лут', color: '#f1c40f' },
        { prize: 'Алмазный блок', desc: 'Редкий ресурс для элитного крафта', color: '#3498db' },
        { prize: 'Набор еды', desc: 'Сытость +30%, снижает голод', color: '#e67e22' },
        { prize: 'Чертеж базы', desc: 'Схема постройки укреплённого дома', color: '#9b59b6' }
      ];

      let isSpinning = false;

      function resize() {
        const size = Math.min(window.innerWidth, 320);
        canvas.width = size;
        canvas.height = size;
        drawWheel();
      }
      window.addEventListener('resize', resize);
      resize();

      function drawWheel() {
        const sectors = prizes.length;
        const radius = canvas.width / 2;
        const angle = (2 * Math.PI) / sectors;

        for (let i = 0; i < sectors; i++) {
          ctx.beginPath();
          ctx.fillStyle = prizes[i].color;
          ctx.moveTo(radius, radius);
          ctx.arc(radius, radius, radius, i * angle, (i + 1) * angle);
          ctx.lineTo(radius, radius);
          ctx.fill();

          // Обводка между секторами для «металлического» вида
          ctx.strokeStyle = 'rgba(0,0,0,0.25)';
          ctx.lineWidth = 2;
          ctx.stroke();

          // Текст приза (поворачиваем и центрируем)
          ctx.save();
          ctx.translate(radius, radius);
          ctx.rotate(i * angle + angle / 2);
          ctx.textAlign = 'right';
          ctx.fillStyle = '#fff';
          ctx.font = 'bold 14px Arial';
          const text = prizes[i].prize;
          ctx.fillText(text, radius - 14, 6);
          ctx.restore();
        }
      }

      spinBtn.addEventListener('click', () => {
        if (isSpinning) return;
        isSpinning = true;
        spinBtn.disabled = true;
        resultBox.style.display = 'none';

        const randomIndex = Math.floor(Math.random() * prizes.length);
        const prize = prizes[randomIndex];

        // Вращение: 6–10 оборотов + точный сектор
        const spins = 6 + Math.random() * 4;
        const degreesPerSpin = 360;
        const totalDegrees = spins * degreesPerSpin + (randomIndex * (360 / prizes.length));
        const duration = 4000 + Math.random() * 2000; // 4–6 секунд

        const startTime = performance.now();

        function animate(currentTime) {
          const elapsed = currentTime - startTime;
          const progress = Math.min(elapsed / duration, 1);

          // Ease-out для реалистичного замедления
          const ease = 1 - Math.pow(1 - progress, 3);

          const currentDegrees = totalDegrees * ease;
          canvas.style.transform = `rotate(${currentDegrees}deg)`;

          if (progress < 1) {
            requestAnimationFrame(animate);
          } else {
            isSpinning = false;
            spinBtn.disabled = false;
            resultText.textContent = prize.prize;
            resultDesc.textContent = prize.desc;
            resultBox.style.display = 'block';

            // Отправляем результат в бота
            const payload = JSON.stringify({
              action: 'wheel_result',
              prize_id: randomIndex,
              prize: prize.prize,
              desc: prize.desc
            });
            tg.sendData(payload);
          }
        }
        requestAnimationFrame(animate);
      });
    </script>
  </body>
</html>
