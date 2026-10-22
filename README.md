<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>ЗАВТРА ПОВЕЗЁТ — DUKE ROULETTE</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Unbounded:wght@600;800&family=Roboto+Condensed:wght@400;700&display=swap');

  :root {
    --bg: #ebebe3;
    --bg-dark: #2b2d22;
    --olive: #5c6d43;
    --mustard: #cba14b;
    --mustard-dark: #a87b1e;
    --brick: #9f4128;
    --sand: #f4eeda;
    --wood: #5c3e25;
    --text: #222;
    --text-muted: #6b6b63;
    --border: #222;
  }

  * { margin:0; padding:0; box-sizing:border-box; -webkit-tap-highlight-color: transparent; }
  
  body {
    font-family: 'Roboto Condensed', sans-serif;
    background: var(--bg);
    background-image: 
      radial-gradient(circle at 50% 0%, rgba(203,161,75,0.15) 0%, transparent 60%),
      repeating-linear-gradient(45deg, rgba(0,0,0,0.02) 0px, rgba(0,0,0,0.02) 2px, transparent 2px, transparent 4px);
    color: var(--text);
    min-height: 100vh;
    display: flex;
    justify-content: center;
    padding-bottom: 80px;
    overflow-x: hidden;
  }

  .app-container { width: 100%; max-width: 480px; position: relative; }

  /* ШАПКА */
  .topbar {
    background: var(--bg-dark); color: var(--mustard);
    padding: 14px 20px; display: flex; justify-content: space-between; align-items: center;
    font-family: 'Unbounded', sans-serif; font-size: 18px; font-weight: 800; letter-spacing: 1px;
    position: sticky; top: 0; z-index: 50; box-shadow: 0 4px 10px rgba(0,0,0,0.3);
  }
  .topbar .logo { display: flex; align-items: center; gap: 8px; }
  .topbar .wallet {
    font-family: 'Roboto Condensed', sans-serif; font-size: 18px; font-weight: 700;
    background: var(--mustard); padding: 4px 12px; border-radius: 4px; color: var(--bg-dark);
    box-shadow: inset 0 -3px 0 rgba(0,0,0,0.2), 0 2px 4px rgba(0,0,0,0.2);
    transition: transform 0.1s;
  }
  .wallet.bump { transform: scale(1.1); color: #fff; background: var(--olive); }

  .sound-toggle { background: transparent; border: none; cursor: pointer; padding: 0 10px; color: var(--mustard); display: flex; align-items: center; }
  .sound-toggle svg { width: 24px; height: 24px; fill: currentColor; }

  /* ТАБЫ */
  .tabs {
    display: flex; justify-content: space-between; background: #e3e1d4; padding: 0 10px;
    border-bottom: 2px solid var(--border); position: sticky; top: 56px; z-index: 49; overflow-x: auto;
    box-shadow: 0 2px 6px rgba(0,0,0,0.05);
  }
  .tab {
    padding: 14px 10px; font-family: 'Unbounded', sans-serif; font-size: 11px; font-weight: 800;
    color: var(--text-muted); cursor: pointer; white-space: nowrap;
    border-bottom: 3px solid transparent; margin-bottom: -2px; transition: all 0.2s;
  }
  .tab.active { color: var(--text); border-bottom-color: var(--text); }

  /* КОНТЕНТ */
  .panel { display: none; padding: 20px; position: relative; }
  .panel.active { display: block; }

  h2 { 
    font-family: 'Unbounded', sans-serif; font-size: 24px; font-weight: 800;
    text-transform: uppercase; margin-bottom: 16px; line-height: 1.1; letter-spacing: -0.5px;
    text-shadow: 2px 2px 0 rgba(0,0,0,0.1);
  }

  .divider {
    height: 12px; width: 100%;
    background: repeating-linear-gradient(-45deg, var(--mustard), var(--mustard) 12px, var(--bg-dark) 12px, var(--bg-dark) 24px);
    margin: 20px 0; border: 1px solid var(--border); box-shadow: inset 0 2px 4px rgba(0,0,0,0.3);
  }

  /* ФОН РУЛЕТКИ */
  .roulette-bg {
    position: relative;
    background: #2a251c;
    background-image: 
      radial-gradient(circle at 50% 100%, rgba(203,161,75,0.2) 0%, transparent 70%),
      url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100"><path d="M0 80 L20 60 L40 70 L60 50 L80 65 L100 55 L100 100 L0 100 Z" fill="%232a251c" opacity="0.5"/></svg>');
    background-size: cover;
    border: 4px solid var(--mustard-dark);
    border-radius: 8px;
    padding: 20px 10px;
    margin-bottom: 20px;
    box-shadow: inset 0 0 40px rgba(0,0,0,0.8), 0 10px 20px rgba(0,0,0,0.3);
    overflow: hidden;
  }
  .roulette-bg::before {
    content: '☢️'; position: absolute; top: 10px; left: 10px; font-size: 40px; opacity: 0.1;
    transform: rotate(-15deg);
  }
  .roulette-bg::after {
    content: ''; position: absolute; bottom: 0; left: 0; width: 100%; height: 40%;
    background: linear-gradient(to top, rgba(0,0,0,0.8), transparent);
    pointer-events: none;
  }

  .ash-particle {
    position: absolute; background: rgba(255,255,255,0.2); border-radius: 50%;
    pointer-events: none; z-index: 1;
    animation: ashFall linear infinite;
  }
  @keyframes ashFall {
    0% { transform: translateY(-20px) translateX(0); opacity: 0; }
    20% { opacity: 1; }
    80% { opacity: 1; }
    100% { transform: translateY(350px) translateX(20px); opacity: 0; }
  }

  .wheel-wrap { position: relative; width: 100%; display: flex; justify-content: center; padding: 10px 0; z-index: 2; }
  .wheel-frame {
    position: relative; width: 300px; height: 300px; border-radius: 50%;
    border: 12px solid #7d8278;
    box-shadow: 0 15px 25px rgba(0,0,0,0.6), inset 0 0 0 4px #4a4e46, inset 0 0 0 10px #a0a59a, inset 0 0 15px rgba(0,0,0,0.8);
    background: var(--wood); padding: 6px;
  }
  #wheelCanvas { width: 100%; height: 100%; display: block; border-radius: 50%; box-shadow: inset 0 0 20px rgba(0,0,0,0.8); }
  
  .pointer {
    position: absolute; top: -8px; left: 50%; transform: translateX(-50%);
    width: 0; height: 0; border-left: 18px solid transparent; border-right: 18px solid transparent;
    border-top: 28px solid var(--bg-dark); z-index: 10; filter: drop-shadow(0 4px 2px rgba(0,0,0,0.8));
  }
  .pointer::after {
    content:''; position:absolute; top:-32px; left:-14px; width:28px; height:28px;
    background:var(--bg-dark); border-radius:50%; box-shadow: inset 0 -3px 5px rgba(255,255,255,0.2);
  }

  .wheel-center {
    position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
    width: 60px; height: 60px; background: var(--bg-dark); border: 4px solid #7d8278; border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    box-shadow: inset 0 4px 10px rgba(0,0,0,0.8), 0 4px 8px rgba(0,0,0,0.6); z-index: 5;
  }
  .wheel-center svg { width: 28px; height: 28px; fill: var(--mustard); filter: drop-shadow(0 0 4px rgba(203,161,75,0.5)); }

  .result-plaque {
    background: var(--sand); border: 2px solid var(--border);
    box-shadow: 4px 4px 0 rgba(0,0,0,0.15), inset 0 2px 4px rgba(255,255,255,0.5);
    padding: 12px 20px; text-align: center; font-family: 'Unbounded', sans-serif;
    font-weight: 800; font-size: 16px; margin-top: 10px; text-transform: uppercase;
    position: relative; min-height: 48px; display: flex; align-items: center; justify-content: center;
    border-radius: 2px; z-index: 2;
  }

  .controls { display: flex; gap: 10px; margin-top: 16px; align-items: stretch; position: relative; z-index: 2; }
  .bet-input {
    width: 80px; background: var(--sand); border: 2px solid var(--border);
    font-family: 'Roboto Condensed', sans-serif; font-size: 18px; font-weight: 700;
    text-align: center; color: var(--text); outline: none; box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
  }
  .btn {
    flex: 1; background: var(--mustard); border: 2px solid var(--border);
    font-family: 'Unbounded', sans-serif; font-weight: 800; font-size: 14px;
    color: var(--text); text-transform: uppercase; cursor: pointer;
    box-shadow: 0 5px 0 var(--border), 0 8px 10px rgba(0,0,0,0.2);
    transition: all 0.1s; display: flex; align-items: center; justify-content: center; gap: 6px;
    padding: 12px; border-radius: 4px;
  }
  .btn:active:not(:disabled) { transform: translateY(5px); box-shadow: 0 0 0 var(--border), 0 2px 4px rgba(0,0,0,0.2); }
  .btn:disabled { background: #ccc; box-shadow: 0 5px 0 #999; border-color: #999; cursor: not-allowed; color: #777; }
  .btn.dark { background: var(--bg-dark); color: var(--mustard); }
  .btn.small { flex: none; padding: 12px 16px; }
  .btn.craft { background: var(--olive); color: #fff; }

  /* ИНВЕНТАРЬ */
  .inventory-box {
    margin-top: 10px; background: #dcd9cc; border: 2px solid var(--border);
    box-shadow: 4px 4px 0 rgba(0,0,0,0.15), inset 0 0 20px rgba(0,0,0,0.1);
    border-radius: 4px; padding: 12px; min-height: 80px;
  }
  .inventory-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
  .inventory-title { font-family: 'Unbounded', sans-serif; font-weight: 800; font-size: 14px; color: var(--bg-dark); display: flex; align-items: center; gap: 6px; }
  .inventory-grid { display: flex; flex-wrap: wrap; gap: 8px; }
  .inv-item {
    width: 50px; height: 50px; background: var(--sand); border: 2px solid var(--border);
    box-shadow: 2px 2px 0 rgba(0,0,0,0.2); border-radius: 4px;
    display: flex; flex-direction: column; align-items: center; justify-content: center; position: relative;
  }
  .inv-item .inv-icon { font-size: 24px; line-height: 1; }
  .inv-item .inv-count {
    position: absolute; bottom: -4px; right: -4px; background: var(--brick); color: #fff;
    font-family: 'Unbounded', sans-serif; font-weight: 800; font-size: 10px; padding: 2px 5px;
    border-radius: 10px; border: 1px solid #fff; box-shadow: 0 2px 4px rgba(0,0,0,0.3);
  }
  .inv-empty { color: var(--text-muted); font-size: 13px; font-style: italic; width: 100%; text-align: center; padding: 10px 0; }

  /* РЕКЛАМНЫЙ БАННЕР */
  .ad-banner {
    margin-top: 30px; margin-bottom: 20px; background: #2a2520;
    border: 3px dashed #555; border-radius: 6px; padding: 20px 15px; text-align: center;
    position: relative; overflow: hidden; box-shadow: inset 0 0 30px rgba(0,0,0,0.8);
  }
  .ad-banner::before {
    content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
    background: repeating-linear-gradient(-45deg, transparent, transparent 10px, rgba(0,0,0,0.2) 10px, rgba(0,0,0,0.2) 20px);
    z-index: 0;
  }
  .ad-banner .ad-text {
    position: relative; z-index: 1; font-family: 'Unbounded', sans-serif; font-weight: 800;
    font-size: 13px; color: #777; text-transform: uppercase; line-height: 1.4; text-shadow: 1px 1px 0 #000;
  }
  .ad-banner .ad-text span { color: var(--brick); }

  /* ПРИЗЫ */
  .prizes-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
  .prize-card {
    background: var(--bg); border: 2px solid var(--border);
    box-shadow: 4px 4px 0 rgba(0,0,0,0.15), inset 0 0 10px rgba(255,255,255,0.5);
    padding: 12px; display: flex; flex-direction: column; position: relative; border-radius: 2px;
  }
  .prize-chance { position: absolute; top: 12px; right: 12px; font-family: 'Unbounded', sans-serif; font-weight: 800; font-size: 16px; }
  .prize-mult { font-size: 11px; font-weight: 700; color: var(--mustard-dark); margin-bottom: 8px; text-transform: uppercase; }
  .prize-icon {
    width: 48px; height: 48px; display: flex; align-items: center; justify-content: center;
    margin-bottom: 10px; border: 1px solid var(--border);
    box-shadow: inset 0 0 10px rgba(0,0,0,0.2), 2px 2px 0 rgba(0,0,0,0.2);
    border-radius: 2px; font-size: 24px;
  }
  .prize-name { font-family: 'Unbounded', sans-serif; font-weight: 800; font-size: 12px; line-height: 1.2; margin-bottom: 8px; text-transform: uppercase; flex: 1; }
  .prize-tag { display: inline-block; font-size: 10px; font-weight: 700; padding: 3px 6px; text-transform: uppercase; border-radius: 2px; align-self: flex-start; box-shadow: inset 0 -2px 0 rgba(0,0,0,0.2); }
  .tag-legend { background: var(--mustard); color: var(--text); }
  .tag-rare { background: var(--olive); color: #fff; }
  .tag-common { background: #dcd9cc; color: var(--text); }
  .tag-empty { background: transparent; border: 1px solid var(--text-muted); color: var(--text-muted); }

  .bg-mustard { background: var(--mustard); }
  .bg-olive { background: var(--olive); }
  .bg-brick { background: var(--brick); }
  .bg-sand { background: var(--sand); }

  /* МАГАЗИН */
  .shop-grid { display: flex; flex-direction: column; gap: 12px; }
  .shop-item {
    display: flex; gap: 14px; align-items: center; background: var(--sand); border: 2px solid var(--border);
    padding: 12px; box-shadow: 4px 4px 0 rgba(0,0,0,0.15), inset 0 0 10px rgba(255,255,255,0.5); border-radius: 2px;
    position: relative;
  }
  .shop-item .icon { font-size: 28px; width: 56px; height: 56px; display: flex; align-items: center; justify-content: center; background: #fff; border: 2px solid var(--border); box-shadow: inset 0 0 10px rgba(0,0,0,0.1); }
  .shop-item .info { flex: 1; display: flex; flex-direction: column; gap: 4px; }
  .shop-item .name { font-family: 'Unbounded', sans-serif; font-weight: 800; font-size: 13px; }
  .shop-item .desc { font-size: 12px; color: var(--text-muted); }
  .shop-item .cost-row { display: flex; gap: 8px; align-items: center; font-size: 12px; font-weight: 700; color: var(--bg-dark); margin-top: 4px; flex-wrap: wrap; }
  .shop-item .cost-row span { display: flex; align-items: center; gap: 2px; }
  .shop-item .buy-btn { 
    background: var(--bg-dark); color: var(--mustard); padding: 8px 12px; font-size: 12px; 
    box-shadow: 0 4px 0 #000; border: none; flex:none; min-width: 80px;
  }
  .shop-item .buy-btn.craft { background: var(--olive); color: #fff; box-shadow: 0 4px 0 #2e3a22; }
  .shop-item .buy-btn:disabled { background: #ccc; color: #777; box-shadow: 0 4px 0 #999; }

  /* ЗАРАБОТОК */
  .earn-tabs { display: flex; gap: 8px; margin-bottom: 16px; flex-wrap: wrap; }
  .earn-tabs .btn { font-size: 11px; padding: 10px 8px; background: var(--bg); color: var(--text); border: 2px solid var(--border); box-shadow: 0 4px 0 var(--border); }
  .earn-tabs .btn.active { background: var(--mustard); }
  .mini-game-container { display: none; }
  .mini-game-container.active { display: block; }

  /* ЗОМБИ */
  .zombie-field {
    height: 300px; border: 2px solid var(--border); background: #dcd9cc;
    position: relative; overflow: hidden; box-shadow: inset 0 0 20px rgba(0,0,0,0.2);
    margin-top: 14px; background-image: radial-gradient(circle, #d0cdbf 1px, transparent 1px); background-size: 20px 20px;
    cursor: none; /* Скрываем стандартный курсор */
  }
  
  /* Снайперский прицел */
  .crosshair {
    position: absolute; width: 40px; height: 40px;
    pointer-events: none; z-index: 10;
    transform: translate(-50%, -50%);
    transition: transform 0.05s;
  }
  .crosshair::before, .crosshair::after {
    content: ''; position: absolute; background: rgba(200, 40, 40, 0.8);
  }
  .crosshair::before { top: 50%; left: 0; width: 100%; height: 2px; transform: translateY(-50%); }
  .crosshair::after { top: 0; left: 50%; width: 2px; height: 100%; transform: translateX(-50%); }
  .crosshair .circle {
    position: absolute; top: 50%; left: 50%; width: 14px; height: 14px;
    border: 2px solid rgba(200, 40, 40, 0.8); border-radius: 50%;
    transform: translate(-50%, -50%);
  }
  /* Отдача */
  .crosshair.recoil { transform: translate(-40%, -60%) scale(1.2); }

  .zombie { position: absolute; font-size: 40px; cursor: none; filter: drop-shadow(2px 4px 2px rgba(0,0,0,0.4)); transition: transform 0.1s; }

  /* Патроны */
  .ammo-counter {
    position: absolute; bottom: 10px; right: 15px; font-family: 'Unbounded', sans-serif;
    font-weight: 800; font-size: 18px; color: var(--brick); z-index: 5;
    text-shadow: 1px 1px 0 #fff;
  }
  .ammo-counter.low { color: var(--mustard-dark); }
  .ammo-counter.empty { color: var(--text-muted); animation: blink 0.5s infinite; }
  @keyframes blink { 50% { opacity: 0.3; } }

  /* СЛОТЫ */
  .slot-machine {
    background: var(--bg-dark); border: 4px solid var(--mustard-dark);
    border-radius: 8px; padding: 16px; box-shadow: inset 0 0 30px rgba(0,0,0,0.8), 0 10px 20px rgba(0,0,0,0.3);
    margin-top: 10px;
  }
  .slot-reels {
    display: flex; gap: 10px; justify-content: center; background: #111; padding: 10px; border-radius: 4px;
    box-shadow: inset 0 20px 20px -10px rgba(0,0,0,0.9), inset 0 -20px 20px -10px rgba(0,0,0,0.9);
    position: relative;
  }
  .slot-reels::after {
    content: ''; position: absolute; top: 0; left: 0; right: 0; bottom: 0;
    background: linear-gradient(to bottom, rgba(255,255,255,0.1) 0%, transparent 20%, transparent 80%, rgba(255,255,255,0.1) 100%);
    pointer-events: none;
  }
  
  .reel {
    width: 80px; height: 100px; border-radius: 4px; border: 2px solid #444;
    display: flex; align-items: center; justify-content: center; font-size: 50px;
    transition: filter 0.1s, box-shadow 0.3s;
    background-color: #2a2a2a;
    background-image: 
      linear-gradient(45deg, #333 25%, transparent 25%, transparent 75%, #333 75%, #333),
      linear-gradient(45deg, #333 25%, transparent 25%, transparent 75%, #333 75%, #333);
    background-size: 20px 20px;
    background-position: 0 0, 10px 10px;
    box-shadow: inset 0 0 20px rgba(0,0,0,0.8);
  }
  .reel.spinning { filter: blur(4px); }
  .reel.win { 
    border-color: var(--mustard); 
    box-shadow: 0 0 15px var(--mustard), inset 0 0 20px var(--mustard); 
    background-color: #3a2f1a;
  }
  .reel.jackpot { 
    animation: flashJackpot 0.3s infinite alternate; 
    border-color: var(--mustard);
  }
  @keyframes flashJackpot {
    0% { box-shadow: 0 0 10px var(--mustard), inset 0 0 20px var(--mustard); background-color: #3a2f1a; }
    100% { box-shadow: 0 0 30px #fff, inset 0 0 30px #fff; background-color: var(--mustard); }
  }
  .slot-result {
    text-align: center; font-family: 'Unbounded', sans-serif; font-weight: 800;
    color: var(--mustard); font-size: 16px; margin-top: 14px; min-height: 24px;
    text-transform: uppercase; transition: transform 0.1s;
  }
  .slot-result.pop { animation: popResult 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
  @keyframes popResult {
    0% { transform: scale(0.8); opacity: 0; }
    50% { transform: scale(1.1); opacity: 1; }
    100% { transform: scale(1); opacity: 1; }
  }

  /* РАННЕР (Разведка) */
  .runner-field {
    height: 220px; border: 2px solid var(--border); background: #dcd9cc;
    position: relative; overflow: hidden; box-shadow: inset 0 0 20px rgba(0,0,0,0.2);
    margin-top: 14px; cursor: pointer;
    background-image: 
      linear-gradient(to bottom, #c8d0d8 0%, #e0d8c8 60%, #dcd9cc 100%),
      url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="200" height="100" viewBox="0 0 200 100"><path d="M0 100 L0 50 L20 50 L20 30 L30 30 L30 50 L50 50 L50 20 L60 20 L60 50 L80 50 L80 40 L100 40 L100 10 L110 10 L110 40 L140 40 L140 60 L150 60 L150 20 L160 20 L160 60 L180 60 L180 45 L200 45 L200 100 Z" fill="%23a0a59a" opacity="0.3"/></svg>');
    background-size: cover, 200px 100px;
    background-position: center, bottom;
    background-repeat: no-repeat, repeat-x;
  }
  
  .runner-field::after {
    content: ''; position: absolute; top: 0; left: 0; width: 200%; height: 100%;
    background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="200" height="100" viewBox="0 0 200 100"><path d="M0 100 L0 60 L10 60 L10 40 L20 40 L20 60 L40 60 L40 50 L50 50 L50 60 L70 60 L70 30 L80 30 L80 60 L100 60 L100 55 L120 55 L120 60 L150 60 L150 35 L160 35 L160 60 L180 60 L180 50 L200 50 L200 100 Z" fill="%23808080" opacity="0.2"/></svg>');
    background-size: 200px 100px;
    background-repeat: repeat-x;
    animation: bgScroll 10s linear infinite;
    z-index: 0;
  }
  @keyframes bgScroll {
    0% { transform: translateX(0); }
    100% { transform: translateX(-50%); }
  }

  .runner-ground {
    position: absolute; bottom: 0; left: 0; width: 100%; height: 40px;
    background: var(--wood);
    border-top: 4px solid #3a2516;
    box-shadow: inset 0 10px 20px rgba(0,0,0,0.5);
    z-index: 1;
  }
  .runner-ground::after {
    content: ''; position: absolute; top: 10px; left: 0; width: 100%; height: 2px;
    background: repeating-linear-gradient(90deg, transparent, transparent 20px, rgba(255,255,255,0.1) 20px, rgba(255,255,255,0.1) 40px);
    animation: groundMove 0.8s linear infinite;
  }
  @keyframes groundMove {
    0% { transform: translateX(0); }
    100% { transform: translateX(-40px); }
  }

  .runner-player {
    position: absolute; bottom: 40px; left: 50px; font-size: 36px;
    transform: scaleX(-1);
    z-index: 2;
    filter: drop-shadow(2px 4px 2px rgba(0,0,0,0.5));
  }
  .runner-player.running {
    animation: runBob 0.3s infinite alternate;
  }
  @keyframes runBob {
    0% { transform: scaleX(-1) translateY(0px); }
    100% { transform: scaleX(-1) translateY(-5px); }
  }

  .runner-obstacle {
    position: absolute; bottom: 40px; font-size: 36px; z-index: 1;
    filter: drop-shadow(2px 4px 2px rgba(0,0,0,0.5));
  }
  .runner-score {
    position: absolute; top: 10px; right: 15px; font-family: 'Unbounded', sans-serif;
    font-weight: 800; font-size: 20px; color: var(--text-muted);
    text-shadow: 1px 1px 0 #fff; z-index: 3;
  }

  /* АВИАТОР (ПОЛЁТ) */
  .aviator-field {
    height: 240px; border: 2px solid var(--border);
    position: relative; overflow: hidden;
    margin-top: 14px;
    background: linear-gradient(to bottom, #1a2a3a 0%, #2a4a5a 60%, #1a3a4a 100%);
    border-radius: 4px;
  }
  .aviator-water {
    position: absolute; bottom: 0; left: 0; width: 100%; height: 40px;
    background: linear-gradient(to bottom, #0a2a4a, #051a2a);
    border-top: 2px solid #3a8a9a;
    box-shadow: inset 0 10px 20px rgba(0,0,0,0.8);
    z-index: 1;
  }
  .aviator-water::after {
    content: ''; position: absolute; top: 0; left: 0; width: 200%; height: 100%;
    background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="20" viewBox="0 0 100 20"><path d="M0 10 Q 25 0, 50 10 T 100 10" fill="none" stroke="rgba(255,255,255,0.2)" stroke-width="2"/></svg>');
    background-size: 100px 20px;
    animation: waterMove 2s linear infinite;
  }
  @keyframes waterMove {
    0% { transform: translateX(0); }
    100% { transform: translateX(-100px); }
  }

  .aviator-plane {
    position: absolute; font-size: 40px; z-index: 5;
    transition: transform 0.1s linear;
    filter: drop-shadow(0 5px 5px rgba(0,0,0,0.5));
  }

  .aviator-multiplier {
    position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
    font-family: 'Unbounded', sans-serif; font-weight: 800; font-size: 48px;
    color: var(--mustard); text-shadow: 0 0 20px rgba(203,161,75,0.8);
    z-index: 10; pointer-events: none;
    opacity: 0; transition: opacity 0.3s;
  }
  .aviator-multiplier.active { opacity: 1; }

  .aviator-missile {
    position: absolute; font-size: 30px; z-index: 6;
    filter: drop-shadow(0 0 10px rgba(255,0,0,0.8));
  }
  
  .explosion {
    position: absolute; font-size: 60px; z-index: 7;
    animation: explode 0.5s ease-out forwards;
  }
  @keyframes explode {
    0% { transform: scale(0.5); opacity: 1; }
    100% { transform: scale(2); opacity: 0; }
  }

  /* ШАХТА (Майнкрафт) */
  .mine-field {
    height: 240px; border: 2px solid var(--border);
    position: relative; overflow: hidden;
    margin-top: 14px;
    background: #1a1a1a;
    border-radius: 4px;
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    gap: 10px;
  }
  .mine-grid {
    display: flex; gap: 8px; margin-bottom: 10px;
  }
  .mine-cell {
    width: 50px; height: 50px; background: #333;
    border: 2px solid #555; display: flex; align-items: center; justify-content: center;
    font-size: 24px; border-radius: 4px;
  }
  .mine-cell.revealed { background: #2a2a2a; border-color: #444; }
  .mine-cell.dynamite { background: #8a1a1a; border-color: #ff4444; animation: explode 0.5s; }
  .mine-cell.treasure { background: #3a5a2a; border-color: #4ade80; }
  
  .mine-multiplier {
    font-family: 'Unbounded', sans-serif; font-weight: 800; font-size: 32px;
    color: var(--mustard); text-shadow: 0 0 15px rgba(203,161,75,0.8);
  }
  .mine-log {
    position: absolute; bottom: 10px; left: 10px; right: 10px;
    font-family: 'Roboto Condensed', sans-serif; font-size: 12px; color: #aaa;
    text-align: center;
  }

  /* ВЗЛОМ */
  .code-display { font-family: 'Unbounded', sans-serif; font-size: 42px; font-weight: 800; letter-spacing: 10px; text-align: center; margin: 20px 0; background: var(--sand); padding: 16px; border: 2px solid var(--border); box-shadow: inset 0 4px 8px rgba(0,0,0,0.1); }
  .keypad { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-top: 16px; }
  .keypad .btn { padding: 16px; font-size: 20px; }

  /* УВЕДОМЛЕНИЯ */
  .toast {
    position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%) translateY(100px);
    background: var(--bg-dark); color: var(--mustard);
    padding: 12px 24px; font-family: 'Unbounded', sans-serif; font-weight: 800; font-size: 13px;
    border: 2px solid var(--border); box-shadow: 0 10px 20px rgba(0,0,0,0.3);
    opacity: 0; transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    z-index: 100; pointer-events: none; text-align: center; border-radius: 4px;
  }
  .toast.show { transform: translateX(-50%) translateY(0); opacity: 1; }
  .toast.error { background: var(--brick); color: #fff; }
</style>
</head>
<body>

<div class="app-container">
  
  <div class="topbar">
    <div class="logo">
      <svg viewBox="0 0 24 24" width="24" height="24" fill="var(--mustard)"><path d="M12 2L2 7l10 5 10-5-10-5zm0 9l2.5-1.25L12 8.5l-2.5 1.25L12 11zm0 2.5l-5-2.5-5 2.5L12 22l10-8.5-5-2.5-5 2.5z"/></svg>
      ЗАВТРА ПОВЕЗЁТ
    </div>
    <div style="display:flex; align-items:center;">
      <button class="sound-toggle" id="soundToggle" title="Звук">
        <svg id="soundIcon" viewBox="0 0 24 24"><path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77s-2.99-7.86-7-8.77z"/></svg>
      </button>
      <div class="wallet" id="walletBox"><span id="dukes">0</span></div>
    </div>
  </div>

  <div class="tabs">
    <div class="tab active" data-panel="roulette">КОЛЕСО</div>
    <div class="tab" data-panel="prizes">ПРИЗЫ</div>
    <div class="tab" data-panel="shop">МАГАЗИН</div>
    <div class="tab" data-panel="earn">ЗАРАБОТОК</div>
  </div>

  <!-- ВКЛАДКА: РУЛЕТКА -->
  <div class="panel active" id="panel-roulette">
    
    <div class="roulette-bg" id="rouletteBg">
      <div class="wheel-wrap">
        <div class="wheel-frame">
          <div class="pointer"></div>
          <canvas id="wheelCanvas" width="600" height="600"></canvas>
          <div class="wheel-center">
            <svg viewBox="0 0 24 24"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8zm-1-13h2v6h-2zm0 8h2v2h-2z"/></svg>
          </div>
        </div>
      </div>

      <div class="result-plaque" id="resultBox">СДЕЛАЙ СТАВКУ</div>

      <div class="controls">
        <button class="btn small dark" id="allInBtn" title="Поставить всё">MAX</button>
        <input type="number" class="bet-input" id="betAmount" value="10" min="1">
        <button class="btn" id="spinBtn">КРУТИТЬ</button>
      </div>
    </div>

    <div class="divider"></div>
    <h2>ДОБЫЧА ДНЯ</h2>
    <div style="font-family:'Roboto Condensed'; font-size:14px; color:var(--text-muted); line-height:1.5;">
      Твоя удача сегодня определяет, выживешь ли ты завтра. Крути колесо и собирай припасы.
    </div>

    <div class="inventory-box">
      <div class="inventory-header">
        <div class="inventory-title">
          <svg viewBox="0 0 24 24" width="18" height="18" fill="currentColor"><path d="M20 6h-4V4c0-1.11-.89-2-2-2h-4c-1.11 0-2 .89-2 2v2H4c-1.11 0-1.99.89-1.99 2L2 19c0 1.11.89 2 2 2h16c1.11 0 2-.89 2-2V8c0-1.11-.89-2-2-2zm-6 0h-4V4h4v2z"/></svg>
          РЮКЗАК
        </div>
        <button class="btn small dark" id="sellAllBtn" style="font-size:10px; padding:6px 10px; box-shadow:0 3px 0 #000;">ПРОДАТЬ ВСЁ</button>
      </div>
      <div class="inventory-grid" id="inventoryGrid">
        <div class="inv-empty">Рюкзак пуст. Крути колесо!</div>
      </div>
    </div>

    <div class="ad-banner">
      <div class="ad-text">
        ТУТ МОГЛА БЫТЬ ВАША РЕКЛАМА,<br>
        <span>НО ВЫ НЕ ГОЙДА</span>
      </div>
    </div>

  </div>

  <!-- ВКЛАДКА: ПРИЗЫ -->
  <div class="panel" id="panel-prizes">
    <h2>ШАНСЫ И МНОЖИТЕЛИ</h2>
    <div class="prizes-grid" id="prizesGrid"></div>
  </div>

  <!-- ВКЛАДКА: МАГАЗИН -->
  <div class="panel" id="panel-shop">
    <h2>ТОРГОВАЯ ЛАВКА</h2>
    <div class="shop-grid" id="shopGrid"></div>
  </div>

  <!-- ВКЛАДКА: ЗАРАБОТОК -->
  <div class="panel" id="panel-earn">
    <h2>ЗАРАБОТОК</h2>
    <div class="earn-tabs">
      <button class="btn active" id="tab-zombie">ЗОМБИ</button>
      <button class="btn" id="tab-slots">СЛОТЫ</button>
      <button class="btn" id="tab-runner">РАЗВЕДКА</button>
      <button class="btn" id="tab-aviator">ПОЛЁТ</button>
      <button class="btn" id="tab-mine">ШАХТА</button>
      <button class="btn" id="tab-hack">СЕЙФ</button>
    </div>

    <!-- Игра: Зомби -->
    <div class="mini-game-container active" id="game-zombie">
      <div style="font-size:14px; color:var(--text-muted); margin-bottom:10px;">
        Уничтожай зомби за 30 секунд. Промах — штраф.
      </div>
      <div style="display:flex; gap:16px; font-family:'Unbounded'; font-weight:800; font-size:16px;">
        <div>Время: <span id="zTime" style="color:var(--brick)">30</span></div>
        <div>Убито: <span id="zHits" style="color:var(--olive)">0</span></div>
      </div>
      <div class="zombie-field" id="zombieField">
        <div class="crosshair" id="crosshair"><div class="circle"></div></div>
        <div class="ammo-counter" id="ammoCounter">30 / 30</div>
      </div>
      <button class="btn" id="startZombie" style="margin-top:16px; width:100%;">НАЧАТЬ БОЙ</button>
    </div>

    <!-- Игра: Слоты -->
    <div class="mini-game-container" id="game-slots">
      <div style="font-size:14px; color:var(--text-muted); margin-bottom:10px; text-align:center;">
        Собери 3 одинаковых символа. <br> 2 совпадения = x1.5, 3 совпадения = x3. <br> <strong style="color:var(--brick)">⚡⚡⚡ = ДЖЕКПОТ x10!</strong>
      </div>
      
      <div class="slot-machine">
        <div class="slot-reels">
          <div class="reel" id="reel0">🥫</div>
          <div class="reel" id="reel1">🔫</div>
          <div class="reel" id="reel2">💊</div>
        </div>
        <div class="slot-result" id="slotResult">УДАЧИ!</div>
        
        <div style="display:flex; gap:10px; margin-top:16px;">
          <input type="number" class="bet-input" id="slotBet" value="10" min="1" style="flex:1; background:#111; color:#cba14b; border-color:#555;">
          <button class="btn" id="spinSlotsBtn" style="flex:2;">КРУТИТЬ</button>
        </div>
      </div>
    </div>

    <!-- Игра: Раннер (Разведка) -->
    <div class="mini-game-container" id="game-runner">
      <div style="font-size:14px; color:var(--text-muted); margin-bottom:10px; text-align:center;">
        Беги от зомби! Прыгай через препятствия. <br> (Клик по полю или Пробел для прыжка)
      </div>
      <div class="runner-field" id="runnerField">
        <div class="runner-score" id="runnerScore">0</div>
        <div class="runner-ground"></div>
        <div class="runner-player" id="runnerPlayer">🏃</div>
      </div>
      <button class="btn" id="startRunner" style="margin-top:16px; width:100%;">НАЧАТЬ БЕГ</button>
    </div>

    <!-- Игра: Авиатор (ПОЛЁТ) -->
    <div class="mini-game-container" id="game-aviator">
      <div style="font-size:14px; color:var(--text-muted); margin-bottom:10px; text-align:center;">
        Самолёт взлетает. Множитель растёт. <br> Нажми «ЗАБРАТЬ», чтобы посадить его на корабль! <br> Не дай ракете сбить его.
      </div>
      
      <div class="aviator-field" id="aviatorField">
        <div class="aviator-water"></div>
        <div class="aviator-multiplier" id="aviatorMultiplier">x1.00</div>
      </div>

      <div style="display:flex; gap:10px; margin-top:16px;">
        <input type="number" class="bet-input" id="aviatorBet" value="10" min="1" style="flex:1; background:#111; color:#cba14b; border-color:#555;">
        <button class="btn" id="startAviator" style="flex:2;">ВЗЛЁТ</button>
        <button class="btn craft" id="cashoutAviator" style="flex:2; display:none;">ЗАБРАТЬ</button>
      </div>
    </div>

    <!-- Игра: Шахта (Майнкрафт) -->
    <div class="mini-game-container" id="game-mine">
      <div style="font-size:14px; color:var(--text-muted); margin-bottom:10px; text-align:center;">
        Копай глубже! Найди сундук с золотом или подорвись на динамите. <br> С каждым шагом множитель растёт, но риск взрыва увеличивается.
      </div>
      
      <div class="mine-field" id="mineField">
        <div class="mine-grid" id="mineGrid">
          <!-- Ячейки будут добавлены через JS -->
        </div>
        <div class="mine-multiplier" id="mineMultiplier">x1.00</div>
        <div class="mine-log" id="mineLog">Нажми "КОПАТЬ", чтобы начать</div>
      </div>

      <div style="display:flex; gap:10px; margin-top:16px;">
        <input type="number" class="bet-input" id="mineBet" value="10" min="1" style="flex:1; background:#111; color:#cba14b; border-color:#555;">
        <button class="btn" id="startMine" style="flex:2;">КОПАТЬ</button>
        <button class="btn craft" id="cashoutMine" style="flex:2; display:none;">ЗАБРАТЬ</button>
      </div>
      
      <!-- Верстак (Крафт) -->
      <div style="margin-top:16px; padding:12px; background:#2a2a2a; border:2px solid #555; border-radius:4px;">
        <div style="font-family:'Unbounded'; font-weight:800; color:var(--mustard); font-size:14px; margin-bottom:8px;">ВЕРСТАК</div>
        <div style="display:flex; gap:8px; flex-wrap:wrap;" id="mineCraftGrid">
          <!-- Улучшения будут добавлены через JS -->
        </div>
      </div>
    </div>

    <!-- Игра: Сейф -->
    <div class="mini-game-container" id="game-hack">
      <div style="font-size:14px; color:var(--text-muted); margin-bottom:10px;">
        Угадай 4 цифры. 🐂 — на месте, 🐄 — не на месте.
      </div>
      <div class="code-display" id="codeDisplay">_ _ _ _</div>
      <div style="font-family:'Unbounded'; font-weight:800; text-align:center; margin-bottom:16px;" id="codeInfo">ПОПЫТОК: 8</div>
      <div class="keypad" id="keypad"></div>
      <button class="btn dark" id="resetHack" style="width:100%; margin-top:16px;">НОВЫЙ СЕЙФ</button>
    </div>

  </div>
</div>

<div class="toast" id="toast"></div>

<script>
/* ================== АУДИО-ДВИЖОК ================== */
const AudioEngine = {
  ctx: null, enabled: true,
  init() { if (!this.ctx) this.ctx = new (window.AudioContext || window.webkitAudioContext)(); if (this.ctx.state === 'suspended') this.ctx.resume(); },
  playTone(freq, type = 'sine', duration = 0.1, vol = 0.1, slideTo = null) {
    if (!this.enabled) return; this.init();
    const osc = this.ctx.createOscillator(), gain = this.ctx.createGain();
    osc.type = type; osc.frequency.setValueAtTime(freq, this.ctx.currentTime);
    if (slideTo) osc.frequency.exponentialRampToValueAtTime(slideTo, this.ctx.currentTime + duration);
    gain.gain.setValueAtTime(vol, this.ctx.currentTime);
    gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + duration);
    osc.connect(gain); gain.connect(this.ctx.destination);
    osc.start(); osc.stop(this.ctx.currentTime + duration);
  },
  noise(duration = 0.1, vol = 0.1) {
    if (!this.enabled) return; this.init();
    const bufferSize = this.ctx.sampleRate * duration, buffer = this.ctx.createBuffer(1, bufferSize, this.ctx.sampleRate), data = buffer.getChannelData(0);
    for (let i = 0; i < bufferSize; i++) data[i] = Math.random() * 2 - 1;
    const noise = this.ctx.createBufferSource(), gain = this.ctx.createGain();
    noise.buffer = buffer; gain.gain.setValueAtTime(vol, this.ctx.currentTime);
    gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + duration);
    noise.connect(gain); gain.connect(this.ctx.destination); noise.start();
  },
  click() { this.playTone(800, 'square', 0.05, 0.05); },
  tick() { this.playTone(1200, 'triangle', 0.03, 0.04); },
  jump() { this.playTone(400, 'sine', 0.15, 0.1, 800); },
  win() { this.playTone(440, 'sine', 0.15, 0.1); setTimeout(() => this.playTone(554, 'sine', 0.15, 0.1), 100); setTimeout(() => this.playTone(659, 'sine', 0.3, 0.1), 200); },
  jackpot() { [440, 554, 659, 880, 1108].forEach((f, i) => setTimeout(() => this.playTone(f, 'square', 0.4, 0.08), i * 100)); },
  lose() { this.playTone(200, 'sawtooth', 0.4, 0.08, 100); },
  error() { this.playTone(150, 'square', 0.15, 0.08); },
  buy() { this.playTone(880, 'sine', 0.1, 0.08); setTimeout(() => this.playTone(1108, 'sine', 0.2, 0.08), 80); },
  shoot() { this.noise(0.12, 0.2); this.playTone(80, 'sawtooth', 0.15, 0.12, 40); },
  reload() { this.playTone(300, 'square', 0.1, 0.1); setTimeout(() => this.playTone(200, 'square', 0.1, 0.1), 150); setTimeout(() => this.playTone(400, 'square', 0.1, 0.1), 300); },
  reelStop() { this.playTone(300, 'square', 0.1, 0.1); this.playTone(150, 'triangle', 0.15, 0.1); },
  flyUp() { this.playTone(400, 'sine', 2, 0.05, 800); },
  explode() { this.noise(0.5, 0.3); this.playTone(60, 'sawtooth', 0.5, 0.2, 20); },
  cashout() { this.playTone(800, 'sine', 0.1, 0.1); setTimeout(() => this.playTone(1000, 'sine', 0.1, 0.1), 100); setTimeout(() => this.playTone(1200, 'sine', 0.2, 0.1), 200); },
  pickaxe() { this.playTone(600, 'square', 0.1, 0.08); this.noise(0.1, 0.05); }
};

document.getElementById('soundToggle').addEventListener('click', () => {
  AudioEngine.enabled = !AudioEngine.enabled;
  const icon = document.getElementById('soundIcon');
  if (AudioEngine.enabled) { 
    icon.innerHTML = '<path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77s-2.99-7.86-7-8.77z"/>'; 
    AudioEngine.click();
  }
  else { 
    icon.innerHTML = '<path d="M16.5 12c0-1.77-1.02-3.29-2.5-4.03v2.21l2.45 2.45c.03-.2.05-.41.05-.63zm2.5 0c0 .94-.2 1.82-.54 2.64l1.51 1.51C20.63 14.91 21 13.5 21 12c0-4.28-2.99-7.86-7-8.77v2.06c2.89.86 5 3.54 5 6.71zM4.27 3L3 4.27 7.73 9H3v6h4l5 5v-6.73l4.25 4.25c-.67.52-1.42.93-2.25 1.18v2.06c1.38-.31 2.63-.95 3.69-1.81L19.73 21 21 19.73l-9-9L4.27 3zM12 4L9.91 6.09 12 8.18V4z"/>'; 
  }
});

/* ================== СОСТОЯНИЕ ================== */
const SAVE_KEY = 'duke_roulette_save_v15'; // v15 (Добавлена Шахта)
let state = { dukes: 100, owned: {}, luckBonus: 0, inventory: {} };

function loadSave() { 
  try { 
    const raw = localStorage.getItem(SAVE_KEY); 
    if (raw) state = Object.assign(state, JSON.parse(raw)); 
    if (!state.inventory) state.inventory = {}; 
  } catch(e){} 
}
function saveGame() { try { localStorage.setItem(SAVE_KEY, JSON.stringify(state)); } catch(e){} }
function updateWallet(animate=false) {
  document.getElementById('dukes').textContent = Math.floor(state.dukes);
  if (animate) {
    const box = document.getElementById('walletBox');
    box.classList.add('bump');
    setTimeout(() => box.classList.remove('bump'), 200);
  }
}
function toast(msg, isError=false) {
  const t = document.getElementById('toast');
  t.textContent = msg; t.className = 'toast show' + (isError ? ' error' : '');
  if (isError) AudioEngine.error();
  clearTimeout(t._timer); t._timer = setTimeout(() => t.className = 'toast', 2500);
}

/* ================== ТАБЫ ================== */
document.querySelectorAll('.tab').forEach(tab => {
  tab.addEventListener('click', () => {
    AudioEngine.click();
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
    tab.classList.add('active');
    document.getElementById('panel-' + tab.dataset.panel).classList.add('active');
    if (tab.dataset.panel === 'shop') renderShop();
  });
});

function switchMiniGame(gameId) {
  document.querySelectorAll('.earn-tabs .btn').forEach(b => b.classList.remove('active'));
  document.querySelectorAll('.mini-game-container').forEach(c => c.classList.remove('active'));
  document.getElementById('tab-' + gameId).classList.add('active');
  document.getElementById('game-' + gameId).classList.add('active');
}

document.getElementById('tab-zombie').addEventListener('click', () => { AudioEngine.click(); switchMiniGame('zombie'); });
document.getElementById('tab-slots').addEventListener('click', () => { AudioEngine.click(); switchMiniGame('slots'); });
document.getElementById('tab-runner').addEventListener('click', () => { AudioEngine.click(); switchMiniGame('runner'); });
document.getElementById('tab-aviator').addEventListener('click', () => { AudioEngine.click(); switchMiniGame('aviator'); });
document.getElementById('tab-mine').addEventListener('click', () => { AudioEngine.click(); switchMiniGame('mine'); });
document.getElementById('tab-hack').addEventListener('click', () => { AudioEngine.click(); switchMiniGame('hack'); });

/* ================== ДАННЫЕ КОЛЕСА ================== */
const SEGMENTS = [
  { id:'empty_box', label:'ПУСТО',    icon:'box',      color:'#dcd9cc', multiplier:0,   type:'nothing', weight:15, rarity:'empty',   tag:'ПУСТО', emoji: '📦' },
  { id:'canned',    label:'ЕДА',      icon:'canned',   color:'#f4eeda', multiplier:1.5, type:'win',     weight:20, rarity:'common',  tag:'ОБЫЧНЫЙ', emoji: '🥫' },
  { id:'bandage',   label:'БИНТЫ',    icon:'bandage',  color:'#5c6d43', multiplier:1.8, type:'win',     weight:17, rarity:'common',  tag:'ОБЫЧНЫЙ', emoji: '🩹' },
  { id:'ammo',      label:'ПАТРОНЫ',  icon:'ammo',     color:'#9f4128', multiplier:2.0, type:'win',     weight:16, rarity:'common',  tag:'ОБЫЧНЫЙ', emoji: '🔫' },
  { id:'medkit',    label:'АПТЕЧКА',  icon:'medkit',   color:'#5c6d43', multiplier:3.0, type:'win',     weight:11, rarity:'rare',    tag:'РЕДКИЙ', emoji: '💊' },
  { id:'scrap',     label:'МЕТАЛЛ',   icon:'scrap',    color:'#cba14b', multiplier:1.2, type:'win',     weight:14, rarity:'common',  tag:'ОБЫЧНЫЙ', emoji: '🔧' },
  { id:'x2',        label:'x2',       icon:'x2',       color:'#cba14b', multiplier:2.0, type:'multi',   weight:5,  rarity:'rare',    tag:'РЕДКИЙ', emoji: '✖️' },
  { id:'shotgun',   label:'ОРУЖИЕ',   icon:'shotgun',  color:'#cba14b', multiplier:10.0,type:'jackpot', weight:2,  rarity:'legend',  tag:'ЛЕГЕНДАРНЫЙ', emoji: '💥' },
];

const canvas = document.getElementById('wheelCanvas');
const ctx = canvas.getContext('2d');
const CX = canvas.width / 2, CY = canvas.height / 2, R = canvas.width / 2 - 2;
let wheelAngle = 0, spinning = false;
let lastTickAngle = 0;

function totalWeight() { return SEGMENTS.reduce((a, s) => a + s.weight, 0); }

function drawIcon(iconName, isDark) {
  ctx.save();
  const color = isDark ? '#fff' : '#222';
  ctx.fillStyle = color; ctx.strokeStyle = color;
  ctx.lineWidth = 2.5; ctx.lineCap = 'round'; ctx.lineJoin = 'round';
  ctx.scale(1.2, 1.2); ctx.translate(-24, -24);

  if (iconName === 'canned') {
    ctx.beginPath(); ctx.rect(10, 14, 28, 24); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(10, 22); ctx.lineTo(38, 22); ctx.stroke();
    ctx.beginPath(); ctx.ellipse(24, 14, 14, 4, 0, 0, Math.PI*2); ctx.stroke();
  } else if (iconName === 'bandage') {
    ctx.beginPath(); ctx.roundRect(12, 12, 24, 24, 6); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(24, 16); ctx.lineTo(24, 32); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(16, 24); ctx.lineTo(32, 24); ctx.stroke();
  } else if (iconName === 'ammo') {
    ctx.beginPath(); ctx.moveTo(16, 36); ctx.lineTo(16, 20); ctx.quadraticCurveTo(16, 10, 24, 10); ctx.quadraticCurveTo(32, 10, 32, 20); ctx.lineTo(32, 36); ctx.closePath(); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(16, 36); ctx.lineTo(32, 36); ctx.stroke();
    ctx.beginPath(); ctx.arc(24, 24, 4, 0, Math.PI*2); ctx.fill();
  } else if (iconName === 'medkit') {
    ctx.beginPath(); ctx.rect(10, 14, 28, 22); ctx.stroke();
    ctx.beginPath(); ctx.rect(18, 10, 12, 6); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(24, 20); ctx.lineTo(24, 30); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(19, 25); ctx.lineTo(29, 25); ctx.stroke();
  } else if (iconName === 'scrap') {
    ctx.beginPath(); ctx.arc(24, 24, 10, 0, Math.PI*2); ctx.stroke();
    for(let i=0; i<6; i++) { ctx.save(); ctx.translate(24,24); ctx.rotate(i*Math.PI/3); ctx.beginPath(); ctx.moveTo(10, 0); ctx.lineTo(16, 0); ctx.stroke(); ctx.restore(); }
  } else if (iconName === 'box') {
    ctx.beginPath(); ctx.moveTo(12, 16); ctx.lineTo(24, 10); ctx.lineTo(36, 16); ctx.lineTo(36, 32); ctx.lineTo(24, 38); ctx.lineTo(12, 32); ctx.closePath(); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(24, 10); ctx.lineTo(24, 38); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(12, 16); ctx.lineTo(24, 22); ctx.lineTo(36, 16); ctx.stroke();
  } else if (iconName === 'x2') {
    ctx.font = 'bold 24px "Roboto Condensed", sans-serif'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle'; ctx.fillText('x2', 24, 24);
  } else if (iconName === 'shotgun') {
    ctx.beginPath(); ctx.moveTo(10, 20); ctx.lineTo(38, 20); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(14, 26); ctx.lineTo(30, 26); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(32, 20); ctx.lineTo(36, 28); ctx.stroke();
  }
  ctx.restore();
}

function drawWheel() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  const total = totalWeight();
  let startAngle = wheelAngle;

  SEGMENTS.forEach(seg => {
    const arc = (seg.weight / total) * Math.PI * 2;
    ctx.beginPath(); ctx.moveTo(CX, CY); ctx.arc(CX, CY, R, startAngle, startAngle + arc); ctx.closePath();
    const grad = ctx.createRadialGradient(CX, CY, R*0.2, CX, CY, R);
    grad.addColorStop(0, seg.color); grad.addColorStop(1, adjustColor(seg.color, -20));
    ctx.fillStyle = grad; ctx.fill();
    ctx.lineWidth = 4; ctx.strokeStyle = '#f4eeda'; ctx.stroke();

    ctx.save(); ctx.translate(CX, CY); ctx.rotate(startAngle + arc / 2);
    const isDark = ['#5c6d43', '#9f4128', '#cba14b'].includes(seg.color);
    
    ctx.save(); ctx.translate(R * 0.38, 0); drawIcon(seg.icon, isDark); ctx.restore();

    ctx.fillStyle = isDark ? '#fff' : '#222';
    ctx.shadowColor = isDark ? 'rgba(0,0,0,0.5)' : 'rgba(255,255,255,0.5)';
    ctx.shadowBlur = 4; ctx.font = 'bold 20px "Roboto Condensed", sans-serif';
    ctx.textAlign = 'right'; ctx.textBaseline = 'middle';
    ctx.fillText(seg.label, R - 15, 0);
    ctx.restore();
    startAngle += arc;
  });
}

function adjustColor(color, percent) {
  const num = parseInt(color.replace("#",""),16), amt = Math.round(2.55 * percent),
  R = (num >> 16) + amt, G = (num >> 8 & 0x00FF) + amt, B = (num & 0x0000FF) + amt;
  return "#" + (0x1000000 + (R<255?R<1?0:R:255)*0x10000 + (G<255?G<1?0:G:255)*0x100 + (B<255?B<1?0:B:255)).toString(16).slice(1);
}

function pickSegment() {
  const weighted = SEGMENTS.map(s => { let w = s.weight; if (s.value >= 50 && state.luckBonus > 0) w *= (1 + state.luckBonus); return w; });
  const total = weighted.reduce((a, b) => a + b, 0);
  let r = Math.random() * total;
  for (let i = 0; i < SEGMENTS.length; i++) { r -= weighted[i]; if (r <= 0) return i; }
  return SEGMENTS.length - 1;
}

function spin() {
  if (spinning) return;
  const bet = Math.floor(Number(document.getElementById('betAmount').value));
  
  if (!bet || bet < 1) { toast('Введи ставку', true); return; }
  if (bet > state.dukes) { toast('Мало дюков!', true); return; }

  AudioEngine.init();
  spinning = true;
  state.dukes -= bet;
  updateWallet();
  saveGame();
  
  const btn = document.getElementById('spinBtn');
  btn.disabled = true;
  const resultBox = document.getElementById('resultBox');
  resultBox.textContent = '...';
  resultBox.style.color = 'var(--text)';

  const winIndex = pickSegment();
  const total = totalWeight();
  let before = 0;
  for (let i = 0; i < winIndex; i++) before += SEGMENTS[i].weight;
  const arcCenter = ((before + SEGMENTS[winIndex].weight / 2) / total) * Math.PI * 2;
  
  const spinsCount = 5 + Math.floor(Math.random() * 3);
  const targetAngle = -Math.PI / 2 - arcCenter - spinsCount * Math.PI * 2;
  const startAngle = wheelAngle;
  const startTime = performance.now();
  const duration = 4000;

  function animate(now) {
    const t = Math.min((now - startTime) / duration, 1);
    const ease = 1 - Math.pow(1 - t, 4);
    wheelAngle = startAngle + (targetAngle - startAngle) * ease;
    
    const currentTick = Math.floor(wheelAngle / (Math.PI * 2 / SEGMENTS.length));
    if (currentTick !== lastTickAngle) { AudioEngine.tick(); lastTickAngle = currentTick; }

    drawWheel();
    if (t < 1) requestAnimationFrame(animate);
    else {
      wheelAngle = ((wheelAngle % (Math.PI * 2)) + Math.PI * 2) % (Math.PI * 2);
      finishSpin(winIndex, bet);
    }
  }
  requestAnimationFrame(animate);
}

function finishSpin(index, bet) {
  spinning = false;
  document.getElementById('spinBtn').disabled = false;
  const seg = SEGMENTS[index];
  const box = document.getElementById('resultBox');

  if (seg.multiplier === 0) {
    box.textContent = 'ПУСТО (x0)';
    box.style.color = 'var(--brick)';
    AudioEngine.lose();
  } else {
    const winAmount = Math.floor(bet * seg.multiplier);
    state.dukes += winAmount;
    box.textContent = `x${seg.multiplier} — +${winAmount}`;
    box.style.color = seg.multiplier >= 10 ? 'var(--mustard-dark)' : 'var(--olive)';
    
    if (seg.type === 'win' || seg.type === 'jackpot') {
      if (!state.inventory) state.inventory = {};
      state.inventory[seg.id] = (state.inventory[seg.id] || 0) + 1;
      renderInventory();
    }

    if (seg.multiplier >= 10) AudioEngine.jackpot();
    else AudioEngine.win();
    
    updateWallet(true);
  }
  saveGame();
}

document.getElementById('spinBtn').addEventListener('click', spin);
document.getElementById('allInBtn').addEventListener('click', () => {
  AudioEngine.click();
  const input = document.getElementById('betAmount');
  if (state.dukes < 10) input.value = Math.max(1, Math.floor(state.dukes));
  else input.value = Math.floor(state.dukes);
});

/* ================== РЕНДЕР ПРИЗОВ ================== */
function renderPrizes() {
  const grid = document.getElementById('prizesGrid'); grid.innerHTML = '';
  const total = totalWeight();
  SEGMENTS.forEach(seg => {
    const chance = Math.round((seg.weight / total) * 100);
    let bgClass = 'bg-transparent';
    if (seg.id === 'canned' || seg.id === 'shotgun') { bgClass = 'bg-mustard'; }
    else if (seg.id === 'bandage' || seg.id === 'medkit') { bgClass = 'bg-olive'; }
    else if (seg.id === 'ammo') { bgClass = 'bg-brick'; }
    else if (seg.id === 'empty_box' || seg.id === 'scrap') { bgClass = 'bg-sand'; }
    
    let tagClass = 'tag-common';
    if (seg.rarity === 'legend') tagClass = 'tag-legend';
    else if (seg.rarity === 'rare') tagClass = 'tag-rare';
    else if (seg.rarity === 'empty') tagClass = 'tag-empty';

    const div = document.createElement('div'); div.className = 'prize-card';
    div.innerHTML = `
      <div class="prize-chance">${chance}%</div>
      <div class="prize-icon ${bgClass}">${seg.emoji}</div> 
      <div class="prize-name">${seg.label}</div>
      <div class="prize-mult">Множитель: x${seg.multiplier}</div>
      <div class="prize-tag ${tagClass}">${seg.tag}</div>
    `;
    grid.appendChild(div);
  });
}

/* ================== ИНВЕНТАРЬ ================== */
function renderInventory() {
  const grid = document.getElementById('inventoryGrid');
  grid.innerHTML = '';
  
  if (!state.inventory || Object.keys(state.inventory).length === 0) {
    grid.innerHTML = '<div class="inv-empty">Рюкзак пуст. Крути колесо!</div>';
    return;
  }

  for (const [id, count] of Object.entries(state.inventory)) {
    if (count <= 0) continue;
    const seg = SEGMENTS.find(s => s.id === id);
    if (!seg) continue;
    
    const div = document.createElement('div');
    div.className = 'inv-item';
    div.title = seg.label;
    div.innerHTML = `
      <div class="inv-icon">${seg.emoji}</div>
      <div class="inv-count">x${count}</div>
    `;
    grid.appendChild(div);
  }
}

document.getElementById('sellAllBtn').addEventListener('click', () => {
  if (!state.inventory || Object.keys(state.inventory).length === 0) {
    toast('Рюкзак пуст!', true);
    return;
  }
  
  let totalValue = 0;
  let totalItems = 0;
  
  for (const [id, count] of Object.entries(state.inventory)) {
    const seg = SEGMENTS.find(s => s.id === id);
    if (seg && count > 0) {
      totalValue += Math.floor(seg.multiplier * 10) * count;
      totalItems += count;
    }
  }
  
  if (totalValue > 0) {
    state.dukes += totalValue;
    state.inventory = {};
    updateWallet(true);
    saveGame();
    renderInventory();
    AudioEngine.buy();
    toast(`Продано ${totalItems} предметов за ${totalValue} дюков!`);
  }
});

/* ================== МАГАЗИН ================== */
const SHOP_ITEMS = [
  { id:'luck_charm', icon:'🍀', name:'Талисман удачи', desc:'+5% к шансу редких призов', cost: { dukes: 200 } },
  { id:'flashlight', icon:'🔦', name:'Фонарик', desc:'+2 сек в игре с зомби', cost: { dukes: 120 } },
  { id:'armor', icon:'🛡️', name:'Броня', desc:'Защита от промахов', cost: { dukes: 300 } },
  { id:'crypto_key', icon:'🔑', name:'Ключ-крипта', desc:'+2 попытки во взломе', cost: { dukes: 250 } },
  { id:'radio', icon:'📻', name:'Радиостанция', desc:'+10% к награде за все мини-игры', cost: { dukes: 500, scrap: 3, ammo: 2 } },
  { id:'hazmat', icon:'🥼', name:'Костюм химзащиты', desc:'Иммунитет к штрафам в мини-играх', cost: { dukes: 400, medkit: 1, bandage: 3 } },
  { id:'multitool', icon:'🛠️', name:'Мультитул', desc:'x2 множитель к выигрышу в слотах', cost: { dukes: 350, scrap: 1, canned: 3 } },
  // Улучшения оружия (влияют на зомби-режим)
  { id:'silencer', icon:'🔇', name:'Глушитель', desc:'+1 очко за зомби, меньше отдача', cost: { dukes: 600, scrap: 2, bandage: 1 } },
  { id:'scope', icon:'🔭', name:'Оптический прицел', desc:'+2 очка за зомби', cost: { dukes: 800, scrap: 3, ammo: 2 } },
  { id:'extended_mag', icon:'📦', name:'Расширенный магазин', desc:'+15 патронов в обойме (всего 45)', cost: { dukes: 500, ammo: 3, canned: 2 } },
  // Улучшения для Шахты
  { id:'diamond_pick', icon:'⛏️', name:'Алмазная кирка', desc:'Снижает шанс взрыва в Шахте', cost: { dukes: 700, scrap: 5, medkit: 1 } }
];

function renderShop() {
  const grid = document.getElementById('shopGrid'); 
  if (!grid) return;
  grid.innerHTML = '';
  
  SHOP_ITEMS.forEach(item => {
    const owned = !!state.owned[item.id];
    
    let canAfford = true;
    let costHtml = '';
    let isCraft = false;
    
    if (item.cost.dukes !== undefined) {
      const hasDukes = state.dukes >= item.cost.dukes;
      if (!hasDukes) canAfford = false;
      costHtml += `<span style="color:${hasDukes ? 'var(--olive)' : 'var(--brick)'}">◈ ${item.cost.dukes}</span>`;
    }
    
    for (const [resId, reqAmount] of Object.entries(item.cost)) {
      if (resId === 'dukes') continue;
      isCraft = true;
      const seg = SEGMENTS.find(s => s.id === resId);
      if (!seg) continue;
      const hasAmount = (state.inventory[resId] || 0);
      const enough = hasAmount >= reqAmount;
      if (!enough) canAfford = false;
      costHtml += `<span style="color:${enough ? 'var(--olive)' : 'var(--brick)'}">${seg.emoji} ${hasAmount}/${reqAmount}</span>`;
    }

    const div = document.createElement('div'); div.className = 'shop-item';
    div.innerHTML = `
      <div class="icon">${item.icon}</div>
      <div class="info">
        <div class="name">${item.name}</div>
        <div class="desc">${item.desc}</div>
        <div class="cost-row">${costHtml}</div>
      </div>
      <button class="btn buy-btn ${isCraft ? 'craft' : ''}" 
              ${owned || !canAfford ? 'disabled' : ''} data-id="${item.id}">
        ${owned ? 'ГОТОВО' : (canAfford ? (isCraft ? 'СОЗДАТЬ' : 'КУПИТЬ') : 'НЕТ РЕСУРСОВ')}
      </button>
    `;
    
    if (!owned && canAfford) {
      div.querySelector('button').addEventListener('click', () => buyItem(item));
    }
    grid.appendChild(div);
  });
}

function buyItem(item) {
  if (item.cost.dukes !== undefined && state.dukes < item.cost.dukes) { toast('Недостаточно дюков!', true); return; }
  for (const [resId, reqAmount] of Object.entries(item.cost)) {
    if (resId === 'dukes') continue;
    if ((state.inventory[resId] || 0) < reqAmount) { toast('Недостаточно ресурсов!', true); return; }
  }

  if (item.cost.dukes !== undefined) state.dukes -= item.cost.dukes;
  for (const [resId, reqAmount] of Object.entries(item.cost)) {
    if (resId === 'dukes') continue;
    state.inventory[resId] -= reqAmount;
    if (state.inventory[resId] <= 0) delete state.inventory[resId];
  }
  
  state.owned[item.id] = true;
  if (item.id === 'luck_charm') state.luckBonus += 0.05;
  
  updateWallet(); saveGame(); renderShop(); renderInventory();
  AudioEngine.buy();
  toast(`Создано: ${item.name}`);
}

/* ================== ЗОМБИ (С ОБОЙМОЙ) ================== */
let zGame = null;
const zombieField = document.getElementById('zombieField');
const crosshair = document.getElementById('crosshair');
let currentAmmo = 30;
let maxAmmo = 30;
let isReloading = false;

// Обновление прицела
function updateCrosshair(e) {
  if (!crosshair) return;
  const rect = zombieField.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;
  crosshair.style.left = x + 'px';
  crosshair.style.top = y + 'px';
}

zombieField.addEventListener('mousemove', updateCrosshair);
zombieField.addEventListener('mouseleave', () => { if(crosshair) crosshair.style.display = 'none'; });
zombieField.addEventListener('mouseenter', () => { if(crosshair) crosshair.style.display = 'block'; });

// Перезарядка по кнопке R
document.addEventListener('keydown', (e) => {
  if (e.key.toLowerCase() === 'r' && zGame && zGame.active) {
    reloadZombieAmmo();
  }
});

function reloadZombieAmmo() {
  if (!zGame || !zGame.active || isReloading || currentAmmo === maxAmmo) return;
  isReloading = true;
  AudioEngine.reload();
  updateAmmoDisplay();
  
  setTimeout(() => {
    if (!zGame || !zGame.active) return;
    currentAmmo = maxAmmo;
    isReloading = false;
    updateAmmoDisplay();
  }, 1200); // 1.2 секунды на перезарядку
}

function updateAmmoDisplay() {
  const el = document.getElementById('ammoCounter');
  if (!el) return;
  el.textContent = isReloading ? 'ПЕРЕЗАРЯДКА...' : `${currentAmmo} / ${maxAmmo}`;
  el.className = 'ammo-counter';
  if (currentAmmo === 0 && !isReloading) el.classList.add('empty');
  else if (currentAmmo <= 5) el.classList.add('low');
}

function startZombieGame() {
  if (zGame) return;
  AudioEngine.click();
  
  let duration = 30 + (state.owned.flashlight ? 2 : 0);
  if (state.owned.extended_mag) duration += 2;
  
  maxAmmo = state.owned.extended_mag ? 45 : 30;
  currentAmmo = maxAmmo;
  isReloading = false;

  zGame = { active:true, hits:0, startTime:performance.now(), timerId:null, spawnId:null };

  document.getElementById('zHits').textContent = '0';
  document.getElementById('zTime').textContent = duration;
  document.getElementById('startZombie').disabled = true;
  zombieField.innerHTML = '<div class="crosshair" id="crosshair"><div class="circle"></div></div><div class="ammo-counter" id="ammoCounter"></div>';
  
  const newCrosshair = document.getElementById('crosshair');
  newCrosshair.style.display = 'block';
  newCrosshair.style.left = '50%';
  newCrosshair.style.top = '50%';
  updateAmmoDisplay();

  zGame.timerId = setInterval(() => {
    const left = Math.max(0, duration - (performance.now() - zGame.startTime) / 1000);
    document.getElementById('zTime').textContent = Math.ceil(left);
    if (left <= 0) endZombieGame();
  }, 100);

  zGame.spawnId = setInterval(spawnZombie, 650);

  // Обработка клика (стрельба)
  zombieField.onclick = (e) => {
    if (!zGame || !zGame.active) return;
    if (isReloading) { toast('Идёт перезарядка!', true); return; }
    if (currentAmmo <= 0) { toast('Нажми R для перезарядки!', true); AudioEngine.error(); return; }
    
    // Тратим патрон
    currentAmmo--;
    updateAmmoDisplay();
    
    // Отдача (сильнее без глушителя)
    const recoilAmount = state.owned.silencer ? 0.5 : 1;
    newCrosshair.style.transform = `translate(calc(-50% + ${-15 * recoilAmount}px), calc(-50% + ${-20 * recoilAmount}px)) scale(1.2)`;
    setTimeout(() => {
      newCrosshair.style.transform = 'translate(-50%, -50%)';
    }, 80);
    
    AudioEngine.shoot();
    
    if (e.target === zombieField || e.target.classList.contains('crosshair') || e.target.classList.contains('circle')) {
      if (!state.owned.armor && !state.owned.hazmat) {
        state.dukes = Math.max(0, state.dukes - 2);
        updateWallet(); AudioEngine.error(); toast('Промах! -2', true);
      }
    }
  };
}

function spawnZombie() {
  if (!zGame || !zGame.active) return;
  const rect = zombieField.getBoundingClientRect();
  const x = Math.random() * (rect.width - 50);
  const y = Math.random() * (rect.height - 50);

  const z = document.createElement('div');
  z.className = 'zombie'; z.textContent = '🧟';
  z.style.left = x + 'px'; z.style.top = y + 'px';

  z.onclick = (e) => {
    e.stopPropagation();
    if (!zGame || !zGame.active) return;
    if (isReloading) { toast('Идёт перезарядка!', true); return; }
    if (currentAmmo <= 0) { toast('Нажми R для перезарядки!', true); AudioEngine.error(); return; }
    
    currentAmmo--;
    updateAmmoDisplay();
    
    const newCrosshair = document.getElementById('crosshair');
    const recoilAmount = state.owned.silencer ? 0.5 : 1;
    newCrosshair.style.transform = `translate(calc(-50% + ${-15 * recoilAmount}px), calc(-50% + ${-20 * recoilAmount}px)) scale(1.2)`;
    setTimeout(() => {
      newCrosshair.style.transform = 'translate(-50%, -50%)';
    }, 80);
    
    AudioEngine.shoot();
    
    zGame.hits++;
    document.getElementById('zHits').textContent = zGame.hits;
    z.remove();
  };
  setTimeout(() => { if (z.parentNode) z.remove(); }, 1400);
  zombieField.appendChild(z);
}

function endZombieGame() {
  if (!zGame) return;
  zGame.active = false;
  clearInterval(zGame.timerId); clearInterval(zGame.spawnId);
  zombieField.innerHTML = '<div class="crosshair" id="crosshair"><div class="circle"></div></div>';
  
  let pointsPerKill = 3;
  if (state.owned.silencer) pointsPerKill += 1;
  if (state.owned.scope) pointsPerKill += 2;
  
  let reward = zGame.hits * pointsPerKill;
  if (state.owned.radio) reward = Math.floor(reward * 1.1);
  
  state.dukes += reward;
  updateWallet(); saveGame();
  AudioEngine.win();
  toast(`Бой окончен! +${reward} дюков`);
  document.getElementById('startZombie').disabled = false;
  zGame = null;
}
document.getElementById('startZombie').addEventListener('click', startZombieGame);

/* ================== СЛОТЫ ================== */
const SLOT_SYMBOLS = ['🥫', '🔫', '💊', '🔧', '⚡'];
let slotsSpinning = false;
let slotIntervals = [null, null, null];

function spinSlots() {
  if (slotsSpinning) return;
  const bet = Math.floor(Number(document.getElementById('slotBet').value));
  if (!bet || bet < 1) { toast('Введи ставку', true); return; }
  if (bet > state.dukes) { toast('Мало дюков!', true); return; }

  AudioEngine.init();
  AudioEngine.click();
  slotsSpinning = true;
  
  state.dukes -= bet;
  updateWallet(); saveGame();
  
  const btn = document.getElementById('spinSlotsBtn');
  btn.disabled = true;
  
  const resultBox = document.getElementById('slotResult');
  resultBox.classList.remove('pop');
  resultBox.textContent = 'КРУТИМ...';
  resultBox.style.color = 'var(--text-muted)';

  for (let i = 0; i < 3; i++) {
    document.getElementById('reel' + i).classList.remove('win', 'jackpot');
    document.getElementById('reel' + i).classList.add('spinning');
  }

  const finalSymbols = [
    SLOT_SYMBOLS[Math.floor(Math.random() * SLOT_SYMBOLS.length)],
    SLOT_SYMBOLS[Math.floor(Math.random() * SLOT_SYMBOLS.length)],
    SLOT_SYMBOLS[Math.floor(Math.random() * SLOT_SYMBOLS.length)]
  ];

  for (let i = 0; i < 3; i++) {
    let speed = 50 + i * 20;
    slotIntervals[i] = setInterval(() => {
      document.getElementById('reel' + i).textContent = SLOT_SYMBOLS[Math.floor(Math.random() * SLOT_SYMBOLS.length)];
      AudioEngine.tick();
    }, speed);
  }

  for (let i = 0; i < 3; i++) {
    setTimeout(() => {
      clearInterval(slotIntervals[i]);
      document.getElementById('reel' + i).classList.remove('spinning');
      document.getElementById('reel' + i).textContent = finalSymbols[i];
      AudioEngine.reelStop();

      if (i === 2) {
        setTimeout(() => finishSlots(finalSymbols, bet), 300);
      }
    }, 1000 + i * 500);
  }
}

function finishSlots(symbols, bet) {
  slotsSpinning = false;
  document.getElementById('spinSlotsBtn').disabled = false;
  
  const resultBox = document.getElementById('slotResult');
  let winAmount = 0;
  let message = '';
  let isJackpot = false;

  let mult = 1;
  if (state.owned.multitool) mult = 2;

  if (symbols[0] === symbols[1] && symbols[1] === symbols[2]) {
    if (symbols[0] === '⚡') {
      winAmount = bet * 10 * mult;
      message = `🔥 ДЖЕКПОТ x${10*mult}! 🔥`;
      isJackpot = true;
      AudioEngine.jackpot();
    } else {
      winAmount = bet * 3 * mult;
      message = `3 В РЯД! +${winAmount}`;
      AudioEngine.win();
    }
    for (let i = 0; i < 3; i++) {
      document.getElementById('reel' + i).classList.add(isJackpot ? 'jackpot' : 'win');
    }
  } else if (symbols[0] === symbols[1] || symbols[1] === symbols[2] || symbols[0] === symbols[2]) {
    winAmount = Math.floor(bet * 1.5 * mult);
    message = `2 СОВПАДЕНИЯ! +${winAmount}`;
    AudioEngine.win();
    if (symbols[0] === symbols[1]) { document.getElementById('reel0').classList.add('win'); document.getElementById('reel1').classList.add('win'); }
    if (symbols[1] === symbols[2]) { document.getElementById('reel1').classList.add('win'); document.getElementById('reel2').classList.add('win'); }
    if (symbols[0] === symbols[2]) { document.getElementById('reel0').classList.add('win'); document.getElementById('reel2').classList.add('win'); }
  } else {
    message = 'ПОВЕЗЁТ В СЛЕДУЮЩИЙ РАЗ';
    AudioEngine.lose();
  }

  if (winAmount > 0) {
    state.dukes += winAmount;
    updateWallet(true);
    saveGame();
    resultBox.style.color = isJackpot ? 'var(--mustard)' : 'var(--olive)';
  } else {
    resultBox.style.color = 'var(--brick)';
  }
  
  resultBox.textContent = message;
  resultBox.classList.add('pop');
}

document.getElementById('spinSlotsBtn').addEventListener('click', spinSlots);

/* ================== РАННЕР (РАЗВЕДКА) ================== */
const runnerField = document.getElementById('runnerField');
const runnerPlayer = document.getElementById('runnerPlayer');
const runnerScoreEl = document.getElementById('runnerScore');
let runnerGame = null;

function startRunnerGame() {
  if (runnerGame) return;
  AudioEngine.click();
  
  runnerGame = {
    active: true, score: 0, speed: 6, playerY: 0, velocityY: 0,
    isJumping: false, obstacles: [], lastSpawn: performance.now(),
    startTime: performance.now(), animId: null, jumpHold: false
  };
  
  runnerPlayer.style.bottom = '40px';
  runnerPlayer.style.transform = 'translateY(0px) scaleX(-1)';
  runnerPlayer.classList.add('running');
  runnerScoreEl.textContent = '0';
  runnerField.querySelectorAll('.runner-obstacle').forEach(el => el.remove());
  
  document.getElementById('startRunner').disabled = true;
  
  function gameLoop(now) {
    if (!runnerGame || !runnerGame.active) return;
    const delta = Math.min(2, (now - runnerGame.startTime) / 16);
    runnerGame.startTime = now;

    if (runnerGame.isJumping || runnerGame.playerY > 0) {
      runnerPlayer.classList.remove('running');
      const gravity = runnerGame.jumpHold ? 0.4 : 0.6;
      runnerGame.velocityY -= gravity; 
      runnerGame.playerY += runnerGame.velocityY;
      if (runnerGame.playerY <= 0) {
        runnerGame.playerY = 0; runnerGame.velocityY = 0; runnerGame.isJumping = false; runnerGame.jumpHold = false;
        runnerPlayer.classList.add('running');
      }
    } else {
      runnerPlayer.classList.add('running');
    }
    
    runnerPlayer.style.transform = `translateY(${-runnerGame.playerY}px) scaleX(-1)`;

    const fieldRect = runnerField.getBoundingClientRect();
    const spawnRate = Math.max(800, 1500 - runnerGame.score * 10);
    
    if (now - runnerGame.lastSpawn > spawnRate) {
      const obs = document.createElement('div');
      obs.className = 'runner-obstacle';
      const isTall = Math.random() > 0.7;
      obs.textContent = isTall ? '🗄️' : (Math.random() > 0.5 ? '📦' : '🪨');
      obs.style.bottom = isTall ? '60px' : '40px';
      obs.style.left = fieldRect.width + 'px';
      runnerField.appendChild(obs);
      runnerGame.obstacles.push({ el: obs, x: fieldRect.width, isTall: isTall });
      runnerGame.lastSpawn = now;
    }
    
    runnerGame.obstacles = runnerGame.obstacles.filter(obs => {
      obs.x -= runnerGame.speed * delta;
      obs.el.style.left = obs.x + 'px';
      const playerX = 50, playerWidth = 30, obsWidth = 30, obsHitboxTop = obs.isTall ? 60 : 40;
      const collision = (obs.x < playerX + playerWidth && obs.x + obsWidth > playerX && runnerGame.playerY < (obsHitboxTop - 40 + 20));
      
      if (collision) { endRunnerGame(); return false; }
      
      if (obs.x < -50) {
        obs.el.remove(); runnerGame.score += 10; runnerScoreEl.textContent = runnerGame.score; runnerGame.speed += 0.05;
        return false;
      }
      return true;
    });
    
    runnerGame.animId = requestAnimationFrame(gameLoop);
  }
  runnerGame.animId = requestAnimationFrame(gameLoop);
}

function jumpRunner() {
  if (!runnerGame || !runnerGame.active || runnerGame.isJumping) return;
  AudioEngine.jump();
  runnerGame.velocityY = 11; runnerGame.isJumping = true; runnerGame.jumpHold = true;
}

function releaseJump() { if (runnerGame) runnerGame.jumpHold = false; }

function endRunnerGame() {
  if (!runnerGame) return;
  runnerGame.active = false;
  cancelAnimationFrame(runnerGame.animId);
  runnerPlayer.classList.remove('running');
  AudioEngine.lose();
  
  let reward = Math.floor(runnerGame.score / 10);
  if (state.owned.radio) reward = Math.floor(reward * 1.1);
  
  state.dukes += reward; updateWallet(true); saveGame();
  toast(`Забег окончен! Счет: ${runnerGame.score}. +${reward} дюков`);
  document.getElementById('startRunner').disabled = false;
  runnerGame = null;
}

document.getElementById('startRunner').addEventListener('click', startRunnerGame);
runnerField.addEventListener('click', jumpRunner);
runnerField.addEventListener('touchstart', (e) => { e.preventDefault(); jumpRunner(); });
document.addEventListener('keydown', (e) => {
  if (e.code === 'Space') {
    if (document.getElementById('game-runner').classList.contains('active')) { e.preventDefault(); jumpRunner(); }
  }
});
document.addEventListener('keyup', (e) => { if (e.code === 'Space') releaseJump(); });

/* ================== АВИАТОР (ПОЛЁТ) ================== */
const aviatorField = document.getElementById('aviatorField');
const aviatorMultiplierEl = document.getElementById('aviatorMultiplier');
let aviatorGame = null;
let aviatorAnimId = null;

function startAviator() {
  if (aviatorGame) return;
  const bet = Math.floor(Number(document.getElementById('aviatorBet').value));
  if (!bet || bet < 1) { toast('Введи ставку', true); return; }
  if (bet > state.dukes) { toast('Мало дюков!', true); return; }

  AudioEngine.init();
  AudioEngine.click();
  
  state.dukes -= bet;
  updateWallet(); saveGame();
  
  document.getElementById('startAviator').style.display = 'none';
  document.getElementById('cashoutAviator').style.display = 'block';
  aviatorMultiplierEl.classList.add('active');
  aviatorField.innerHTML = '<div class="aviator-water"></div><div class="aviator-multiplier active" id="aviatorMultiplier">x1.00</div>';
  
  const plane = document.createElement('div');
  plane.className = 'aviator-plane';
  plane.textContent = '✈️';
  plane.style.left = '20px';
  plane.style.bottom = '40px';
  aviatorField.appendChild(plane);

  aviatorGame = {
    active: true,
    bet: bet,
    multiplier: 1.00,
    startTime: performance.now(),
    planeEl: plane,
    crashed: false,
    animId: null
  };

  AudioEngine.flyUp();

  function gameLoop(now) {
    if (!aviatorGame || !aviatorGame.active) return;

    const elapsed = (now - aviatorGame.startTime) / 1000;
    // Более мягкий рост в начале
    aviatorGame.multiplier = 1.00 + (Math.pow(elapsed, 1.2) * 0.15);
    
    const multEl = document.getElementById('aviatorMultiplier');
    if (multEl) multEl.textContent = 'x' + aviatorGame.multiplier.toFixed(2);

    const x = 20 + elapsed * 30;
    const y = 40 + Math.sin(elapsed * 2) * 5 + elapsed * 3;
    
    aviatorGame.planeEl.style.left = Math.min(x, aviatorField.clientWidth - 60) + 'px';
    aviatorGame.planeEl.style.bottom = Math.min(y, aviatorField.clientHeight - 60) + 'px';
    
    // Уменьшенный шанс краша (0.002 вместо 0.005)
    if (Math.random() < (0.002 * aviatorGame.multiplier)) {
      crashAviator();
      return;
    }

    aviatorGame.animId = requestAnimationFrame(gameLoop);
  }
  aviatorGame.animId = requestAnimationFrame(gameLoop);
}

function crashAviator() {
  if (!aviatorGame || !aviatorGame.active) return;
  aviatorGame.active = false;
  cancelAnimationFrame(aviatorGame.animId);
  AudioEngine.explode();
  
  const explosion = document.createElement('div');
  explosion.className = 'explosion';
  explosion.textContent = '💥';
  explosion.style.left = aviatorGame.planeEl.style.left;
  explosion.style.bottom = aviatorGame.planeEl.style.bottom;
  aviatorField.appendChild(explosion);
  
  aviatorGame.planeEl.style.display = 'none';
  
  setTimeout(() => {
    aviatorField.innerHTML = '<div class="aviator-water"></div><div class="aviator-multiplier" id="aviatorMultiplier">x1.00</div>';
    document.getElementById('startAviator').style.display = 'block';
    document.getElementById('cashoutAviator').style.display = 'none';
    aviatorGame = null;
  }, 1500);
}

function cashoutAviator() {
  if (!aviatorGame || !aviatorGame.active) return;
  aviatorGame.active = false;
  cancelAnimationFrame(aviatorGame.animId);
  
  AudioEngine.cashout();
  
  const winAmount = Math.floor(aviatorGame.bet * aviatorGame.multiplier);
  state.dukes += winAmount;
  updateWallet(true);
  saveGame();
  
  toast(`Успешная посадка! x${aviatorGame.multiplier.toFixed(2)} — +${winAmount} дюков`);
  
  aviatorField.innerHTML = '<div class="aviator-water"></div><div class="aviator-multiplier" id="aviatorMultiplier">x1.00</div>';
  document.getElementById('startAviator').style.display = 'block';
  document.getElementById('cashoutAviator').style.display = 'none';
  aviatorGame = null;
}

document.getElementById('startAviator').addEventListener('click', startAviator);
document.getElementById('cashoutAviator').addEventListener('click', cashoutAviator);

/* ================== ШАХТА (МАЙНКРАФТ) ================== */
const mineField = document.getElementById('mineField');
const mineGrid = document.getElementById('mineGrid');
const mineMultiplierEl = document.getElementById('mineMultiplier');
const mineLogEl = document.getElementById('mineLog');

let mineGame = null;
const GRID_SIZE = 5; // 5x5 сетка (25 ячеек)

function initMineGrid() {
  mineGrid.innerHTML = '';
  for (let i = 0; i < GRID_SIZE * GRID_SIZE; i++) {
    const cell = document.createElement('div');
    cell.className = 'mine-cell';
    cell.dataset.index = i;
    cell.textContent = '?';
    mineGrid.appendChild(cell);
  }
}

function startMine() {
  if (mineGame) return;
  const bet = Math.floor(Number(document.getElementById('mineBet').value));
  if (!bet || bet < 1) { toast('Введи ставку', true); return; }
  if (bet > state.dukes) { toast('Мало дюков!', true); return; }

  AudioEngine.init();
  AudioEngine.click();
  
  state.dukes -= bet;
  updateWallet(); saveGame();
  
  document.getElementById('startMine').style.display = 'none';
  document.getElementById('cashoutMine').style.display = 'block';
  mineMultiplierEl.textContent = 'x1.00';
  mineLogEl.textContent = 'Выбери ячейку...';
  
  // Генерируем поле
  // 25 ячеек. Шанс динамита зависит от улучшения "Алмазная кирка"
  let dynamiteChance = 0.2; // 20% базово
  if (state.owned.diamond_pick) dynamiteChance = 0.1; // 10% с киркой
  
  const totalCells = GRID_SIZE * GRID_SIZE;
  const dynamiteCount = Math.floor(totalCells * dynamiteChance);
  
  // Создаем массив индексов
  const indices = Array.from({ length: totalCells }, (_, i) => i);
  // Перемешиваем
  for (let i = indices.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [indices[i], indices[j]] = [indices[j], indices[i]];
  }
  
  const dynamiteIndices = indices.slice(0, dynamiteCount);
  
  mineGame = {
    active: true,
    bet: bet,
    multiplier: 1.00,
    dynamiteIndices: dynamiteIndices,
    revealedCount: 0,
    totalCells: totalCells,
    animId: null
  };
  
  // Очищаем старые обработчики
  const cells = mineGrid.querySelectorAll('.mine-cell');
  cells.forEach((cell, i) => {
    cell.className = 'mine-cell';
    cell.textContent = '?';
    cell.onclick = () => mineCellClick(i);
  });
}

function mineCellClick(index) {
  if (!mineGame || !mineGame.active) return;
  
  const cell = mineGrid.children[index];
  if (cell.classList.contains('revealed')) return; // Уже открыта
  
  AudioEngine.pickaxe();
  
  if (mineGame.dynamiteIndices.includes(index)) {
    // Попали на динамит
    cell.classList.add('dynamite');
    cell.textContent = '💣';
    mineGame.active = false;
    AudioEngine.explode();
    mineLogEl.textContent = '💥 БУМ! Ты подорвался. Ставка потеряна.';
    
    // Открываем все динамиты
    mineGame.dynamiteIndices.forEach(idx => {
      const c = mineGrid.children[idx];
      c.classList.add('dynamite');
      c.textContent = '💣';
    });
    
    setTimeout(() => {
      document.getElementById('startMine').style.display = 'block';
      document.getElementById('cashoutMine').style.display = 'none';
      mineGame = null;
    }, 2000);
  } else {
    // Нашли сундук/руду
    cell.classList.add('revealed', 'treasure');
    cell.textContent = '💎'; // Или можно менять на '💰'
    mineGame.revealedCount++;
    
    // Множитель растёт
    // Чем больше открыто, тем больше множитель, но и риск
    mineGame.multiplier = 1.00 + (mineGame.revealedCount * 0.25);
    mineMultiplierEl.textContent = 'x' + mineGame.multiplier.toFixed(2);
    
    AudioEngine.win();
    mineLogEl.textContent = `Найдено золото! Множитель: x${mineGame.multiplier.toFixed(2)}`;
    
    // Если открыты все безопасные ячейки — авто-кассаут
    const safeCells = mineGame.totalCells - mineGame.dynamiteIndices.length;
    if (mineGame.revealedCount >= safeCells) {
      cashoutMine();
    }
  }
}

function cashoutMine() {
  if (!mineGame || !mineGame.active) return;
  mineGame.active = false;
  
  AudioEngine.cashout();
  
  const winAmount = Math.floor(mineGame.bet * mineGame.multiplier);
  state.dukes += winAmount;
  updateWallet(true); saveGame();
  
  toast(`Удача! x${mineGame.multiplier.toFixed(2)} — +${winAmount} дюков`);
  mineLogEl.textContent = `Ты забрал ${winAmount} дюков!`;
  
  // Открываем все оставшиеся ячейки для наглядности
  const cells = mineGrid.querySelectorAll('.mine-cell');
  cells.forEach((cell, i) => {
    if (!cell.classList.contains('revealed') && !cell.classList.contains('dynamite')) {
      if (mineGame.dynamiteIndices.includes(i)) {
        cell.classList.add('dynamite');
        cell.textContent = '💣';
      } else {
        cell.classList.add('treasure');
        cell.textContent = '💎';
      }
    }
  });
  
  setTimeout(() => {
    document.getElementById('startMine').style.display = 'block';
    document.getElementById('cashoutMine').style.display = 'none';
    mineGame = null;
  }, 2000);
}

document.getElementById('startMine').addEventListener('click', startMine);
document.getElementById('cashoutMine').addEventListener('click', cashoutMine);

/* ================== ВЗЛОМ ================== */
let hackState = null;

function initHack() {
  const code = [];
  while (code.length < 4) {
    const d = Math.floor(Math.random() * 10);
    if (!code.includes(d)) code.push(d);
  }
  const maxAttempts = 8 + (state.owned.crypto_key ? 2 : 0);
  hackState = { secret: code, guess: [], attempts: maxAttempts, maxAttempts };
  renderHack();
}

function renderHack() {
  const disp = hackState.guess.map(d=>d).join(' ') + ' ' + '_ '.repeat(4 - hackState.guess.length);
  document.getElementById('codeDisplay').textContent = disp.trim() || '_ _ _ _';
  document.getElementById('codeInfo').textContent = `ПОПЫТОК: ${hackState.attempts} / ${hackState.maxAttempts}`;
}

function pressDigit(d) {
  if (!hackState || hackState.guess.length >= 4) return;
  if (hackState.guess.includes(d)) { toast('Цифры не повторяются', true); return; }
  AudioEngine.click();
  hackState.guess.push(d);
  renderHack();
  if (hackState.guess.length === 4) setTimeout(checkGuess, 300);
}

function checkGuess() {
  const g = hackState.guess, s = hackState.secret;
  let bulls = 0, cows = 0;
  for (let i = 0; i < 4; i++) {
    if (g[i] === s[i]) bulls++;
    else if (s.includes(g[i])) cows++;
  }
  hackState.attempts--;

  if (bulls === 4) {
    let reward = 100 + hackState.attempts * 20;
    if (state.owned.radio) reward = Math.floor(reward * 1.1);
    state.dukes += reward; updateWallet(true); saveGame();
    AudioEngine.jackpot();
    toast(`Взлом! +${reward} дюков`);
    document.getElementById('codeInfo').textContent = 'ВЗЛОМАНО!';
    document.getElementById('codeDisplay').textContent = '✓ ✓ ✓ ✓';
    hackState.guess = []; return;
  }

  if (hackState.attempts <= 0) {
    AudioEngine.lose();
    toast(`Провал! Код: ${s.join('')}`, true);
    document.getElementById('codeInfo').textContent = 'ПРОВАЛ. КОД: ' + s.join('');
    document.getElementById('codeDisplay').textContent = '_ _ _ _';
    hackState.guess = []; return;
  }

  AudioEngine.error();
  toast(`🐂 ${bulls}  🐄 ${cows}`);
  hackState.guess = []; renderHack();
}

const keypad = document.getElementById('keypad');
for (let i = 1; i <= 9; i++) {
  const b = document.createElement('button'); b.className = 'btn dark'; b.textContent = i;
  b.onclick = () => pressDigit(i); keypad.appendChild(b);
}
const b0 = document.createElement('button'); b0.className = 'btn dark'; b0.textContent = '0';
b0.onclick = () => pressDigit(0); keypad.appendChild(b0);
const bDel = document.createElement('button'); bDel.className = 'btn'; bDel.textContent = '⌫';
bDel.onclick = () => { if (hackState) { AudioEngine.click(); hackState.guess.pop(); renderHack(); } }; keypad.appendChild(bDel);
document.getElementById('resetHack').addEventListener('click', () => { AudioEngine.click(); initHack(); });

/* ================== ФОНОВЫЕ ЭФФЕКТЫ ================== */
function spawnAshParticles() {
  const bg = document.getElementById('rouletteBg');
  const count = 15;
  for (let i = 0; i < count; i++) {
    const p = document.createElement('div');
    p.className = 'ash-particle';
    const size = Math.random() * 3 + 1;
    p.style.width = size + 'px'; p.style.height = size + 'px';
    p.style.left = Math.random() * 100 + '%';
    p.style.animationDuration = (Math.random() * 4 + 3) + 's';
    p.style.animationDelay = Math.random() * 5 + 's';
    p.style.opacity = Math.random() * 0.3 + 0.1;
    bg.appendChild(p);
  }
}

/* ================== СТАРТ ================== */
loadSave();
updateWallet();
renderShop();
renderPrizes();
renderInventory();
drawWheel();
initHack();
initMineGrid(); // Инициализация сетки для Шахты
spawnAshParticles();
</script>
</body>
</html>
