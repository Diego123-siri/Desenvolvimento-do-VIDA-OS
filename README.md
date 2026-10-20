# Desenvolvimento-do-VIDA-OS
Este app está em desenvolvimeto e o intuito da criação desse app e pra ajudar as pessoa em suas rotinas diarias e em sua vida, mas não e um aplicativo qualquer a ideia e ele ir alem de um app.
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Vida OS — Núcleo de Voz</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
:root{--bg-primary:#05070D;--surface:#0D1422;--border:#17243A;--blue-primary:#1677FF;--blue-bright:#2EA7FF;--blue-neon:#00B7FF;--text-primary:#F5F9FF;--text-secondary:#8D9BB2;--text-disabled:#536174;--red:#FF5C6C}
*{box-sizing:border-box}
html,body{margin:0;background:#010204;font-family:'Inter',sans-serif;color:var(--text-primary);display:flex;justify-content:center;padding:40px 16px}
.app{width:420px;max-width:100%;background:var(--bg-primary);border-radius:28px;border:1px solid var(--border);padding:32px 28px 28px;position:relative;overflow:hidden;box-shadow:0 40px 100px rgba(0,20,60,.4)}
.app::before{content:'';position:absolute;inset:0;background-image:linear-gradient(rgba(22,119,255,.035) 1px,transparent 1px),linear-gradient(90deg,rgba(22,119,255,.035) 1px,transparent 1px);background-size:28px 28px;pointer-events:none}
.header{text-align:center;margin-bottom:4px;position:relative}
.eyebrow{font-family:'IBM Plex Mono',monospace;font-size:11px;letter-spacing:.08em;text-transform:uppercase;color:var(--text-disabled);margin:0}
.header h1{font-weight:300;font-size:24px;letter-spacing:-.01em;margin:4px 0 0}
.mono{font-family:'IBM Plex Mono',monospace}
.voice-badge{text-align:center;font-family:'IBM Plex Mono',monospace;font-size:10.5px;color:var(--text-disabled);margin-top:6px;position:relative}
.secure-warning{display:none;margin-top:10px;background:rgba(255,92,108,.08);border:1px solid rgba(255,92,108,.3);color:#FFB4BC;font-size:11.5px;padding:8px 12px;border-radius:10px;text-align:center;position:relative}
.secure-warning.show{display:block}
.orb-wrap{display:flex;flex-direction:column;align-items:center;margin:22px 0 8px;position:relative}
.orb{width:140px;height:140px;border-radius:50%;background:radial-gradient(circle at 35% 30%,#10203A 0%,#0A0F1A 70%);border:1.5px solid var(--border);position:relative;display:flex;align-items:center;justify-content:center;cursor:pointer;transition:border-color .3s,box-shadow .3s}
.orb-icon{font-size:12px;color:var(--text-disabled);font-family:'IBM Plex Mono',monospace;text-align:center;padding:0 18px;line-height:1.4}
.orb.idle{border-color:var(--blue-primary);box-shadow:0 0 20px rgba(22,119,255,.25),inset 0 0 20px rgba(22,119,255,.08)}
.orb.requesting{border-color:var(--blue-neon);animation:flicker 1s ease-in-out infinite}
@keyframes flicker{0%,100%{box-shadow:0 0 10px rgba(0,183,255,.2)}50%{box-shadow:0 0 24px rgba(0,183,255,.5)}}
.orb.listening{border-color:var(--blue-bright);animation:pulse-listen 1.8s ease-out infinite}
@keyframes pulse-listen{0%{box-shadow:0 0 0 0 rgba(46,167,255,.45)}70%{box-shadow:0 0 0 26px rgba(46,167,255,0)}100%{box-shadow:0 0 0 0 rgba(46,167,255,0)}}
.orb.thinking{border-color:var(--blue-neon);box-shadow:0 0 16px rgba(0,183,255,.3)}
.orb.thinking::before{content:'';position:absolute;inset:-3px;border-radius:50%;border:1.5px solid transparent;border-top-color:var(--blue-neon);border-right-color:rgba(0,183,255,.3);animation:spin .9s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}
.orb.speaking{border-color:var(--blue-bright);box-shadow:0 0 24px rgba(46,167,255,.4)}
.orb.speaking .wave{display:flex}
.wave{display:none;align-items:center;gap:3px;height:24px}
.wave span{width:3px;border-radius:2px;background:var(--blue-bright);animation:wave-bounce .9s ease-in-out infinite}
.wave span:nth-child(1){height:8px}
.wave span:nth-child(2){height:18px;animation-delay:.12s}
.wave span:nth-child(3){height:24px;animation-delay:.24s}
.wave span:nth-child(4){height:14px;animation-delay:.36s}
.wave span:nth-child(5){height:20px;animation-delay:.48s}
@keyframes wave-bounce{0%,100%{transform:scaleY(.4)}50%{transform:scaleY(1)}}
.orb.denied,.orb.mic-error{border-color:var(--red);box-shadow:0 0 16px rgba(255,92,108,.3)}
.orb.unsupported{border-color:var(--text-disabled)}
@media (prefers-reduced-motion:reduce){.orb,.orb::before,.wave span{animation:none!important}}
.state-text{margin-top:14px;font-size:14px;color:var(--text-secondary);font-family:'IBM Plex Mono',monospace;min-height:18px;text-align:center;position:relative}
.transcript-preview{margin-top:6px;font-size:13px;color:var(--blue-bright);text-align:center;min-height:16px;font-style:italic;position:relative}
.mic-indicator{display:none;align-items:center;gap:6px;margin-top:8px;font-size:11.5px;color:var(--blue-bright);font-family:'IBM Plex Mono',monospace}
.mic-indicator.active{display:flex}
.rec-dot{width:7px;height:7px;border-radius:50%;background:var(--red);box-shadow:0 0 6px rgba(255,92,108,.7);animation:rec-blink 1.1s ease-in-out infinite}
@keyframes rec-blink{0%,100%{opacity:1}50%{opacity:.3}}
.action-row{display:flex;gap:8px;margin-top:12px;flex-wrap:wrap;justify-content:center}
.ghost-btn{background:transparent;border:1px solid var(--border);color:var(--text-secondary);font-family:'Inter',sans-serif;font-size:12.5px;padding:7px 14px;border-radius:20px;cursor:pointer;transition:border-color .2s,color .2s;display:none;align-items:center;gap:6px}
.ghost-btn.visible{display:inline-flex}
.ghost-btn:hover{border-color:var(--blue-bright);color:var(--blue-bright)}
.ghost-btn.danger:hover{border-color:var(--red);color:var(--red)}
.ghost-btn.primary{border-color:var(--blue-primary);color:var(--blue-bright)}
.conversation{background:var(--surface);border:1px solid var(--border);border-radius:16px;padding:16px 18px;max-height:220px;overflow-y:auto;display:flex;flex-direction:column;gap:12px;position:relative;margin-top:16px}
.conversation:empty::before{content:'Toque no círculo, fale ou digite abaixo.';color:var(--text-disabled);font-size:13.5px}
.msg{font-size:14px;line-height:1.5}
.msg .role{font-family:'IBM Plex Mono',monospace;font-size:10.5px;letter-spacing:.06em;text-transform:uppercase;color:var(--text-disabled);display:block;margin-bottom:2px}
.msg.user .role{color:var(--blue-bright)}
.msg.assistant .role{color:var(--blue-neon)}
.msg p{margin:0}
.text-input-row{display:flex;gap:8px;margin-top:10px;position:relative}
.text-input{flex:1;background:var(--surface);border:1px solid var(--border);color:var(--text-primary);font-family:'Inter',sans-serif;font-size:13.5px;padding:10px 12px;border-radius:10px;outline:none}
.text-input:focus{border-color:var(--blue-bright)}
.send-btn{background:var(--blue-primary);border:none;color:#fff;font-size:13px;padding:0 16px;border-radius:10px;cursor:pointer}
.send-btn:hover{background:var(--blue-bright)}
.panel-label{font-family:'IBM Plex Mono',monospace;font-size:11px;letter-spacing:.08em;text-transform:uppercase;color:var(--text-disabled);margin:20px 0 10px;position:relative}
.list-empty{font-size:13px;color:var(--text-disabled);margin:0}
.list-item{display:flex;align-items:center;gap:10px;font-size:13.5px;padding:6px 0;border-bottom:1px solid var(--border);position:relative}
.list-item:last-child{border-bottom:none}
.dot{width:6px;height:6px;border-radius:50%;background:var(--blue-bright);box-shadow:0 0 5px rgba(46,167,255,.6);flex-shrink:0}
.list-item.done .dot{background:var(--blue-neon)}
.list-item.done .label{text-decoration:line-through;color:var(--text-disabled)}
.settings-row{margin-top:20px;padding-top:16px;border-top:1px solid var(--border);display:flex;flex-wrap:wrap;gap:8px;align-items:center;position:relative}
.settings-row.tight{margin-top:8px;border-top:none;padding-top:0}
.set-label{font-family:'IBM Plex Mono',monospace;font-size:10.5px;color:var(--text-disabled)}
.rate-btn,.toggle-btn{background:var(--surface);border:1px solid var(--border);color:var(--text-secondary);font-family:'IBM Plex Mono',monospace;font-size:11.5px;padding:6px 10px;border-radius:8px;cursor:pointer;transition:all .2s}
.rate-btn.active,.toggle-btn.active{border-color:var(--blue-bright);color:var(--blue-bright);box-shadow:0 0 8px rgba(46,167,255,.25)}
.rate-btn:hover,.toggle-btn:hover{border-color:var(--blue-bright)}
.forget-btn{margin-left:auto;background:transparent;border:1px solid var(--border);color:var(--text-disabled);font-family:'Inter',sans-serif;font-size:11.5px;padding:6px 10px;border-radius:8px;cursor:pointer}
.forget-btn:hover{border-color:var(--red);color:var(--red)}
#micSelect{background:var(--surface);border:1px solid var(--border);color:var(--text-secondary);font-family:'Inter',sans-serif;font-size:11.5px;padding:5px 8px;border-radius:8px;max-width:180px}
#diagPanel{display:none;margin-top:10px;background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:12px 14px;font-family:'IBM Plex Mono',monospace;font-size:11.5px}
#diagPanel div{padding:3px 0;color:var(--text-secondary)}
#diagPanel .ok{color:var(--blue-neon)}
#diagPanel .bad{color:var(--red)}
</style>
</head>
<body>
<div class="app">
  <div class="header">
    <p class="eyebrow">Núcleo de voz</p>
    <h1>Vida OS</h1>
  </div>
  <p class="voice-badge" id="voiceBadge">Carregando voz…</p>
  <p class="secure-warning" id="secureWarning">Abra esta página por http://localhost ou HTTPS. Em file:// o microfone pode não funcionar.</p>

  <div class="orb-wrap">
    <div class="orb idle" id="orb">
      <span class="orb-icon" id="orbIcon">🎙️ Falar com o Vida</span>
      <div class="wave"><span></span><span></span><span></span><span></span><span></span></div>
    </div>
    <p class="state-text" id="stateText">Estou aqui.</p>
    <p class="transcript-preview" id="transcriptPreview"></p>
    <div class="mic-indicator" id="micIndicator"><span class="rec-dot"></span><span>Microfone ativo</span></div>
    <div class="action-row">
      <button class="ghost-btn danger" id="stopListeningBtn">⏹ Parar de ouvir</button>
      <button class="ghost-btn" id="stopSpeakingBtn">⏹ Parar</button>
      <button class="ghost-btn primary" id="retryBtn">🔁 Tentar novamente</button>
    </div>
  </div>

  <div class="conversation" id="conversation"></div>

  <div class="text-input-row">
    <input type="text" class="text-input" id="textInput" placeholder="Ou digite aqui e pressione Enter...">
    <button class="send-btn" id="sendBtn">Enviar</button>
  </div>

  <p class="panel-label">Tarefas e lembretes</p>
  <div id="itemsPanel"><p class="list-empty">Nada por aqui ainda.</p></div>

  <div class="settings-row">
    <span class="set-label">RESPOSTA</span>
    <button class="toggle-btn active" data-mode="voice">🎙️ Voz</button>
    <button class="toggle-btn" data-mode="text">⌨️ Texto</button>
  </div>
  <div class="settings-row tight">
    <span class="set-label">VELOCIDADE</span>
    <button class="rate-btn" data-rate="0.95">Mais lento</button>
    <button class="rate-btn active" data-rate="1.1">Natural</button>
    <button class="rate-btn" data-rate="1.25">Mais rápido</button>
    <button class="forget-btn" id="forgetBtn">Apagar memória</button>
  </div>
  <div class="settings-row tight">
    <span class="set-label">MICROFONE</span>
    <select id="micSelect"><option value="">Padrão do sistema</option></select>
    <button class="forget-btn" id="diagBtn">🩺 Diagnóstico</button>
  </div>
  <div id="diagPanel"></div>
</div>

<script>
/* ===== Utilidades ===== */
const esc = (s) => String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));

/* ===== MicrophoneService — permissão e disponibilidade do microfone ===== */
const MicrophoneService = (() => {
  let selectedDeviceId = null;
  const log = (l, v) => console.log(`[VIDA MICROPHONE] ${l}:`, v !== undefined ? v : '');
  const isMediaApiSupported = () => !!(navigator.mediaDevices && navigator.mediaDevices.getUserMedia);

  const getContextIssue = () => {
    if (location.protocol === 'file:') return 'FILE_PROTOCOL';
    if (!window.isSecureContext) return 'INSECURE_HTTP';
    return null;
  };

  const getAvailableMicrophones = async () => {
    if (!navigator.mediaDevices || !navigator.mediaDevices.enumerateDevices) return [];
    try {
      const devices = await navigator.mediaDevices.enumerateDevices();
      return devices.filter(d => d.kind === 'audioinput').map(d => ({
        deviceId: d.deviceId, label: d.label || 'Microfone'
      }));
    } catch (e) { log('enumerate error', e); return []; }
  };

  const checkPermission = async () => {
    if (!navigator.permissions || !navigator.permissions.query) return 'unknown';
    try { return (await navigator.permissions.query({ name: 'microphone' })).state; }
    catch (e) { return 'unknown'; }
  };

  const selectMicrophone = (id) => { selectedDeviceId = id || null; };

  const requestPermission = async () => {
    const issue = getContextIssue();
    if (issue) return { ok: false, reason: issue };
    if (!isMediaApiSupported()) return { ok: false, reason: 'NOT_SUPPORTED' };
    try {
      const audio = selectedDeviceId ? { deviceId: { exact: selectedDeviceId } } : true;
      const stream = await navigator.mediaDevices.getUserMedia({ audio, video: false });
      const tracks = stream.getAudioTracks();
      const label = tracks[0] ? tracks[0].label : '';
      stream.getTracks().forEach(t => t.stop()); // o SpeechRecognition abre o mic por conta própria
      if (!tracks.length) return { ok: false, reason: 'NO_AUDIO_TRACK' };
      return { ok: true, deviceLabel: label };
    } catch (err) {
      log('getUserMedia error', `${err.name}: ${err.message}`);
      const map = {
        NotAllowedError: 'DENIED', PermissionDeniedError: 'DENIED',
        NotFoundError: 'NO_DEVICE', DevicesNotFoundError: 'NO_DEVICE',
        NotReadableError: 'BUSY', TrackStartError: 'BUSY',
        OverconstrainedError: 'OVERCONSTRAINED', SecurityError: 'INSECURE_HTTP', AbortError: 'ABORTED'
      };
      return { ok: false, reason: map[err.name] || 'ERROR' };
    }
  };

  const runDiagnostics = async () => {
    const r = [];
    r.push({ label: 'Navegador compatível (mediaDevices)', ok: isMediaApiSupported() });
    const issue = getContextIssue();
    r.push({ label: 'Contexto seguro (HTTPS/localhost)', ok: !issue, detail: issue || location.protocol });
    const perm = await checkPermission();
    r.push({ label: 'Permissão do microfone', ok: perm !== 'denied', detail: perm });
    let streamOk = false, trackOk = false;
    if (isMediaApiSupported()) {
      try {
        const s = await navigator.mediaDevices.getUserMedia({ audio: true });
        streamOk = true; trackOk = s.getAudioTracks().length > 0;
        s.getTracks().forEach(t => t.stop());
      } catch (e) { log('diag stream error', e.name); }
    }
    const mics = await getAvailableMicrophones();
    r.push({ label: 'Dispositivo de áudio detectado', ok: mics.length > 0, detail: `${mics.length} encontrado(s)` });
    r.push({ label: 'MediaStream obtido', ok: streamOk });
    r.push({ label: 'Faixa de áudio presente', ok: trackOk });
    r.push({ label: 'Reconhecimento de voz (SpeechRecognition)', ok: !!(window.SpeechRecognition || window.webkitSpeechRecognition) });
    r.push({ label: 'Síntese de voz (speechSynthesis)', ok: 'speechSynthesis' in window });
    return r;
  };

  return { isMediaApiSupported, getAvailableMicrophones, selectMicrophone, checkPermission, requestPermission, getContextIssue, runDiagnostics };
})();

/* ===== VoiceSettings ===== */
const VoiceSettings = (() => {
  const s = { rate: 1.1, lang: 'pt-BR', voiceEnabled: true };
  return {
    setRate: (r) => { s.rate = r; }, getRate: () => s.rate, getLang: () => s.lang,
    setVoiceEnabled: (v) => { s.voiceEnabled = v; }, isVoiceEnabled: () => s.voiceEnabled
  };
})();

/* ===== VoiceProfile — escolhe a melhor voz pt-BR do navegador ===== */
const VoiceProfile = (() => {
  let selectedVoice = null;
  let readyCallback = null;

  const pickVoice = () => {
    const voices = window.speechSynthesis ? window.speechSynthesis.getVoices() : [];
    if (!voices.length) return null;
    const ptBR = voices.filter(v => (v.lang || '').toLowerCase().replace('_', '-') === 'pt-br');
    const pt = voices.filter(v => (v.lang || '').toLowerCase().startsWith('pt'));
    const pool = ptBR.length ? ptBR : pt.length ? pt : voices;
    const maleHints = ['daniel', 'ricardo', 'felipe', 'antonio', 'antônio', 'diego', 'thiago', 'male', 'homem'];
    const male = pool.find(v => maleHints.some(h => v.name.toLowerCase().includes(h)));
    selectedVoice = male || pool[0];
    return selectedVoice;
  };

  const init = () => {
    if (!('speechSynthesis' in window)) return;
    pickVoice();
    window.speechSynthesis.onvoiceschanged = () => { pickVoice(); if (readyCallback) readyCallback(); };
  };

  return { init, getVoice: () => selectedVoice, onReady: (cb) => { readyCallback = cb; } };
})();

/* ===== SpeechFormatter — limpa e divide o texto em frases curtas ===== */
const SpeechFormatter = (() => {
  const clean = (text) => text
    .replace(/\*\*(.*?)\*\*/g, '$1')
    .replace(/\*(.*?)\*/g, '$1')
    .replace(/`([^`]*)`/g, '$1')
    .replace(/#+\s*/g, '')
    .replace(/https?:\/\/\S+/g, '')
    .replace(/[\u{1F300}-\u{1FAFF}\u{2600}-\u{27BF}]/gu, '')
    .replace(/\s+/g, ' ')
    .trim();
  const splitSentences = (text) => text.split(/(?<=[.!?])\s+/).map(s => s.trim()).filter(Boolean);
  return { clean, splitSentences };
})();

/* ===== SpeechRecognitionService — STT via Web Speech API ===== */
const SpeechRecognitionService = (() => {
  const Recognition = window.SpeechRecognition || window.webkitSpeechRecognition;
  let recognition = null;
  const cb = { start: null, interim: null, result: null, error: null, end: null };

  const isSupported = () => !!Recognition;

  const init = () => {
    if (!isSupported()) return;
    recognition = new Recognition();
    recognition.lang = 'pt-BR';
    recognition.continuous = false;
    recognition.interimResults = true;
    recognition.maxAlternatives = 1;
    recognition.onstart = () => cb.start && cb.start();
    recognition.onresult = (event) => {
      let finalText = '', interimText = '';
      for (let i = event.resultIndex; i < event.results.length; i++) {
        const t = event.results[i][0].transcript;
        if (event.results[i].isFinal) finalText += t; else interimText += t;
      }
      if (interimText && cb.interim) cb.interim(interimText);
      if (finalText && cb.result) cb.result(finalText.trim());
    };
    recognition.onerror = (e) => cb.error && cb.error(e.error);
    recognition.onend = () => cb.end && cb.end();
  };

  const start = () => {
    if (!recognition) init();
    if (!recognition) return false;
    try { recognition.start(); return true; } catch (e) { return false; }
  };
  const stop = () => { if (recognition) recognition.stop(); };

  return {
    isSupported, start, stop,
    onStart: (f) => { cb.start = f; }, onInterim: (f) => { cb.interim = f; },
    onResult: (f) => { cb.result = f; }, onError: (f) => { cb.error = f; }, onEnd: (f) => { cb.end = f; }
  };
})();

/* ===== TextToSpeechService — TTS frase a frase ===== */
const TextToSpeechService = (() => {
  const isSupported = () => 'speechSynthesis' in window;
  const PAUSE_MS = 160;
  let runId = 0;

  const speakOne = (sentence) => new Promise((resolve) => {
    const u = new SpeechSynthesisUtterance(sentence);
    u.lang = VoiceSettings.getLang();
    u.rate = VoiceSettings.getRate();
    u.pitch = 0.85;
    u.volume = 1;
    const voice = VoiceProfile.getVoice();
    if (voice) u.voice = voice;
    u.onend = resolve;
    u.onerror = resolve;
    window.speechSynthesis.speak(u);
  });

  const speak = async (text, { onStart, onEnd } = {}) => {
    if (!VoiceSettings.isVoiceEnabled() || !isSupported() || !text) { if (onEnd) onEnd(); return; }
    window.speechSynthesis.cancel();
    const myRun = ++runId;
    const sentences = SpeechFormatter.splitSentences(SpeechFormatter.clean(text));
    if (onStart) onStart();
    for (const s of sentences) {
      if (myRun !== runId) return; // foi interrompida ou substituída
      await speakOne(s);
      if (myRun !== runId) return;
      await new Promise(r => setTimeout(r, PAUSE_MS));
    }
    if (myRun === runId && onEnd) onEnd();
  };

  const stop = () => { runId++; if (isSupported()) window.speechSynthesis.cancel(); };
  return { isSupported, speak, stop };
})();

/* ===== MemoryService ===== */
const MemoryService = (() => {
  const shortTerm = { awaiting: null, pendingData: null };
  const longTerm = { tasks: [], reminders: [], goals: [], notes: [] };
  const conversation = [];
  const remember = (patch) => Object.assign(shortTerm, patch);
  const recall = () => ({ ...shortTerm });
  const clearShortTerm = () => { shortTerm.awaiting = null; shortTerm.pendingData = null; };
  const uid = () => (window.crypto && crypto.randomUUID) ? crypto.randomUUID() : String(Date.now() + Math.random());
  const addMessage = (role, content, type = 'text') => {
    const m = { id: uid(), role, content, timestamp: new Date().toISOString(), type };
    conversation.push(m);
    return m;
  };
  const forgetAll = () => {
    longTerm.tasks = []; longTerm.reminders = []; longTerm.goals = []; longTerm.notes = [];
    conversation.length = 0; clearShortTerm();
  };
  return { shortTerm, longTerm, remember, recall, clearShortTerm, addMessage, conversation, forgetAll };
})();

/* ===== CommandInterpreter — linguagem natural → intenção ===== */
const CommandInterpreter = (() => {
  const norm = (s) => s.toLowerCase().trim();

  // Só reconhece horário com marcador explícito: "às 8", "8h", "8:30", "8 horas".
  const extractTime = (text) => {
    const m = text.match(/(?:\b(?:às|as|para as)\s+(\d{1,2})(?::(\d{2}))?|\b(\d{1,2})(?::(\d{2}))?\s*(?:h\b|horas?\b))(?:\s*e\s*(\d{1,2})\s*(?:minutos?|min)\b)?/);
    if (!m) return null;
    const hour = parseInt(m[1] || m[3], 10);
    const min = parseInt(m[2] || m[4] || m[5] || '0', 10);
    if (hour > 23 || min > 59) return null;
    return `${String(hour).padStart(2, '0')}:${String(min).padStart(2, '0')}`;
  };

  const extractQuantity = (text) => {
    const m = text.match(/(\d+(?:[.,]\d+)?)\s*(mil|k)?/i);
    if (!m) return null;
    let v = parseFloat(m[1].replace(',', '.'));
    if (m[2]) v *= 1000;
    return v || null;
  };

  const interpret = (rawText) => {
    const text = norm(rawText);
    const ctx = MemoryService.recall();

    if (ctx.awaiting === 'GOAL_DEADLINE' && /^\d{1,2}\s*(mes|mês|meses|dia|dias|semana|semanas|ano|anos)/.test(text)) {
      return { intent: 'CREATE_GOAL', data: { ...ctx.pendingData, deadline: text } };
    }
    if (ctx.awaiting === 'ORGANIZE_ITEMS') {
      const items = text.split(/,| e /).map(s => s.trim()).filter(Boolean);
      return { intent: 'ORGANIZE_ITEMS_RECEIVED', data: { items } };
    }
    if (ctx.awaiting === 'ORGANIZE_PRIORITY') {
      const chosen = ((ctx.pendingData && ctx.pendingData.items) || []).find(i => text.includes(i) || i.includes(text)) || text;
      return { intent: 'ORGANIZE_PRIORITY_RECEIVED', data: { chosen } };
    }
    if (ctx.awaiting === 'GOAL_TITLE') {
      return { intent: 'CREATE_GOAL', data: { title: rawText.trim(), amount: extractQuantity(text) } };
    }
    if (ctx.awaiting === 'CHANGE_GOAL_TARGET') {
      const goal = MemoryService.longTerm.goals.find(g => text.includes(g.title.toLowerCase()) || g.title.toLowerCase().includes(text));
      return { intent: 'CHANGE_GOAL_TARGET_RECEIVED', data: { goal } };
    }
    if (ctx.awaiting === 'CHANGE_GOAL_NEW_TITLE') {
      return { intent: 'CHANGE_GOAL_APPLIED', data: { newTitle: rawText.trim(), goal: ctx.pendingData.goal } };
    }

    if (/(^|\s)(pare|para) de falar|^(vida,?\s*)?pare$|^stop$|^silêncio$/.test(text)) return { intent: 'STOP_SPEAKING', data: {} };
    if (/(você é humano|voce e humano|você é real|é uma pessoa)/.test(text)) return { intent: 'HUMAN_QUESTION', data: {} };
    if (/(quem é você|quem e voce|o que você é|se apresenta)/.test(text)) return { intent: 'IDENTITY_QUESTION', data: {} };
    if (/(obrigad|valeu|brigad[oa])/.test(text)) return { intent: 'GRATITUDE', data: {} };
    if (/(quero mudar minha meta|mudar (a )?meta|trocar (a )?meta)/.test(text)) return { intent: 'CHANGE_GOAL', data: {} };
    if (/(você lembra|voce lembra|lembra (de|daquele|daquela))/.test(text) && !extractTime(text)) {
      const keywords = text.replace(/(você lembra|voce lembra|lembra (de|daquele|daquela))/g, '').trim();
      return { intent: 'MEMORY_RECALL', data: { keywords } };
    }
    if (/(quero criar uma meta$|crie uma meta$|nova meta$|^meta$)/.test(text)) return { intent: 'CREATE_GOAL_BARE', data: {} };
    if (/(estou cansad|tô cansad|to cansad|dia pesado|dia cansativo)/.test(text)) return { intent: 'TIRED_CHECKIN', data: {} };
    if (/(estou perdido|to perdido|tô perdido|muita coisa (pra|para) fazer|quero organizar meu dia|me ajuda a organizar)/.test(text)) return { intent: 'ORGANIZE_DAY', data: {} };
    if (/(não entendi|nao entendi|não compreendi|nao compreendi)/.test(text) && text.length < 30) return { intent: 'CLARIFY', data: {} };
    if (/^(oi|olá|ola|vida|e aí|e ai|eae|bom dia|boa tarde|boa noite)\b/.test(text) && text.length < 20) return { intent: 'GREETING', data: {} };
    if (/(o que você faz|o que voce faz|ajuda|help|o que posso pedir)/.test(text)) return { intent: 'HELP', data: {} };
    if (/(apaga|apague|remove|deleta).*(essa tarefa|a tarefa|tarefa)/.test(text)) return { intent: 'DELETE_TASK', data: {} };

    if (/(me lembr|lembrete|lembra de|coloca.*(pra|para)|anota.*(horário|hora))/.test(text) && extractTime(text)) {
      const time = extractTime(text);
      const title = text
        .replace(/(vida,?|me lembr\w*( de)?|quero um lembrete( para)?|anota aí que( preciso)?|coloca)/g, '')
        .replace(/(?:\b(?:às|as|para as)\s+)?\d{1,2}(?::\d{2})?\s*(?:h\b|horas?\b)?(?:\s*e\s*\d{1,2}\s*(?:minutos?|min)\b)?/g, '')
        .replace(/\s+/g, ' ').trim();
      return { intent: 'CREATE_REMINDER', data: { title: title || 'Lembrete', time } };
    }
    if (/(meus lembretes|listar lembretes|quais.*lembretes)/.test(text)) return { intent: 'LIST_REMINDERS', data: {} };
    if (/(marca.*conclu|termin\w+ a tarefa|conclu\w+ a tarefa|completa a tarefa)/.test(text)) {
      const title = text.replace(/(marca|marque|termin\w+|conclu\w+|completa)( a)? tarefa/g, '').replace(/(como )?conclu\w*/g, '').trim();
      return { intent: 'COMPLETE_TASK', data: { title } };
    }
    if (/(cria\w* uma tarefa|nova tarefa|adiciona.*tarefa)/.test(text)) {
      const title = text
        .replace(/(vida,?|cria\w* uma tarefa( chamada)?|nova tarefa|adiciona.*tarefa( chamada)?)/g, '')
        .replace(/(para|pra) (amanhã|amanha|hoje)/g, '').trim();
      return { intent: 'CREATE_TASK', data: { title: title || 'Nova tarefa' } };
    }
    if (/(minhas tarefas|quais.*tarefas|o que eu tenho|tenho alguma coisa (pra|para) fazer|como está meu dia|como esta meu dia|me mostra minha agenda|minha missão de hoje|minha missao de hoje)/.test(text)) {
      return { intent: 'GET_TODAY_PLAN', data: {} };
    }
    if (/(cria\w* uma meta|nova meta|quero economizar|quero uma meta)/.test(text)) {
      const amount = extractQuantity(text);
      const title = text.replace(/(vida,?|cria\w* uma meta( de)?|nova meta( de)?|quero economizar|quero uma meta( de)?)/g, '').trim();
      return { intent: 'CREATE_GOAL', data: { title: title || 'Nova meta', amount } };
    }
    if (/(minhas metas|listar metas|quais.*metas)/.test(text)) return { intent: 'LIST_GOALS', data: {} };
    if (/(anota que|salva uma nota|anota aí que|guarda essa informação)/.test(text)) {
      const content = text.replace(/(vida,?|anota( aí)? que|salva uma nota( de)?|guarda essa informação( de)?)/g, '').trim();
      return { intent: 'SAVE_NOTE', data: { content } };
    }
    if (/(o que eu anotei|minha nota|busca a nota)/.test(text)) return { intent: 'GET_NOTE', data: {} };
    return { intent: 'UNKNOWN_COMMAND', data: { raw: rawText } };
  };

  return { interpret };
})();

/* ===== CommandExecutor — executa e gera a resposta ===== */
const CommandExecutor = (() => {
  const pick = (arr) => arr[Math.floor(Math.random() * arr.length)];
  const plural = (n, s, p) => n === 1 ? s : p;

  const execute = ({ intent, data }) => {
    const { tasks, reminders, goals, notes } = MemoryService.longTerm;
    switch (intent) {
      case 'GREETING':
        return pick(['Olá. Estou aqui. O que vamos resolver hoje?', 'Oi. Pode falar, estou ouvindo.', 'Estou aqui. O que você precisa?']);
      case 'GRATITUDE':
        return pick(['De nada. Estou aqui quando precisar.', 'Por nada. Qualquer coisa, é só chamar.', 'Sempre que precisar.']);
      case 'HELP':
        return 'Posso criar e listar tarefas, lembretes e metas, anotar coisas para você e contar como está o seu dia. Também converso, se você só quiser pensar em voz alta.';
      case 'IDENTITY_QUESTION':
        return 'Eu sou o Vida. Fui criado para te ajudar a organizar sua rotina, seus objetivos e o que for importante no seu dia a dia.';
      case 'HUMAN_QUESTION':
        return 'Não. Eu sou uma inteligência artificial criada para ajudar você, mas fui projetado para conversar de forma natural.';
      case 'TIRED_CHECKIN':
        return 'Entendi. Seu dia parece ter sido pesado. Quer que eu reorganize o restante da sua rotina para deixar apenas o que é realmente importante hoje?';
      case 'CLARIFY':
        return 'Sem problema. Pode me contar de outro jeito o que você precisa? Assim eu entendo melhor.';
      case 'ORGANIZE_DAY':
        MemoryService.remember({ awaiting: 'ORGANIZE_ITEMS', pendingData: {} });
        return 'Vamos organizar isso juntos. Me diga o que precisa ser resolvido hoje e eu separo o que é urgente do que pode esperar.';
      case 'ORGANIZE_ITEMS_RECEIVED':
        MemoryService.remember({ awaiting: 'ORGANIZE_PRIORITY', pendingData: { items: data.items } });
        return 'Certo. Qual dessas é mais importante hoje?';
      case 'ORGANIZE_PRIORITY_RECEIVED':
        MemoryService.clearShortTerm();
        return `Então vamos proteger seu horário de ${data.chosen} primeiro. Depois encaixamos o resto sem sobrecarregar seu dia.`;
      case 'STOP_SPEAKING':
        TextToSpeechService.stop();
        return null;
      case 'CREATE_REMINDER':
        reminders.push({ title: data.title, time: data.time, done: false });
        renderItems();
        return pick([`Pronto. Vou lembrar você de ${data.title} às ${data.time}.`, `Claro. Reservei ${data.time} para ${data.title}.`, `Certo, já deixei isso marcado para ${data.time}.`]);
      case 'LIST_REMINDERS':
        if (!reminders.length) return 'Você não tem lembretes ainda.';
        return `Você tem ${reminders.length} ${plural(reminders.length, 'lembrete', 'lembretes')}: ${reminders.map(r => `${r.title} às ${r.time}`).join(', ')}.`;
      case 'CREATE_TASK':
        tasks.push({ title: data.title, done: false });
        renderItems();
        return pick([`Pronto. Criei a tarefa ${data.title}.`, `Feito. Já adicionei ${data.title} nas suas tarefas.`, `Pode deixar, já está na lista: ${data.title}.`]);
      case 'DELETE_TASK': {
        if (!tasks.length) return 'Você não tem tarefas para apagar.';
        const removed = tasks.pop();
        renderItems();
        return `Pronto. Removi "${removed.title}" das suas tarefas.`;
      }
      case 'COMPLETE_TASK': {
        const task = tasks.find(t => data.title && (t.title.includes(data.title) || data.title.includes(t.title)));
        if (!task) return 'Não encontrei essa tarefa. Pode repetir o nome dela?';
        task.done = true;
        renderItems();
        return pick([`Show. Marquei ${task.title} como concluída.`, `Boa. ${task.title} está concluída.`]);
      }
      case 'LIST_TASKS': {
        if (!tasks.length) return 'Você não tem tarefas ainda.';
        const pending = tasks.filter(t => !t.done);
        return `Você tem ${tasks.length} ${plural(tasks.length, 'tarefa', 'tarefas')}, ${pending.length} ${plural(pending.length, 'pendente', 'pendentes')}: ${tasks.map(t => t.title).join(', ')}.`;
      }
      case 'CREATE_GOAL_BARE':
        MemoryService.remember({ awaiting: 'GOAL_TITLE', pendingData: {} });
        return 'Boa. Qual é a meta que você quer criar?';
      case 'CREATE_GOAL': {
        const existing = goals.find(g => g.title === data.title);
        if (existing && data.deadline) {
          existing.deadline = data.deadline;
          renderItems();
          MemoryService.clearShortTerm();
          return `Combinado. Marquei o prazo de ${data.deadline} para a meta ${existing.title}.`;
        }
        goals.push({ title: data.title, amount: data.amount, deadline: data.deadline || null });
        renderItems();
        if (!data.deadline) {
          MemoryService.remember({ awaiting: 'GOAL_DEADLINE', pendingData: { title: data.title, amount: data.amount } });
          return `Boa. Criei sua meta de ${data.title}. Em quanto tempo você quer alcançar isso?`;
        }
        MemoryService.clearShortTerm();
        return `Boa. Criei sua meta de ${data.title}.`;
      }
      case 'CHANGE_GOAL':
        if (!goals.length) return 'Você ainda não tem metas para mudar. Quer criar uma?';
        MemoryService.remember({ awaiting: 'CHANGE_GOAL_TARGET', pendingData: {} });
        return `Claro. Qual meta você quer mudar? Você tem: ${goals.map(g => g.title).join(', ')}.`;
      case 'CHANGE_GOAL_TARGET_RECEIVED':
        if (!data.goal) return 'Não encontrei essa meta. Pode repetir o nome dela?';
        MemoryService.remember({ awaiting: 'CHANGE_GOAL_NEW_TITLE', pendingData: { goal: data.goal } });
        return `Certo. Qual é o novo nome para a meta ${data.goal.title}?`;
      case 'CHANGE_GOAL_APPLIED':
        data.goal.title = data.newTitle;
        MemoryService.clearShortTerm();
        renderItems();
        return `Pronto. Agora a meta é ${data.newTitle}.`;
      case 'LIST_GOALS':
        if (!goals.length) return 'Você ainda não tem metas registradas.';
        return `Você tem ${goals.length} ${plural(goals.length, 'meta', 'metas')}: ${goals.map(g => g.title).join(', ')}.`;
      case 'GET_TODAY_PLAN': {
        const pending = tasks.filter(t => !t.done);
        if (!pending.length && !reminders.length) return 'Você não tem nada registrado para hoje ainda. Quer que eu te ajude a organizar o dia?';
        const parts = [];
        if (pending.length) parts.push(`${pending.length} ${plural(pending.length, 'tarefa pendente', 'tarefas pendentes')}`);
        if (reminders.length) parts.push(`${reminders.length} ${plural(reminders.length, 'lembrete', 'lembretes')}`);
        return `Hoje você tem ${parts.join(' e ')}.`;
      }
      case 'SAVE_NOTE':
        notes.push({ content: data.content, createdAt: new Date().toISOString() });
        return pick(['Anotado. Vou guardar isso para você.', 'Pode deixar, guardei isso.', 'Pronto, já deixei salvo.', 'Certo. Está salvo.']);
      case 'GET_NOTE':
        if (!notes.length) return 'Você ainda não tem notas salvas.';
        return `Sua última nota é: ${notes[notes.length - 1].content}.`;
      case 'MEMORY_RECALL': {
        const words = (data.keywords || '').split(/\s+/).filter(w => w.length > 2);
        const found = words.length ? notes.filter(n => words.some(w => n.content.includes(w))) : [];
        if (!found.length) return 'Não encontrei nada sobre isso nas suas notas.';
        return `Encontrei isto: ${found[found.length - 1].content}.`;
      }
      default:
        return pick(['Não consegui entender essa parte. Pode me explicar de outro jeito?', 'Ainda não sei fazer isso, mas posso aprender em versões futuras.']);
    }
  };

  return { execute };
})();

/* ===== VoiceAssistant — orquestra o fluxo completo ===== */
const VoiceAssistant = (() => {
  const STATES = { IDLE: 'idle', REQUESTING: 'requesting', LISTENING: 'listening', THINKING: 'thinking', SPEAKING: 'speaking', DENIED: 'denied', MIC_ERROR: 'mic-error', UNSUPPORTED: 'unsupported' };
  const STATE_LABELS = {
    idle: 'Estou aqui.', requesting: 'Solicitando acesso...', listening: 'Estou ouvindo...',
    thinking: 'Estou pensando...', speaking: 'Estou falando...',
    denied: 'Não consegui acessar seu microfone.', 'mic-error': 'Não consegui acessar seu microfone.',
    unsupported: 'O reconhecimento de voz não está disponível neste navegador.'
  };
  const ORB_LABELS = {
    idle: '🎙️ Falar com o Vida', requesting: 'Solicitando acesso...', listening: '', thinking: '', speaking: '',
    denied: '⚠️ Acesso negado', 'mic-error': '⚠️ Erro no microfone', unsupported: '⌨️ Use o modo texto'
  };

  const $ = (id) => document.getElementById(id);
  const orb = $('orb'), orbIcon = $('orbIcon'), stateText = $('stateText'), transcriptPreview = $('transcriptPreview');
  const conversationEl = $('conversation'), micIndicator = $('micIndicator');
  const stopListeningBtn = $('stopListeningBtn'), stopSpeakingBtn = $('stopSpeakingBtn'), retryBtn = $('retryBtn');

  let currentState = STATES.IDLE;

  const setState = (state, customText) => {
    currentState = state;
    orb.className = `orb ${state}`;
    stateText.textContent = customText || STATE_LABELS[state];
    orbIcon.textContent = ORB_LABELS[state];
    micIndicator.classList.toggle('active', state === STATES.LISTENING);
    stopListeningBtn.classList.toggle('visible', state === STATES.LISTENING);
    stopSpeakingBtn.classList.toggle('visible', state === STATES.SPEAKING);
    retryBtn.classList.toggle('visible', state === STATES.DENIED || state === STATES.MIC_ERROR);
    if (state !== STATES.LISTENING) transcriptPreview.textContent = '';
  };

  const renderMessage = (role, content) => {
    const div = document.createElement('div');
    div.className = `msg ${role}`;
    const r = document.createElement('span');
    r.className = 'role';
    r.textContent = role === 'user' ? 'você' : 'vida';
    const p = document.createElement('p');
    p.textContent = content;
    div.append(r, p);
    conversationEl.appendChild(div);
    conversationEl.scrollTop = conversationEl.scrollHeight;
  };

  const processText = (text) => {
    renderMessage('user', text);
    MemoryService.addMessage('user', text);
    setState(STATES.THINKING);

    const { intent, data } = CommandInterpreter.interpret(text);
    const replyText = CommandExecutor.execute({ intent, data });

    if (!replyText) { setState(STATES.IDLE); return; }

    renderMessage('assistant', replyText);
    MemoryService.addMessage('assistant', replyText);

    if (!VoiceSettings.isVoiceEnabled() || !TextToSpeechService.isSupported()) { setState(STATES.IDLE); return; }
    setState(STATES.SPEAKING);
    TextToSpeechService.speak(replyText, {
      onEnd: () => { if (currentState === STATES.SPEAKING) setState(STATES.IDLE); }
    });
  };

  const beginListening = () => {
    SpeechRecognitionService.onStart(() => setState(STATES.LISTENING));
    SpeechRecognitionService.onInterim((partial) => { transcriptPreview.textContent = partial; });
    SpeechRecognitionService.onResult((text) => { transcriptPreview.textContent = ''; processText(text); });
    SpeechRecognitionService.onError((err) => {
      if (err === 'not-allowed' || err === 'service-not-allowed' || err === 'permission-denied') {
        setState(STATES.DENIED, 'Não consegui acessar seu microfone. Permita o acesso nas configurações do navegador.');
      } else if (err === 'no-speech') {
        setState(STATES.IDLE, 'Não ouvi nada. Toque para tentar de novo.');
      } else if (err === 'aborted') {
        if (currentState === STATES.LISTENING) setState(STATES.IDLE);
      } else if (err === 'audio-capture') {
        setState(STATES.MIC_ERROR, 'Não encontramos um microfone disponível neste dispositivo.');
      } else if (err === 'network') {
        setState(STATES.MIC_ERROR, 'Erro de rede no reconhecimento de voz. Tente novamente.');
      } else {
        setState(STATES.MIC_ERROR, 'Não consegui entender completamente. Pode repetir?');
      }
    });
    SpeechRecognitionService.onEnd(() => { if (currentState === STATES.LISTENING) setState(STATES.IDLE); });

    if (!SpeechRecognitionService.start()) setState(STATES.MIC_ERROR, 'Não consegui iniciar o reconhecimento de voz.');
  };

  const activate = async () => {
    if (currentState === STATES.LISTENING || currentState === STATES.REQUESTING) return;
    TextToSpeechService.stop();

    if (!SpeechRecognitionService.isSupported()) { setState(STATES.UNSUPPORTED); return; }

    setState(STATES.REQUESTING);
    const permission = await MicrophoneService.requestPermission();

    if (!permission.ok) {
      const msgs = {
        DENIED: 'Não consegui acessar seu microfone. Permita o acesso nas configurações do navegador para conversar por voz.',
        NO_DEVICE: 'Não encontramos um microfone disponível. Você ainda pode usar o modo texto.',
        BUSY: 'O microfone está sendo usado por outro aplicativo. Feche-o e tente de novo.',
        FILE_PROTOCOL: 'A página foi aberta como arquivo (file://). Abra por http://localhost ou HTTPS.',
        INSECURE_HTTP: 'O microfone só funciona em HTTPS ou localhost.',
        OVERCONSTRAINED: 'O microfone escolhido não está disponível. Selecione outro.'
      };
      if (permission.reason === 'NOT_SUPPORTED') { setState(STATES.UNSUPPORTED); return; }
      const state = permission.reason === 'DENIED' ? STATES.DENIED : STATES.MIC_ERROR;
      setState(state, msgs[permission.reason] || 'Não consegui acessar seu microfone. Verifique a permissão do navegador.');
      return;
    }

    refreshMicList(); // agora os nomes dos microfones ficam disponíveis
    beginListening();
  };

  const stopListening = () => { SpeechRecognitionService.stop(); setState(STATES.IDLE); };
  const interruptSpeaking = () => { TextToSpeechService.stop(); setState(STATES.IDLE); };

  orb.addEventListener('click', activate);
  stopListeningBtn.addEventListener('click', stopListening);
  stopSpeakingBtn.addEventListener('click', interruptSpeaking);
  retryBtn.addEventListener('click', activate);

  return { activate, processText, setState, STATES };
})();

/* ===== Painel de tarefas/lembretes/metas ===== */
function renderItems() {
  const panel = document.getElementById('itemsPanel');
  const { tasks, reminders, goals } = MemoryService.longTerm;
  const items = [
    ...tasks.map(t => ({ label: t.title, done: t.done })),
    ...reminders.map(r => ({ label: `${r.title} · ${r.time}`, done: false })),
    ...goals.map(g => ({ label: g.title + (g.deadline ? ` · ${g.deadline}` : ''), done: false }))
  ];
  if (!items.length) { panel.innerHTML = '<p class="list-empty">Nada por aqui ainda.</p>'; return; }
  panel.innerHTML = items.map(i =>
    `<div class="list-item ${i.done ? 'done' : ''}"><span class="dot"></span><span class="label">${esc(i.label)}</span></div>`
  ).join('');
}

/* ===== Entrada de texto ===== */
const textInput = document.getElementById('textInput');
function submitTextInput() {
  const value = textInput.value.trim();
  if (!value) return;
  textInput.value = '';
  VoiceAssistant.processText(value);
}
document.getElementById('sendBtn').addEventListener('click', submitTextInput);
textInput.addEventListener('keydown', (e) => { if (e.key === 'Enter') submitTextInput(); });

/* ===== Toggle Voz / Texto ===== */
document.querySelectorAll('.toggle-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.toggle-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    VoiceSettings.setVoiceEnabled(btn.dataset.mode === 'voice');
    if (btn.dataset.mode === 'text') TextToSpeechService.stop();
  });
});

/* ===== Velocidade ===== */
document.querySelectorAll('.rate-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.rate-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    VoiceSettings.setRate(parseFloat(btn.dataset.rate));
  });
});

/* ===== Apagar memória ===== */
document.getElementById('forgetBtn').addEventListener('click', () => {
  MemoryService.forgetAll();
  document.getElementById('conversation').innerHTML = '';
  renderItems();
});

/* ===== Seletor de microfone ===== */
const micSelect = document.getElementById('micSelect');
async function refreshMicList() {
  const mics = await MicrophoneService.getAvailableMicrophones();
  const current = micSelect.value;
  micSelect.innerHTML = '<option value="">Padrão do sistema</option>' +
    mics.map(m => `<option value="${esc(m.deviceId)}">${esc(m.label)}</option>`).join('');
  micSelect.value = current;
}
micSelect.addEventListener('change', () => MicrophoneService.selectMicrophone(micSelect.value));
if (navigator.mediaDevices && navigator.mediaDevices.addEventListener) {
  navigator.mediaDevices.addEventListener('devicechange', refreshMicList);
}
refreshMicList();

/* ===== Diagnóstico ===== */
const diagPanel = document.getElementById('diagPanel');
document.getElementById('diagBtn').addEventListener('click', async () => {
  diagPanel.style.display = 'block';
  diagPanel.textContent = 'Executando diagnóstico...';
  const results = await MicrophoneService.runDiagnostics();
  diagPanel.innerHTML = results.map(r =>
    `<div><span class="${r.ok ? 'ok' : 'bad'}">${r.ok ? '✓' : '✗'}</span> ${esc(r.label)}${r.detail ? ` — ${esc(r.detail)}` : ''}</div>`
  ).join('');
  refreshMicList();
});

/* ===== Aviso de contexto inseguro ===== */
if (!window.isSecureContext) document.getElementById('secureWarning').classList.add('show');

/* ===== Inicialização da voz ===== */
const voiceBadge = document.getElementById('voiceBadge');
function updateVoiceBadge() {
  const v = VoiceProfile.getVoice();
  voiceBadge.textContent = v ? `Voz: ${v.name}` : 'Voz: padrão do navegador';
}
VoiceProfile.onReady(updateVoiceBadge);
VoiceProfile.init();
updateVoiceBadge();
setTimeout(updateVoiceBadge, 500);
</script>
</body>
</html>
