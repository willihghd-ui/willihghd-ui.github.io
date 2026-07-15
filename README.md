# willihghd-ui.github.io
LEGO VLL for MicroScout, designed for mobile devices

<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, viewport-fit=cover">
<title>VLL LINK — MicroScout Transmitter</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Chakra+Petch:wght@500;600;700&family=IBM+Plex+Mono:wght@400;500;600&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#121317;
    --panel:#1b1d24;
    --panel-alt:#20232c;
    --line:#2e313d;
    --yellow:#ffc53d;
    --red:#ff5d4a;
    --blue:#4ea1ff;
    --text:#f2f0e9;
    --text-dim:#8b8f9c;
    --led-off:#3a3d47;
    --led-on:#ffdd9e;
    --radius:14px;
  }
  *{box-sizing:border-box; -webkit-tap-highlight-color:transparent;}
  html,body{margin:0; padding:0; background:var(--bg); color:var(--text);
    font-family:'Inter',system-ui,sans-serif; overscroll-behavior:contain;}
  body{padding-bottom:env(safe-area-inset-bottom);}

  .app{max-width:480px; margin:0 auto; min-height:100vh; display:flex; flex-direction:column;}

  /* ---------- Header ---------- */
  .header{padding:22px 20px 14px; display:flex; align-items:center; gap:12px;
    border-bottom:1px solid var(--line);}
  .led-dot{width:14px; height:14px; border-radius:50%; background:var(--led-off);
    box-shadow:0 0 0 rgba(255,221,158,0); flex:none;
    transition:background-color .06s linear, box-shadow .06s linear;}
  .led-dot.on{background:var(--led-on); box-shadow:0 0 14px 4px rgba(255,221,158,.65);}
  .title-block{display:flex; flex-direction:column; line-height:1.1;}
  .title{font-family:'Chakra Petch',sans-serif; font-weight:700; font-size:22px;
    letter-spacing:.04em; margin:0;}
  .subtitle{font-size:12px; color:var(--text-dim); letter-spacing:.02em; margin-top:2px;}

  /* ---------- Camera / status bar ---------- */
  .camera-bar{margin:14px 20px 0; padding:12px 14px; background:var(--panel);
    border:1px solid var(--line); border-radius:var(--radius);
    display:flex; align-items:center; gap:10px; justify-content:space-between;}
  .status-pill{font-family:'IBM Plex Mono',monospace; font-size:11px; letter-spacing:.05em;
    padding:5px 9px; border-radius:20px; background:var(--panel-alt); color:var(--text-dim);
    text-transform:uppercase; white-space:nowrap;}
  .status-pill.ready{color:#7CE38B;}
  .status-pill.error{color:var(--red);}
  .btn-flash{font-family:'Chakra Petch',sans-serif; font-weight:600; font-size:13px;
    letter-spacing:.03em; background:var(--yellow); color:#1a1305; border:none;
    padding:10px 16px; border-radius:10px; cursor:pointer; white-space:nowrap;}
  .btn-flash:disabled{opacity:.45;}
  .camera-err{margin:8px 20px 0; font-size:12px; color:var(--red); line-height:1.4;}

  /* ---------- Tabs ---------- */
  .tabs{display:flex; margin:16px 20px 0; background:var(--panel); border:1px solid var(--line);
    border-radius:12px; padding:4px; gap:4px;}
  .tab-btn{flex:1; border:none; background:transparent; color:var(--text-dim);
    font-family:'Chakra Petch',sans-serif; font-weight:600; font-size:13px; letter-spacing:.03em;
    padding:9px 4px; border-radius:9px; cursor:pointer;}
  .tab-btn.active{background:var(--panel-alt); color:var(--text);}

  .panel{margin:16px 20px 24px; flex:1;}
  .panel[hidden]{display:none;}

  /* ---------- Comandos tab ---------- */
  .section-label{font-family:'Chakra Petch',sans-serif; font-size:12px; font-weight:600;
    letter-spacing:.12em; color:var(--text-dim); text-transform:uppercase; margin:18px 0 8px;}
  .section-label:first-child{margin-top:0;}
  .cmd-grid{display:grid; grid-template-columns:1fr 1fr; gap:8px;}
  .cmd-btn{font-family:'Inter',sans-serif; font-size:13.5px; font-weight:600; color:var(--text);
    background:var(--panel); border:1px solid var(--line); border-radius:10px;
    padding:12px 10px; text-align:left; cursor:pointer;}
  .cmd-btn .code{display:block; font-family:'IBM Plex Mono',monospace; font-size:11px;
    color:var(--text-dim); margin-top:3px;}
  .cmd-btn--direct{border-left:3px solid var(--red);}
  .cmd-btn--script{border-left:3px solid var(--blue);}
  .cmd-btn:active{background:var(--panel-alt);}
  .demo-btn{margin-top:18px; width:100%; font-family:'Chakra Petch',sans-serif; font-weight:600;
    font-size:14px; letter-spacing:.03em; background:var(--panel-alt); color:var(--yellow);
    border:1px dashed var(--yellow); border-radius:10px; padding:13px; cursor:pointer;}

  /* ---------- Manual tab ---------- */
  .manual-wrap{display:flex; flex-direction:column; align-items:center;}
  .digits{display:flex; gap:12px; margin-top:6px;}
  .digit-col{display:flex; flex-direction:column; align-items:center; gap:8px;}
  .digit-btn{width:56px; height:44px; border-radius:10px; border:1px solid var(--line);
    background:var(--panel); color:var(--yellow); font-size:20px; font-weight:700;
    cursor:pointer; touch-action:none; user-select:none;}
  .digit-btn:active{background:var(--panel-alt);}
  .digit-display{width:56px; height:64px; display:flex; align-items:center; justify-content:center;
    font-family:'IBM Plex Mono',monospace; font-size:34px; font-weight:600;
    background:var(--panel-alt); border-radius:10px; border:1px solid var(--line);}
  .digit-col.active .digit-display{border-color:var(--yellow); box-shadow:0 0 0 2px rgba(255,197,61,.25) inset;}
  .manual-result{margin-top:20px; font-family:'IBM Plex Mono',monospace; font-size:15px;
    color:var(--text-dim); text-align:center; min-height:20px;}
  .manual-result b{color:var(--text); font-weight:600;}
  .send-btn{margin-top:22px; width:100%; font-family:'Chakra Petch',sans-serif; font-weight:700;
    font-size:16px; letter-spacing:.04em; background:var(--yellow); color:#1a1305; border:none;
    border-radius:12px; padding:16px; cursor:pointer;}
  .send-btn:disabled{opacity:.4;}
  .manual-hint{margin-top:14px; font-size:11.5px; color:var(--text-dim); text-align:center; line-height:1.5;}

  /* ---------- Escaneo tab ---------- */
  .scan-desc{font-size:13px; color:var(--text-dim); line-height:1.55; margin-bottom:16px;}
  .scan-btn{width:100%; font-family:'Chakra Petch',sans-serif; font-weight:700; font-size:15px;
    letter-spacing:.03em; background:var(--yellow); color:#1a1305; border:none;
    border-radius:12px; padding:15px; cursor:pointer;}
  .scan-btn.stop{background:var(--red); color:#2a0e09;}
  .scan-progress{margin-top:20px; display:none;}
  .scan-progress.show{display:block;}
  .scan-current{font-family:'IBM Plex Mono',monospace; font-size:40px; font-weight:600;
    text-align:center; margin-bottom:4px;}
  .scan-label{font-size:12px; color:var(--text-dim); text-align:center; min-height:16px; margin-bottom:14px;}
  .scan-bar-track{height:8px; border-radius:6px; background:var(--panel-alt); overflow:hidden;}
  .scan-bar-fill{height:100%; width:0%; background:var(--yellow); border-radius:6px; transition:width .1s linear;}
  .scan-count{margin-top:8px; font-size:11.5px; color:var(--text-dim); text-align:center;}

  /* ---------- Info / advertencia ---------- */
  .info{margin:4px 20px 0;}
  .info summary{font-size:12px; color:var(--text-dim); cursor:pointer; list-style:none;
    display:flex; align-items:center; gap:6px;}
  .info summary::-webkit-details-marker{display:none;}
  .info summary::before{content:"i"; width:15px; height:15px; border-radius:50%; border:1px solid var(--text-dim);
    display:inline-flex; align-items:center; justify-content:center; font-size:10px; font-style:normal;}
  .info-body{font-size:12px; color:var(--text-dim); line-height:1.6; margin:10px 2px 0;}

  .footer-space{height:14px;}

  @media (prefers-reduced-motion: reduce){
    .led-dot{transition:none;}
  }
</style>
</head>
<body>
<div class="app">

  <div class="header">
    <div class="led-dot" id="ledDot"></div>
    <div class="title-block">
      <p class="title">VLL LINK</p>
      <p class="subtitle">Transmisor óptico para LEGO MicroScout</p>
    </div>
  </div>

  <div class="camera-bar">
    <span class="status-pill" id="statusPill">SIN FLASH</span>
    <button class="btn-flash" id="btnActivate">Activar flash</button>
  </div>
  <p class="camera-err" id="cameraErr" hidden></p>

  <details class="info">
    <summary>Sobre la precisión de esta app</summary>
    <p class="info-body">
      El protocolo VLL distingue un bit "0" de un bit "1" por una diferencia
      de solo 20ms. Esta app controla el flash a través de la cámara del
      teléfono, un camino con más latencia que el hardware dedicado (como el
      GPIO de un Flipper Zero). Puede funcionar perfecto o puede que la
      decodificación falle en tu equipo — es una limitación del hardware del
      teléfono, no un error de la app. Prueba primero el botón "DEMO" en la
      pestaña Comandos para verificar si tu MicroScout responde.
    </p>
  </details>

  <div class="tabs">
    <button class="tab-btn active" data-tab="cmd">Comandos</button>
    <button class="tab-btn" data-tab="manual">Manual</button>
    <button class="tab-btn" data-tab="scan">Escaneo</button>
  </div>

  <!-- ===================== TAB: COMANDOS ===================== -->
  <div class="panel" id="panel-cmd">
    <p class="section-label">Directos — se ejecutan al instante</p>
    <div class="cmd-grid" id="gridDirect"></div>

    <p class="section-label">Script — se encolan (máx. 15), ejecutar con RUN</p>
    <div class="cmd-grid" id="gridScript"></div>

    <button class="demo-btn" id="btnDemo">DEMO: delete + adelante 2s + beep + RUN</button>
  </div>

  <!-- ===================== TAB: MANUAL ===================== -->
  <div class="panel" id="panel-manual" hidden>
    <div class="manual-wrap">
      <div class="digits" id="digitsRow"></div>
      <p class="manual-result" id="manualResult">= 0 <b>(?)</b></p>
      <button class="send-btn" id="btnSendManual">ENVIAR CÓDIGO</button>
      <p class="manual-hint">Toca +/− para cambiar cada dígito (mantén presionado para avanzar rápido). El código se recorta a 127 si te pasas.</p>
    </div>
  </div>

  <!-- ===================== TAB: ESCANEO ===================== -->
  <div class="panel" id="panel-scan" hidden>
    <p class="scan-desc">
      Envía automáticamente los 128 códigos posibles (0-127), uno por uno,
      con una pequeña pausa entre cada uno. Dura entre 2.5 y 3 minutos —
      a diferencia de la versión para Flipper Zero, aquí sí puedes
      <b>detenerlo</b> en cualquier momento.
    </p>
    <button class="scan-btn" id="btnScan">INICIAR ESCANEO (0-127)</button>
    <div class="scan-progress" id="scanProgress">
      <p class="scan-current" id="scanCurrent">0</p>
      <p class="scan-label" id="scanLabel">&nbsp;</p>
      <div class="scan-bar-track"><div class="scan-bar-fill" id="scanBarFill"></div></div>
      <p class="scan-count" id="scanCount">0 / 128</p>
    </div>
  </div>

  <div class="footer-space"></div>
</div>

<script>
(function(){
  "use strict";

  // ===================================================================
  // Tabla de comandos VLL conocidos (misma que la version Flipper Zero)
  // ===================================================================
  const VLL_COMMANDS = [
    {code:0,  name:'Motor Adelante',    group:'D'},
    {code:1,  name:'Motor Atrás',       group:'D'},
    {code:2,  name:'Motor Stop',        group:'D'},
    {code:4,  name:'Beep 1',            group:'D'},
    {code:5,  name:'Beep 2',            group:'D'},
    {code:6,  name:'Beep 3',            group:'D'},
    {code:7,  name:'Beep 4',            group:'D'},
    {code:8,  name:'Beep 5',            group:'D'},
    {code:33, name:'RUN programa',      group:'D'},
    {code:34, name:'DELETE programa',   group:'D'},
    {code:16, name:'Adelante 0.5s',     group:'S'},
    {code:17, name:'Adelante 1.0s',     group:'S'},
    {code:18, name:'Adelante 2.0s',     group:'S'},
    {code:19, name:'Adelante 5.0s',     group:'S'},
    {code:20, name:'Atrás 0.5s',        group:'S'},
    {code:21, name:'Atrás 1.0s',        group:'S'},
    {code:22, name:'Atrás 2.0s',        group:'S'},
    {code:23, name:'Atrás 5.0s',        group:'S'},
    {code:24, name:'Beep 1',            group:'S'},
    {code:25, name:'Beep 2',            group:'S'},
    {code:26, name:'Beep 3',            group:'S'},
    {code:27, name:'Beep 4',            group:'S'},
    {code:28, name:'Beep 5',            group:'S'},
    {code:29, name:'Esperar luz',       group:'S'},
    {code:30, name:'Buscar luz',        group:'S'},
    {code:31, name:'Code',              group:'S'},
    {code:32, name:'Keep Alive',        group:'S'},
  ];

  function knownLabel(code){
    const found = VLL_COMMANDS.find(c => c.code === code);
    return found ? found.name : null;
  }

  // ===================================================================
  // Motor VLL: bit-banging del flash via Camera2/WebRTC "torch"
  // Mismos timings que la version C del Flipper Zero.
  // ===================================================================
  let track = null;
  let busy = false;

  const ledDot = document.getElementById('ledDot');

  function sleep(ms){ return new Promise(r => setTimeout(r, ms)); }

  async function torchWrite(on){
    ledDot.classList.toggle('on', on);
    if(!track) return;
    try{
      await track.applyConstraints({advanced:[{torch: on}]});
    }catch(e){
      // Si falla a mitad de transmision, no interrumpimos la secuencia:
      // solo queda registrado en consola.
      console.warn('torch error', e);
    }
  }

  async function vllPreamble(){ await torchWrite(true); await sleep(400); }
  async function vllStartBit(){ await torchWrite(false); await sleep(20); }
  async function vllBit0(){ await torchWrite(true); await sleep(40); await torchWrite(false); await sleep(20); }
  async function vllBit1(){ await torchWrite(true); await sleep(20); await torchWrite(false); await sleep(40); }
  async function vllStopBit(){
    await torchWrite(true);  await sleep(20);
    await torchWrite(false); await sleep(60);
    await torchWrite(true);  await sleep(120);
    await torchWrite(false);
  }

  function checksum(n){
    const s2 = n >> 2, s4 = n >> 4;
    return 7 - ((n + s2 + s4) & 7);
  }

  async function vllSendCode(code){
    code = code & 0x7F;
    const frame = (checksum(code) << 7) | code;
    await vllPreamble();
    await vllStartBit();
    for(let i = 9; i >= 0; i--){
      if(frame & (1 << i)) await vllBit1(); else await vllBit0();
    }
    await vllStopBit();
  }

  async function vllSendDemo(){
    await vllSendCode(34); await sleep(250); // DELETE
    await vllSendCode(18); await sleep(250); // [S] Adelante 2.0s
    await vllSendCode(24); await sleep(250); // [S] Beep 1
    await vllSendCode(33);                   // RUN
  }

  // ===================================================================
  // Camara / permiso de flash
  // ===================================================================
  const statusPill = document.getElementById('statusPill');
  const btnActivate = document.getElementById('btnActivate');
  const cameraErr = document.getElementById('cameraErr');

  async function activateFlash(){
    btnActivate.disabled = true;
    statusPill.textContent = 'Conectando…';
    statusPill.className = 'status-pill';
    cameraErr.hidden = true;
    try{
      const stream = await navigator.mediaDevices.getUserMedia({
        video: { facingMode: { ideal: 'environment' } }
      });
      const t = stream.getVideoTracks()[0];
      const caps = t.getCapabilities ? t.getCapabilities() : {};
      if(!caps.torch){
        stream.getTracks().forEach(tr => tr.stop());
        throw new Error('no-torch');
      }
      track = t;
      statusPill.textContent = 'Flash listo';
      statusPill.className = 'status-pill ready';
      btnActivate.textContent = 'Reconectar';
      btnActivate.disabled = false;
    }catch(e){
      statusPill.textContent = 'Sin flash';
      statusPill.className = 'status-pill error';
      btnActivate.disabled = false;
      cameraErr.hidden = false;
      if(e && e.message === 'no-torch'){
        cameraErr.textContent = 'La cámara respondió, pero este navegador/equipo no expone control de flash (torch). Prueba con Chrome actualizado en Android.';
      } else if(e && e.name === 'NotAllowedError'){
        cameraErr.textContent = 'Permiso de cámara denegado. Actívalo en los ajustes de Chrome para este sitio y vuelve a intentar.';
      } else {
        cameraErr.textContent = 'No se pudo acceder a la cámara (' + (e && e.message ? e.message : 'error desconocido') + '). Si abriste este archivo directo desde el almacenamiento, prueba a servirlo por HTTPS (por ejemplo GitHub Pages).';
      }
    }
  }
  btnActivate.addEventListener('click', activateFlash);

  // ===================================================================
  // Tabs
  // ===================================================================
  const tabButtons = document.querySelectorAll('.tab-btn');
  const panels = {
    cmd: document.getElementById('panel-cmd'),
    manual: document.getElementById('panel-manual'),
    scan: document.getElementById('panel-scan'),
  };
  tabButtons.forEach(btn => {
    btn.addEventListener('click', () => {
      tabButtons.forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      Object.keys(panels).forEach(k => panels[k].hidden = (k !== btn.dataset.tab));
    });
  });

  // ===================================================================
  // Tab: Comandos conocidos
  // ===================================================================
  function makeCmdButton(cmd){
    const b = document.createElement('button');
    b.className = 'cmd-btn ' + (cmd.group === 'D' ? 'cmd-btn--direct' : 'cmd-btn--script');
    b.innerHTML = cmd.name + '<span class="code">codigo ' + cmd.code + '</span>';
    b.addEventListener('click', async () => {
      if(busy) return;
      busy = true;
      await vllSendCode(cmd.code);
      busy = false;
    });
    return b;
  }
  const gridDirect = document.getElementById('gridDirect');
  const gridScript = document.getElementById('gridScript');
  VLL_COMMANDS.filter(c => c.group === 'D').forEach(c => gridDirect.appendChild(makeCmdButton(c)));
  VLL_COMMANDS.filter(c => c.group === 'S').forEach(c => gridScript.appendChild(makeCmdButton(c)));

  document.getElementById('btnDemo').addEventListener('click', async () => {
    if(busy) return;
    busy = true;
    await vllSendDemo();
    busy = false;
  });

  // ===================================================================
  // Tab: Manual (3 digitos)
  // ===================================================================
  const digits = [0, 0, 0];
  let activeDigit = 2;
  const digitsRow = document.getElementById('digitsRow');
  const manualResult = document.getElementById('manualResult');
  const digitDisplays = [];

  function manualCode(){
    const c = digits[0] * 100 + digits[1] * 10 + digits[2];
    return Math.min(127, c);
  }
  function refreshManualUI(){
    const c = manualCode();
    const label = knownLabel(c);
    manualResult.innerHTML = '= ' + c + ' <b>(' + (label ? label : '?') + ')</b>';
    digitDisplays.forEach((el, i) => { el.textContent = digits[i]; });
    document.querySelectorAll('.digit-col').forEach((col, i) => {
      col.classList.toggle('active', i === activeDigit);
    });
  }

  function bindRepeat(el, fn){
    let timeout = null, interval = null;
    function start(){ fn(); timeout = setTimeout(() => { interval = setInterval(fn, 90); }, 380); }
    function stop(){ clearTimeout(timeout); clearInterval(interval); timeout = null; interval = null; }
    el.addEventListener('pointerdown', e => { e.preventDefault(); start(); });
    ['pointerup', 'pointerleave', 'pointercancel'].forEach(ev => el.addEventListener(ev, stop));
  }

  for(let i = 0; i < 3; i++){
    const col = document.createElement('div');
    col.className = 'digit-col';

    const up = document.createElement('button');
    up.className = 'digit-btn'; up.textContent = '+';
    const display = document.createElement('div');
    display.className = 'digit-display'; display.textContent = '0';
    const down = document.createElement('button');
    down.className = 'digit-btn'; down.textContent = '−';

    col.addEventListener('click', () => { activeDigit = i; refreshManualUI(); });
    bindRepeat(up, () => { activeDigit = i; digits[i] = (digits[i] + 1) % 10; refreshManualUI(); });
    bindRepeat(down, () => { activeDigit = i; digits[i] = (digits[i] + 9) % 10; refreshManualUI(); });

    col.appendChild(up);
    col.appendChild(display);
    col.appendChild(down);
    digitsRow.appendChild(col);
    digitDisplays.push(display);
  }
  refreshManualUI();

  document.getElementById('btnSendManual').addEventListener('click', async () => {
    if(busy) return;
    busy = true;
    await vllSendCode(manualCode());
    busy = false;
  });

  // ===================================================================
  // Tab: Escaneo completo (interrumpible)
  // ===================================================================
  const btnScan = document.getElementById('btnScan');
  const scanProgress = document.getElementById('scanProgress');
  const scanCurrent = document.getElementById('scanCurrent');
  const scanLabel = document.getElementById('scanLabel');
  const scanBarFill = document.getElementById('scanBarFill');
  const scanCount = document.getElementById('scanCount');

  let scanning = false;
  let stopRequested = false;

  btnScan.addEventListener('click', async () => {
    if(scanning){
      stopRequested = true;
      return;
    }
    if(busy) return;
    busy = true;
    scanning = true;
    stopRequested = false;
    btnScan.textContent = 'DETENER ESCANEO';
    btnScan.classList.add('stop');
    scanProgress.classList.add('show');

    for(let c = 0; c <= 127; c++){
      if(stopRequested) break;
      scanCurrent.textContent = c;
      const label = knownLabel(c);
      scanLabel.textContent = label ? label : '(sin documentar)';
      scanBarFill.style.width = ((c / 127) * 100).toFixed(1) + '%';
      scanCount.textContent = c + ' / 128';
      await vllSendCode(c);
      await sleep(100);
    }

    scanning = false;
    busy = false;
    btnScan.textContent = 'INICIAR ESCANEO (0-127)';
    btnScan.classList.remove('stop');
    if(stopRequested){
      scanLabel.textContent = 'Detenido por el usuario';
    } else {
      scanCurrent.textContent = '127';
      scanLabel.textContent = 'Escaneo completo';
      scanBarFill.style.width = '100%';
    }
  });

})();
</script>
</body>
</html>
