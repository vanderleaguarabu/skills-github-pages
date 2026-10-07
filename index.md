<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>O Culto dos Gatos - Full Idle UI</title>
  <style>
    :root {
      --bg-dark: #0a0812;
      --panel-dark: #140f24;
      --panel-border: #2e1d4a;
      --accent-purple: #9d4edd;
      --accent-pink: #ff007f;
      --gold-yellow: #ffb703;
      --btn-green-top: #4caf50;
      --btn-green-bot: #2e7d32;
      --btn-purple-top: #7b2cbf;
      --btn-purple-bot: #5a189a;
      --text-muted: #a092b8;
    }

    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
      background-color: var(--bg-dark);
      color: #fff;
      margin: 0;
      padding: 0;
      user-select: none;
      display: flex;
      flex-direction: column;
      height: 100vh;
      overflow: hidden;
    }

    /* HUD SUPERIOR */
    .top-hud {
      background: linear-gradient(180deg, #160c2b 0%, #0d081a 100%);
      border-bottom: 2px solid var(--panel-border);
      padding: 8px 12px;
      display: flex;
      flex-direction: column;
      gap: 8px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.85);
      z-index: 10;
    }

    .hud-resources-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 8px;
    }

    .hud-resource-card {
      background: #1a1030;
      border: 1px solid var(--panel-border);
      border-radius: 12px;
      padding: 6px 10px;
      display: flex;
      align-items: center;
      gap: 8px;
      flex: 1;
      justify-content: center;
      box-shadow: inset 0 0 6px rgba(0,0,0,0.5);
    }

    .hud-resource-svg { width: 22px; height: 22px; flex-shrink: 0; }
    .hud-resource-info { display: flex; flex-direction: column; align-items: flex-start; line-height: 1.1; }
    .hud-resource-val { font-weight: 900; font-size: 0.95em; }
    .hud-resource-sub { font-size: 0.65em; color: var(--text-muted); }

    .hud-rank-section {
      display: flex;
      align-items: center;
      gap: 10px;
      background: rgba(15, 10, 26, 0.6);
      padding: 4px 10px;
      border-radius: 14px;
      border: 1px solid #23163a;
    }

    .rank-badge {
      background: linear-gradient(180deg, var(--accent-pink), #99004d);
      border: 2px solid #fff;
      border-radius: 50%;
      width: 32px;
      height: 32px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 900;
      font-size: 0.9em;
      box-shadow: 0 0 8px var(--accent-pink);
      flex-shrink: 0;
    }

    .rank-progress-bg {
      flex-grow: 1;
      background: #06040a;
      border-radius: 10px;
      height: 14px;
      border: 1px solid var(--panel-border);
      position: relative;
      overflow: hidden;
    }

    .rank-progress-fill {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #7b2cbf, #ff007f);
      transition: width 0.3s ease;
    }

    .rank-btn {
      background: linear-gradient(180deg, #4caf50, #2e7d32);
      border: 1px solid #8bc34a;
      border-radius: 10px;
      color: #fff;
      font-size: 0.75em;
      font-weight: 900;
      padding: 5px 10px;
      cursor: pointer;
      text-transform: uppercase;
      box-shadow: 0 2px 4px rgba(0,0,0,0.5);
    }

    .rank-btn:disabled {
      background: #2a2a2a;
      border-color: #444;
      color: #666;
    }

    /* CONTEÚDO PRINCIPAL */
    .main-content {
      flex-grow: 1;
      overflow-y: auto;
      padding: 12px;
      padding-bottom: 85px;
    }

    .tab-page { display: none; }
    .tab-page.active { display: block; }

    /* MULTIPLICADORES */
    .buy-mode-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 14px;
      background: var(--panel-dark);
      padding: 8px 14px;
      border-radius: 14px;
      border: 1px solid var(--panel-border);
    }

    .buy-mode-options { display: flex; gap: 6px; }
    .buy-mode-btn {
      background: #1c1430;
      border: 1px solid var(--panel-border);
      color: var(--text-muted);
      font-weight: 900;
      font-size: 0.75em;
      padding: 6px 12px;
      border-radius: 10px;
      cursor: pointer;
    }

    .buy-mode-btn.active {
      background: linear-gradient(180deg, var(--accent-pink), #a10054);
      border-color: #fff;
      color: #fff;
      box-shadow: 0 0 10px rgba(255, 0, 127, 0.6);
    }

    /* CARDS DE GERADOR */
    .generator-card {
      background: var(--panel-dark);
      border: 2px solid var(--panel-border);
      border-radius: 18px;
      padding: 12px;
      margin-bottom: 14px;
      display: flex;
      align-items: center;
      gap: 12px;
      position: relative;
    }

    /* ITEM CORRIGIDO: AVATAR TOTALMENTE CIRCULAR E SEM BORDAS QUADRADAS */
    .card-avatar-wrap {
      position: relative;
      width: 70px;
      height: 70px;
      border-radius: 50%; /* Garante formato circular absoluto */
      flex-shrink: 0;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      background: transparent;
    }

    .avatar-ring-svg {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      transform: rotate(-90deg);
      pointer-events: none;
      border-radius: 50%;
    }

    .avatar-ring-bg {
      fill: none;
      stroke: #1b122e;
      stroke-width: 5;
    }

    .avatar-ring-fill {
      fill: none;
      stroke: url(#ringGrad);
      stroke-width: 5;
      stroke-linecap: round;
      stroke-dasharray: 201; /* 2 * PI * r (r=32 => ~201) */
      stroke-dashoffset: 201;
      transition: stroke-dashoffset 0.3s ease;
    }

    .card-avatar-img {
      width: 58px;
      height: 58px;
      border-radius: 50%;
      object-fit: cover;
      box-shadow: 0 0 6px rgba(0,0,0,0.8);
      transition: transform 0.2s ease, filter 0.2s ease;
    }

    /* Efeito de destaque/brilho circular ao atingir 100% de progresso */
    .card-avatar-wrap.level-ready {
      border-radius: 50%;
      animation: avatarGlowPulse 1s infinite alternate ease-in-out;
    }

    .card-avatar-wrap.level-ready .card-avatar-img {
      filter: brightness(1.2);
    }

    @keyframes avatarGlowPulse {
      0% {
        box-shadow: 0 0 8px 2px rgba(255, 0, 127, 0.6);
        transform: scale(1);
      }
      100% {
        box-shadow: 0 0 18px 6px rgba(255, 0, 127, 1);
        transform: scale(1.06);
      }
    }

    /* CORREÇÃO DO TUTORIAL: Destaque inteiramente CIRCULAR */
    .card-avatar-wrap.highlight-tutorial {
      border-radius: 50% !important;
      animation: pulseHighlightCircular 1.2s infinite ease-in-out !important;
      z-index: 5;
    }

    @keyframes pulseHighlightCircular {
      0% {
        box-shadow: 0 0 0px 0px rgba(255, 0, 127, 0.9);
        transform: scale(1);
      }
      50% {
        box-shadow: 0 0 18px 8px rgba(255, 0, 127, 1);
        transform: scale(1.08);
      }
      100% {
        box-shadow: 0 0 0px 0px rgba(255, 0, 127, 0.9);
        transform: scale(1);
      }
    }

    .btn-buy-unit.highlight-tutorial {
      animation: pulseHighlightBtn 1.2s infinite ease-in-out !important;
      z-index: 5;
    }

    @keyframes pulseHighlightBtn {
      0% { box-shadow: 0 0 0px 0px rgba(255, 0, 127, 0.9); transform: scale(1); }
      50% { box-shadow: 0 0 16px 6px rgba(255, 0, 127, 1); transform: scale(1.05); }
      100% { box-shadow: 0 0 0px 0px rgba(255, 0, 127, 0.9); transform: scale(1); }
    }

    .card-avatar-wrap.avatar-click-anim .card-avatar-img { animation: avatarPop 0.2s ease-out; }
    @keyframes avatarPop {
      0% { transform: scale(0.9); }
      50% { transform: scale(1.15); }
      100% { transform: scale(1); }
    }

    .card-center { flex-grow: 1; display: flex; flex-direction: column; gap: 4px; }

    .cycle-bar-bg {
      background: #08060d;
      border-radius: 8px;
      height: 22px;
      border: 1px solid var(--panel-border);
      position: relative;
      overflow: hidden;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 8px;
    }

    .cycle-bar-fill {
      position: absolute;
      top: 0; left: 0;
      height: 100%; width: 0%;
      background: linear-gradient(90deg, #7b2cbf, #ff007f);
      z-index: 1;
    }

    .cycle-bar-text {
      position: relative;
      z-index: 2;
      font-size: 0.75em;
      font-weight: 900;
      color: #fff;
    }

    .btn-buy-unit {
      background: linear-gradient(180deg, var(--btn-green-top), var(--btn-green-bot));
      border: 1.5px solid #70e000;
      border-radius: 12px;
      color: #fff;
      font-weight: 900;
      padding: 8px 10px;
      cursor: pointer;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-width: 100px;
      box-shadow: 0 4px 0 #1b5e20;
    }

    .btn-buy-unit:disabled {
      background: linear-gradient(180deg, #333, #1a1a1a);
      border-color: #444;
      box-shadow: 0 3px 0 #111;
      color: #666;
      cursor: not-allowed;
    }

    .btn-buy-label { font-size: 0.8em; text-transform: uppercase; }
    .btn-buy-cost { font-size: 0.7em; color: var(--gold-yellow); margin-top: 2px; }

    /* GALERIA & DIÁRIO */
    .gallery-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
    .art-card-item {
      background: var(--panel-dark);
      border: 2px solid var(--panel-border);
      border-radius: 14px;
      overflow: hidden;
      position: relative;
      cursor: pointer;
    }

    .art-img-wrap { position: relative; width: 100%; height: 140px; overflow: hidden; background: #080511; }
    .art-card-img { width: 100%; height: 100%; object-fit: cover; }
    .art-card-item.locked .art-card-img { filter: blur(8px) brightness(0.35); transform: scale(1.08); }

    .art-lock-overlay {
      position: absolute; top: 0; left: 0; width: 100%; height: 100%;
      display: flex; flex-direction: column; align-items: center; justify-content: center;
      background: rgba(10, 8, 18, 0.5); color: #fff;
    }

    .art-lock-icon-svg { width: 32px; height: 32px; margin-bottom: 4px; fill: #aaa; }
    .art-lock-req { font-size: 0.68em; font-weight: bold; color: var(--gold-yellow); text-align: center; padding: 0 4px; }

    @keyframes shakeLock {
      0% { transform: translateX(0); }
      20% { transform: translateX(-6px) rotate(-1deg); }
      40% { transform: translateX(6px) rotate(1deg); }
      60% { transform: translateX(-4px); }
      80% { transform: translateX(4px); }
      100% { transform: translateX(0); }
    }

    .art-card-item.shake { animation: shakeLock 0.4s ease-in-out; }

    .art-card-info { padding: 8px; text-align: center; background: #100b1d; }
    .art-card-title { font-size: 0.78em; font-weight: bold; }
    .art-card-status { font-size: 0.65em; margin-top: 2px; }

    .diary-card {
      background: var(--panel-dark);
      border: 1px solid var(--panel-border);
      border-radius: 12px;
      padding: 12px;
      margin-bottom: 10px;
    }

    .diary-card.locked { opacity: 0.55; filter: grayscale(0.8); }
    .diary-card-title { font-weight: bold; color: var(--accent-pink); font-size: 0.9em; margin-bottom: 4px; }
    .diary-card-body { font-size: 0.8em; color: #ddd; line-height: 1.35; }

    /* MODAIS */
    .art-modal-overlay, .helper-modal-overlay {
      position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
      background: rgba(6, 4, 12, 0.92); backdrop-filter: blur(8px);
      z-index: 1000; display: flex; justify-content: center; align-items: center;
      padding: 16px; opacity: 0; pointer-events: none; transition: opacity 0.3s ease;
    }

    .art-modal-overlay.active, .helper-modal-overlay.active {
      opacity: 1; pointer-events: auto;
    }

    .art-modal-container {
      background: linear-gradient(180deg, #1c1333 0%, #0d081a 100%);
      border: 2px solid var(--accent-pink); border-radius: 20px;
      max-width: 420px; width: 100%; overflow: hidden;
    }

    .art-modal-img { width: 100%; max-height: 55vh; object-fit: contain; background: #050308; }
    .art-modal-body { padding: 16px; display: flex; flex-direction: column; gap: 8px; }
    .art-modal-title { font-size: 1.1em; font-weight: 900; color: var(--accent-pink); }
    .art-modal-desc { font-size: 0.85em; color: #ddd; }
    .art-modal-close-btn {
      background: linear-gradient(180deg, var(--btn-purple-top), var(--btn-purple-bot));
      border: 1px solid var(--accent-pink); border-radius: 12px;
      color: #fff; font-weight: 900; padding: 10px; cursor: pointer; text-align: center;
    }

    .helper-dialog-container {
      width: 100%; max-width: 450px; display: flex; flex-direction: column;
      align-items: center; position: relative; max-height: 90vh; justify-content: flex-end;
    }

    .helper-modal-character {
      width: clamp(260px, 75vw, 380px); height: auto; max-height: 48vh;
      object-fit: contain; filter: drop-shadow(0px 0px 20px rgba(157, 78, 221, 0.85));
      margin-bottom: -35px; z-index: 2;
    }

    .helper-modal-box {
      background: linear-gradient(180deg, #1f1538 0%, #120b24 100%);
      border: 2px solid var(--accent-pink); border-radius: 20px;
      padding: 20px 18px 18px; width: 100%; z-index: 3;
    }

    .helper-title { color: var(--accent-pink); font-weight: 900; font-size: 0.9em; margin-bottom: 6px; text-transform: uppercase; }
    .helper-text { color: #fff; font-size: 0.85em; line-height: 1.4; margin-bottom: 14px; }
    .helper-btn-next {
      background: linear-gradient(180deg, var(--btn-purple-top), var(--btn-purple-bot));
      border: 1px solid var(--accent-pink); border-radius: 12px;
      color: #fff; font-weight: 900; font-size: 0.85em; padding: 10px 16px;
      width: 100%; cursor: pointer; text-align: center;
    }

    /* NAV INFERIOR COM DIVISÓRIAS */
    .bottom-nav {
      position: fixed; bottom: 0; left: 0; width: 100%; height: 72px;
      background: linear-gradient(180deg, #120d22 0%, #090612 100%);
      border-top: 2px solid var(--panel-border);
      display: flex; justify-content: space-around; align-items: center; z-index: 100;
      box-shadow: 0 -4px 20px rgba(0,0,0,0.8);
    }

    .nav-item {
      display: flex; flex-direction: column; align-items: center; justify-content: center;
      color: var(--text-muted); font-size: 0.78em; font-weight: 700; cursor: pointer; flex: 1; height: 100%;
      position: relative; transition: color 0.2s ease;
    }

    .nav-item:not(:last-child)::after {
      content: '';
      position: absolute;
      right: 0;
      top: 20%;
      height: 60%;
      width: 1px;
      background: linear-gradient(180deg, rgba(46, 29, 74, 0.1), rgba(157, 78, 221, 0.4), rgba(46, 29, 74, 0.1));
    }

    .nav-item.active { color: var(--accent-pink); }
    .nav-item.active .nav-icon-svg { fill: var(--accent-pink); filter: drop-shadow(0 0 6px rgba(255, 0, 127, 0.6)); }

    .nav-icon-wrap {
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
      margin-bottom: 3px;
    }

    .nav-icon-svg {
      width: 28px;
      height: 28px;
      fill: var(--text-muted);
      transition: fill 0.2s ease, transform 0.2s ease;
    }

    .nav-item:hover .nav-icon-svg { transform: translateY(-2px); }

    .nav-badge {
      position: absolute;
      top: -4px;
      right: -10px;
      background: linear-gradient(180deg, #ff007f, #b30059);
      color: #fff;
      font-size: 0.7em;
      font-weight: 900;
      min-width: 18px;
      height: 18px;
      border-radius: 9px;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 0 4px;
      border: 1.5px solid #ffffff;
      box-shadow: 0 0 8px rgba(255, 0, 127, 0.8);
      transform: scale(0);
      transition: transform 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    }

    .nav-badge.active { transform: scale(1); }

    /* ANIMAÇÃO DE NÚMEROS SUBINDO */
    .floating-text {
      position: fixed;
      pointer-events: none;
      font-weight: 900;
      font-size: 1.1em;
      color: #70e000;
      text-shadow: 0 0 6px rgba(0,0,0,0.9), 0 0 10px #70e000;
      z-index: 9999;
      animation: floatUpAndFade 0.8s ease-out forwards;
    }

    @keyframes floatUpAndFade {
      0% { opacity: 1; transform: translate(-50%, 0) scale(0.8); }
      50% { opacity: 1; transform: translate(-50%, -25px) scale(1.25); }
      100% { opacity: 0; transform: translate(-50%, -50px) scale(1); }
    }
  </style>
</head>
<body>

  <!-- DEFINIÇÃO DE GRADIENTES SVG COMPARTILHADOS -->
  <svg style="width:0;height:0;position:absolute;" aria-hidden="true">
    <defs>
      <linearGradient id="ringGrad" x1="0%" y1="0%" x2="100%" y2="100%">
        <stop offset="0%" stop-color="#9d4edd" />
        <stop offset="100%" stop-color="#ff007f" />
      </linearGradient>
    </defs>
  </svg>

  <!-- HUD SUPERIOR -->
  <div class="top-hud">
    <div class="hud-resources-row">
      <!-- SALMÃO SVG -->
      <div class="hud-resource-card">
        <svg class="hud-resource-svg" viewBox="0 0 24 24" fill="#ffb703"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm1 14.5h-2v-2h2v2zm0-4h-2V7h2v5.5z"/></svg>
        <div class="hud-resource-info">
          <span style="color: var(--gold-yellow);" class="hud-resource-val" id="favor-count">0</span>
          <span class="hud-resource-sub">Salmão</span>
        </div>
      </div>

      <!-- POR SEGUNDO SVG -->
      <div class="hud-resource-card">
        <svg class="hud-resource-svg" viewBox="0 0 24 24" fill="#70e000"><path d="M7 2v11h3v9l7-12h-4l4-8z"/></svg>
        <div class="hud-resource-info">
          <span style="color: #70e000;" class="hud-resource-val" id="fps-count">0</span>
          <span class="hud-resource-sub">/segundo</span>
        </div>
      </div>

      <!-- VELAS SVG -->
      <div class="hud-resource-card">
        <svg class="hud-resource-svg" viewBox="0 0 24 24" fill="#ff007f"><path d="M12 2c-1.1 0-2 .9-2 2 0 .74.4 1.38 1 1.72V11h2V5.72c.6-.34 1-.98 1-1.72 0-1.1-.9-2-2-2zm-3 11v9h6v-9H9z"/></svg>
        <div class="hud-resource-info">
          <span style="color: var(--accent-pink);" class="hud-resource-val" id="velas-count">0</span>
          <span class="hud-resource-sub">Velas</span>
        </div>
      </div>
    </div>

    <div class="hud-rank-section">
      <div class="rank-badge" id="rank-number">1</div>
      <div class="rank-progress-bg">
        <div id="rank-progress-fill" class="rank-progress-fill"></div>
      </div>
      <button id="btn-subir-posto" class="rank-btn" onclick="subirPosto(event)" disabled>Devoção</button>
    </div>
  </div>

  <!-- CONTEÚDO PRINCIPAL -->
  <div class="main-content">

    <!-- MANSÃO -->
    <div id="tab-mansao" class="tab-page active">
      <div class="buy-mode-bar">
        <span style="font-size: 0.75em; color: var(--text-muted); font-weight: bold;">Multiplicador:</span>
        <div class="buy-mode-options">
          <button id="mult-1" class="buy-mode-btn active" onclick="setMultiplierMode('1')">1x</button>
          <button id="mult-10pct" class="buy-mode-btn" onclick="setMultiplierMode('10%')">10%</button>
          <button id="mult-50pct" class="buy-mode-btn" onclick="setMultiplierMode('50%')">50%</button>
          <button id="mult-100pct" class="buy-mode-btn" onclick="setMultiplierMode('100%')">MAX</button>
        </div>
      </div>

      <!-- CARD 1: LUNNA -->
      <div class="generator-card" id="card-lunna">
        <div class="card-avatar-wrap" id="avatar-lunna-wrap" onclick="clickAvatarProgress('lunna', event)">
          <svg class="avatar-ring-svg" viewBox="0 0 70 70">
            <circle class="avatar-ring-bg" cx="35" cy="35" r="32" />
            <circle id="lunna-ring-fill" class="avatar-ring-fill" cx="35" cy="35" r="32" />
          </svg>
          <img src="img/Lunna.jpg" class="card-avatar-img" alt="Lunna">
        </div>

        <div class="card-center">
          <div style="font-weight: bold; font-size: 0.9em;">Lunna <span style="font-size: 0.8em; color: var(--accent-pink);">(Niv <span id="lunna-lvl">0</span>)</span></div>
          <div class="cycle-bar-bg">
            <div id="lunna-cycle-fill" class="cycle-bar-fill"></div>
            <span class="cycle-bar-text"><span id="lunna-fps">0</span> 🐟/s</span>
            <span class="cycle-bar-text">1.0s</span>
          </div>
        </div>

        <button id="btn-buy-lunna" class="btn-buy-unit" onclick="buyGenerator('lunna', event)">
          <span class="btn-buy-label" id="lbl-lunna">Comprar</span>
          <span class="btn-buy-cost" id="cost-lunna">10 🐟</span>
        </button>
      </div>

      <!-- CARD 2: RAVENNA -->
      <div class="generator-card" id="card-ravenna">
        <div class="card-avatar-wrap" id="avatar-ravenna-wrap" onclick="clickAvatarProgress('ravenna', event)">
          <svg class="avatar-ring-svg" viewBox="0 0 70 70">
            <circle class="avatar-ring-bg" cx="35" cy="35" r="32" />
            <circle id="ravenna-ring-fill" class="avatar-ring-fill" cx="35" cy="35" r="32" />
          </svg>
          <img src="img/Ravenna.jpg" class="card-avatar-img" alt="Ravenna">
        </div>

        <div class="card-center">
          <div style="font-weight: bold; font-size: 0.9em;">Ravenna <span style="font-size: 0.8em; color: var(--accent-pink);">(Niv <span id="ravenna-lvl">0</span>)</span></div>
          <div class="cycle-bar-bg">
            <div id="ravenna-cycle-fill" class="cycle-bar-fill"></div>
            <span class="cycle-bar-text"><span id="ravenna-fps">0</span> 🐟/s</span>
            <span class="cycle-bar-text">3.0s</span>
          </div>
        </div>

        <button id="btn-buy-ravenna" class="btn-buy-unit" onclick="buyGenerator('ravenna', event)">
          <span class="btn-buy-label" id="lbl-ravenna">Comprar</span>
          <span class="btn-buy-cost" id="cost-ravenna">100 🐟</span>
        </button>
      </div>
    </div>

    <!-- GALERIA -->
    <div id="tab-artes" class="tab-page">
      <h3 style="margin-top: 0; color: var(--accent-pink);">Rituais & Ilustrações</h3>
      <div class="gallery-grid" id="gallery-container"></div>
    </div>

    <!-- DIÁRIO -->
    <div id="tab-diario" class="tab-page">
      <h3 style="margin-top: 0; color: var(--accent-purple);">Crônicas da Mansão</h3>
      <div id="diarios-list"></div>
    </div>

    <!-- LOJA -->
    <div id="tab-loja" class="tab-page">
      <h3 style="margin-top: 0; color: var(--gold-yellow);">Ofertas do Culto</h3>
      <div style="background: var(--panel-dark); padding: 20px; border-radius: 12px; border: 1px solid var(--panel-border); text-align: center;">
        <p style="color: var(--text-muted); font-size: 0.9em;">Loja de Velas em breve!</p>
      </div>
    </div>

  </div>

  <!-- MODAL ARTE -->
  <div id="art-modal" class="art-modal-overlay" onclick="closeArtModal(event)">
    <div class="art-modal-container" onclick="event.stopPropagation()">
      <img id="art-modal-img" src="" class="art-modal-img" alt="Arte Ampliada">
      <div class="art-modal-body">
        <div id="art-modal-title" class="art-modal-title">Título</div>
        <div id="art-modal-desc" class="art-modal-desc">Descrição</div>
        <button class="art-modal-close-btn" onclick="closeArtModal()">Fechar</button>
      </div>
    </div>
  </div>

  <!-- MODAL TUTORIAL / ASSISTENTE LUNNA -->
  <div id="helper-modal" class="helper-modal-overlay active">
    <div class="helper-dialog-container">
      <img src="img/Lunna-removebg-preview.png" class="helper-modal-character" alt="Lunna Guia">
      <div class="helper-modal-box">
        <div class="helper-title" id="helper-title">Boas-vindas ao Culto!</div>
        <div class="helper-text" id="helper-text">
          Olá! Eu sou a Lunna. Clique no meu retrato para avançar meu anel de progresso e coletar Salmões! 🐟
        </div>
        <button class="helper-btn-next" id="helper-btn" onclick="proximoPassoModal()">Entendido!</button>
      </div>
    </div>
  </div>

  <!-- NAV INFERIOR -->
  <div class="bottom-nav">
    <div class="nav-item active" onclick="switchTab('mansao', this)">
      <div class="nav-icon-wrap">
        <svg class="nav-icon-svg" viewBox="0 0 24 24"><path d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z"/></svg>
      </div>
      <div>Mansão</div>
    </div>

    <div class="nav-item" onclick="switchTab('artes', this)">
      <div class="nav-icon-wrap">
        <svg class="nav-icon-svg" viewBox="0 0 24 24"><path d="M21 19V5c0-1.1-.9-2-2-2H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2zM8.5 13.5l2.5 3.01L14.5 12l4.5 6H5l3.5-4.5z"/></svg>
        <div class="nav-badge" id="badge-artes">0</div>
      </div>
      <div>Galeria</div>
    </div>

    <div class="nav-item" onclick="switchTab('diario', this)">
      <div class="nav-icon-wrap">
        <svg class="nav-icon-svg" viewBox="0 0 24 24"><path d="M18 2H6c-1.1 0-2 .9-2 2v16c0 1.1.9 2 2 2h12c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zM6 4h5v8l-2.5-1.5L6 12V4z"/></svg>
        <div class="nav-badge" id="badge-diario">0</div>
      </div>
      <div>Diário</div>
    </div>

    <div class="nav-item" onclick="switchTab('loja', this)">
      <div class="nav-icon-wrap">
        <svg class="nav-icon-svg" viewBox="0 0 24 24"><path d="M19 5h-2V3c0-.55-.45-1-1-1h-8c-.55 0-1 .45-1 1v2H5c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V7c0-1.1-.9-2-2-2zm-9-2h4v2h-4V3zm9 16H5V7h14v12z"/></svg>
      </div>
      <div>Loja</div>
    </div>
  </div>

  <script>
    const state = {
      favor: 0,
      velas: 0,
      rank: 1,
      xp: 0,
      xpNeeded: 50,
      buyMode: '1',
      tutorialStep: 0,

      unseenGalleryCount: 0,
      unseenDiarioCount: 0,
      unlockedGalleryIds: new Set(),
      unlockedDiarioIds: new Set(),

      generators: {
        lunna: { name: 'Lunna', count: 0, cost: 10, mult: 1.15, baseFps: 1, cycleTime: 1000, progress: 0, clickClicks: 0, reqClicks: 5 },
        ravenna: { name: 'Ravenna', count: 0, cost: 100, mult: 1.18, baseFps: 8, cycleTime: 3000, progress: 0, clickClicks: 0, reqClicks: 10 }
      },

      gallery: [
        {
          id: 'art-lunna',
          title: "O Ritual do Salmão",
          imgSrc: "img/Lunna.jpg",
          desc: "Lunna conduz o ritual sagrado de oferendas. Os felinos se reúnem para receber a bênção do Salmão Primordial.",
          reqType: 'lvl',
          reqVal: 1
        },
        {
          id: 'art-ravenna',
          title: "Invocação das Sombras",
          imgSrc: "img/Ravenna.jpg",
          desc: "Ravenna conjura a chama violeta do Culto, purificando a Mansão e acelerando a devoção dos seguidores.",
          reqType: 'lvl',
          reqVal: 10
        }
      ],

      diarioEntries: [
        { id: 'diary-1', reqLvl: 1, title: "Página I: A Chegada", text: "Encontramos a Mansão abandonada e acendemos as primeiras velas. Lunna assumiu o cuidado dos suprimentos de Salmão." },
        { id: 'diary-2', reqLvl: 5, title: "Página II: A Primeira Invocação", text: "As vibrações do culto começam a ressoar. Ravenna juntou-se a nós para liderar os rituais noturnos." }
      ]
    };

    function clickAvatarProgress(key, event) {
      const g = state.generators[key];
      state.favor += 1;
      state.xp += 1;

      const wrap = document.getElementById(`avatar-${key}-wrap`);
      if (wrap) {
        wrap.classList.remove('avatar-click-anim');
        void wrap.offsetWidth;
        wrap.classList.add('avatar-click-anim');
      }

      if (g.clickClicks >= g.reqClicks) {
        g.clickClicks = 0;
        g.count += 1;
        
        spawnFloatingText(`NÍVEL +1! ✨`, event, '#ff007f');

        checkNewUnlocks();
        updateUI();
        renderGallery();
        renderDiario();
        return;
      }

      g.clickClicks++;
      spawnFloatingText('+1 🐟', event, '#70e000');

      if (key === 'lunna' && state.tutorialStep === 0 && state.favor >= 10) {
        document.getElementById('avatar-lunna-wrap').classList.remove('highlight-tutorial');
        state.tutorialStep = 1;
        document.getElementById('btn-buy-lunna').classList.add('highlight-tutorial');
        showModal("Salmões Suficientes!", "Você já acumulou Salmões suficientes! Clique no botão verde 'Comprar' para automatizar a produção da Lunna.");
      }

      checkNewUnlocks();
      updateUI();
    }

    function spawnFloatingText(text, event, color = '#70e000') {
      const el = document.createElement('div');
      el.className = 'floating-text';
      el.innerText = text;
      el.style.color = color;

      let x = event ? event.clientX : null;
      let y = event ? event.clientY : null;

      if (!x || !y) {
        const rect = event && event.currentTarget ? event.currentTarget.getBoundingClientRect() : { left: window.innerWidth/2, top: window.innerHeight/2, width: 0, height: 0 };
        x = rect.left + rect.width / 2;
        y = rect.top + rect.height / 2;
      }

      const randomOffsetX = (Math.random() - 0.5) * 20;
      el.style.left = `${x + randomOffsetX}px`;
      el.style.top = `${y}px`;

      document.body.appendChild(el);

      setTimeout(() => {
        el.remove();
      }, 800);
    }

    function calculateMaxAffordable(generatorKey, availableResources) {
      const g = state.generators[generatorKey];
      let currentLvl = g.count;
      let totalCost = 0;
      let levelsToBuy = 0;

      while (true) {
        let nextCost = Math.floor(g.cost * Math.pow(g.mult, currentLvl + levelsToBuy));
        if (totalCost + nextCost <= availableResources) {
          totalCost += nextCost;
          levelsToBuy++;
        } else {
          break;
        }
      }

      if (levelsToBuy === 0) {
        totalCost = Math.floor(g.cost * Math.pow(g.mult, currentLvl));
        levelsToBuy = 1;
      }

      return { levels: levelsToBuy, cost: totalCost };
    }

    function setMultiplierMode(mode) {
      state.buyMode = mode;
      document.querySelectorAll('.buy-mode-btn').forEach(btn => btn.classList.remove('active'));
      
      if (mode === '1') document.getElementById('mult-1').classList.add('active');
      if (mode === '10%') document.getElementById('mult-10pct').classList.add('active');
      if (mode === '50%') document.getElementById('mult-50pct').classList.add('active');
      if (mode === '100%') document.getElementById('mult-100pct').classList.add('active');

      updateUI();
    }

    function buyGenerator(key, event) {
      const g = state.generators[key];

      let budget = state.favor;
      if (state.buyMode === '10%') budget = state.favor * 0.10;
      if (state.buyMode === '50%') budget = state.favor * 0.50;

      let calc = (state.buyMode === '1') 
        ? { levels: 1, cost: Math.floor(g.cost * Math.pow(g.mult, g.count)) }
        : calculateMaxAffordable(key, budget);

      if (state.favor >= calc.cost && calc.levels > 0) {
        state.favor -= calc.cost;
        g.count += calc.levels;
        state.xp += (key === 'lunna' ? 5 : 20) * calc.levels;

        if (event) {
          spawnFloatingText(`+${calc.levels} Niv`, event, '#ffb703');
        }

        if (key === 'lunna' && state.tutorialStep === 1) {
          document.getElementById('btn-buy-lunna').classList.remove('highlight-tutorial');
          state.tutorialStep = 2;
          showModal("Excelente Progresso!", "Agora que você contratou a Lunna, ela produzirá Salmões automaticamente a cada segundo!");
        }

        checkNewUnlocks();
        updateUI();
        renderGallery();
        renderDiario();
      }
    }

    function subirPosto(event) {
      if (state.xp >= state.xpNeeded) {
        state.xp -= state.xpNeeded;
        state.rank += 1;
        state.xpNeeded = Math.floor(state.xpNeeded * 1.8);
        state.velas += 5;

        if (event) {
          spawnFloatingText('+5 Velas! 🕯️', event, '#ff007f');
        }

        showModal("Altar Aumentado!", `Você subiu para o Nível de Devoção ${state.rank} e ganhou 5 Velas!`);
        checkNewUnlocks();
        updateUI();
        renderGallery();
        renderDiario();
      }
    }

    function checkNewUnlocks() {
      state.gallery.forEach(art => {
        if (isArtUnlocked(art) && !state.unlockedGalleryIds.has(art.id)) {
          state.unlockedGalleryIds.add(art.id);
          state.unseenGalleryCount++;
        }
      });

      const totalLvl = state.generators.lunna.count + state.generators.ravenna.count;
      state.diarioEntries.forEach(entry => {
        if (totalLvl >= entry.reqLvl && !state.unlockedDiarioIds.has(entry.id)) {
          state.unlockedDiarioIds.add(entry.id);
          state.unseenDiarioCount++;
        }
      });

      updateBadgesUI();
    }

    function updateBadgesUI() {
      const badgeArtes = document.getElementById('badge-artes');
      if (badgeArtes) {
        if (state.unseenGalleryCount > 0) {
          badgeArtes.innerText = state.unseenGalleryCount;
          badgeArtes.classList.add('active');
        } else {
          badgeArtes.classList.remove('active');
        }
      }

      const badgeDiario = document.getElementById('badge-diario');
      if (badgeDiario) {
        if (state.unseenDiarioCount > 0) {
          badgeDiario.innerText = state.unseenDiarioCount;
          badgeDiario.classList.add('active');
        } else {
          badgeDiario.classList.remove('active');
        }
      }
    }

    function isArtUnlocked(art) {
      const totalLvl = state.generators.lunna.count + state.generators.ravenna.count;
      if (art.reqType === 'lvl') return totalLvl >= art.reqVal;
      if (art.reqType === 'rank') return state.rank >= art.reqVal;
      return false;
    }

    function renderGallery() {
      const container = document.getElementById('gallery-container');
      if (!container) return;

      container.innerHTML = '';

      state.gallery.forEach(art => {
        const unlocked = isArtUnlocked(art);
        const card = document.createElement('div');
        card.className = `art-card-item ${unlocked ? 'unlocked' : 'locked'}`;
        card.onclick = () => openArtModal(art, card);

        const reqText = art.reqType === 'lvl' ? `Niv Coletivo ${art.reqVal}` : `Posto Devoção ${art.reqVal}`;

        card.innerHTML = `
          <div class="art-img-wrap">
            <img src="${art.imgSrc}" class="art-card-img" alt="${art.title}">
            ${!unlocked ? `
              <div class="art-lock-overlay">
                <svg class="art-lock-icon-svg" viewBox="0 0 24 24"><path d="M18 8h-1V6c0-2.76-2.24-5-5-5S7 3.24 7 6v2H6c-1.1 0-2 .9-2 2v10c0 1.1.9 2 2 2h12c1.1 0 2-.9 2-2V10c0-1.1-.9-2-2-2zm-6 9c-1.1 0-2-.9-2-2s.9-2 2-2 2 .9 2 2-.9 2-2 2zm3.1-9H8.9V6c0-1.71 1.39-3.1 3.1-3.1 1.71 0 3.1 1.39 3.1 3.1v2z"/></svg>
                <span class="art-lock-req">Requer ${reqText}</span>
              </div>
            ` : ''}
          </div>
          <div class="art-card-info">
            <div class="art-card-title">${art.title}</div>
            <div class="art-card-status" style="color: ${unlocked ? 'var(--accent-pink)' : 'var(--text-muted)'}">
              ${unlocked ? 'Desbloqueado ✨' : 'Bloqueado'}
            </div>
          </div>
        `;

        container.appendChild(card);
      });
    }

    function openArtModal(art, element) {
      const unlocked = isArtUnlocked(art);

      if (!unlocked) {
        if (element) {
          element.classList.remove('shake');
          void element.offsetWidth;
          element.classList.add('shake');
          
          setTimeout(() => {
            element.classList.remove('shake');
          }, 400);
        }
        return;
      }

      document.getElementById('art-modal-img').src = art.imgSrc;
      document.getElementById('art-modal-title').innerText = art.title;
      document.getElementById('art-modal-desc').innerText = art.desc;
      document.getElementById('art-modal').classList.add('active');
    }

    function closeArtModal(event) {
      if (!event || event.target.id === 'art-modal' || event.target.classList.contains('art-modal-close-btn')) {
        document.getElementById('art-modal').classList.remove('active');
      }
    }

    function showModal(title, text, btnText) {
      document.getElementById('helper-title').innerText = title;
      document.getElementById('helper-text').innerText = text;
      document.getElementById('helper-btn').innerText = btnText || "Continuar";
      document.getElementById('helper-modal').classList.add('active');
    }

    function closeModal() {
      document.getElementById('helper-modal').classList.remove('active');
    }

    function proximoPassoModal() {
      closeModal();
      if (state.tutorialStep === 0) {
        document.getElementById('avatar-lunna-wrap').classList.add('highlight-tutorial');
      }
    }

    function switchTab(tabId, element) {
      document.querySelectorAll('.tab-page').forEach(page => page.classList.remove('active'));
      document.querySelectorAll('.nav-item').forEach(nav => nav.classList.remove('active'));
      document.getElementById(`tab-${tabId}`).classList.add('active');
      element.classList.add('active');

      if (tabId === 'artes') {
        state.unseenGalleryCount = 0;
        updateBadgesUI();
      } else if (tabId === 'diario') {
        state.unseenDiarioCount = 0;
        updateBadgesUI();
      }
    }

    function renderDiario() {
      const container = document.getElementById('diarios-list');
      if (!container) return;

      const totalLvl = state.generators.lunna.count + state.generators.ravenna.count;
      container.innerHTML = '';

      state.diarioEntries.forEach(entry => {
        const isUnlocked = totalLvl >= entry.reqLvl;
        const card = document.createElement('div');
        card.className = `diary-card ${isUnlocked ? '' : 'locked'}`;

        card.innerHTML = `
          <div class="diary-card-title">${entry.title} ${isUnlocked ? '✨' : '🔒'}</div>
          <div class="diary-card-body">
            ${isUnlocked ? entry.text : `Requer Nível Coletivo ${entry.reqLvl} para desbloquear.`}
          </div>
        `;
        container.appendChild(card);
      });
    }

    // LOOP PRINCIPAL
    setInterval(() => {
      const l = state.generators.lunna;
      if (l.count > 0) {
        l.progress += 100;
        if (l.progress >= l.cycleTime) {
          l.progress = 0;
          state.favor += l.count * l.baseFps;
        }
        document.getElementById('lunna-cycle-fill').style.width = `${(l.progress / l.cycleTime) * 100}%`;
      }

      const r = state.generators.ravenna;
      if (r.count > 0) {
        r.progress += 100;
        if (r.progress >= r.cycleTime) {
          r.progress = 0;
          state.favor += r.count * r.baseFps;
        }
        document.getElementById('ravenna-cycle-fill').style.width = `${(r.progress / r.cycleTime) * 100}%`;
      }

      updateUI();
    }, 100);

    function updateUI() {
      document.getElementById('favor-count').innerText = Math.floor(state.favor);
      document.getElementById('velas-count').innerText = state.velas;
      
      const fps = (state.generators.lunna.count * state.generators.lunna.baseFps) + 
                  (state.generators.ravenna.count * (state.generators.ravenna.baseFps / 3));
      document.getElementById('fps-count').innerText = fps.toFixed(1);

      document.getElementById('rank-number').innerText = state.rank;
      const xpPct = Math.min(100, (state.xp / state.xpNeeded) * 100);
      document.getElementById('rank-progress-fill').style.width = `${xpPct}%`;
      document.getElementById('btn-subir-posto').disabled = state.xp < state.xpNeeded;

      Object.keys(state.generators).forEach(key => {
        const g = state.generators[key];
        document.getElementById(`${key}-lvl`).innerText = g.count;
        document.getElementById(`${key}-fps`).innerText = (key === 'lunna' ? g.count * g.baseFps : (g.count * g.baseFps).toFixed(1));

        const ringFill = document.getElementById(`${key}-ring-fill`);
        const wrap = document.getElementById(`avatar-${key}-wrap`);
        
        if (ringFill) {
          const circumference = 201; // 2 * PI * r
          const pct = Math.min(1, g.clickClicks / g.reqClicks);
          const offset = circumference - (pct * circumference);
          ringFill.style.strokeDashoffset = offset;

          if (pct >= 1) {
            wrap.classList.add('level-ready');
          } else {
            wrap.classList.remove('level-ready');
          }
        }

        let budget = state.favor;
        if (state.buyMode === '10%') budget = state.favor * 0.10;
        if (state.buyMode === '50%') budget = state.favor * 0.50;

        let calc = (state.buyMode === '1') 
          ? { levels: 1, cost: Math.floor(g.cost * Math.pow(g.mult, g.count)) }
          : calculateMaxAffordable(key, budget);

        const btn = document.getElementById(`btn-buy-${key}`);
        const lbl = document.getElementById(`lbl-${key}`);
        const costLbl = document.getElementById(`cost-${key}`);

        const canAfford = state.favor >= calc.cost && calc.levels > 0;

        lbl.innerText = `+${calc.levels} ${calc.levels === 1 ? 'Nível' : 'Níveis'}`;
        costLbl.innerText = `${calc.cost} 🐟`;

        btn.disabled = !canAfford;
      });
    }

    renderGallery();
    renderDiario();
    checkNewUnlocks();
    updateUI();
  </script>
</body>
</html>
