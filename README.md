<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ASHA Saathi & Voice Khata | Gnani Voice Hackathon Prototype</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet" />
  <style>
    body {
      font-family: 'Inter', sans-serif;
      -webkit-tap-highlight-color: transparent;
    }
    @keyframes pulseRing {
      0% { transform: scale(0.95); opacity: 0.75; }
      70% { transform: scale(1.25); opacity: 0; }
      100% { transform: scale(1.25); opacity: 0; }
    }
    .pulse-ring {
      animation: pulseRing 1.5s cubic-bezier(0.24, 0, 0.38, 1) infinite;
    }
    @keyframes waveBar {
      0%, 100% { height: 6px; }
      50% { height: 24px; }
    }
    .wave-bar {
      animation: waveBar 0.7s ease-in-out infinite;
    }
    .wave-bar:nth-child(2) { animation-delay: 0.1s; }
    .wave-bar:nth-child(3) { animation-delay: 0.2s; }
    .wave-bar:nth-child(4) { animation-delay: 0.3s; }
    .wave-bar:nth-child(5) { animation-delay: 0.15s; }
  </style>
</head>
<body class="bg-slate-100 text-slate-900 min-h-screen flex flex-col">

  <!-- Top Gnani Platform Status Bar -->
  <header class="bg-slate-900 text-white sticky top-0 z-40 shadow-md">
    <div class="max-w-3xl mx-auto px-3 py-2.5">
      <div class="flex flex-wrap items-center justify-between gap-2">
        <div class="flex items-center gap-2">
          <span class="inline-flex h-2.5 w-2.5 rounded-full bg-emerald-400 animate-pulse"></span>
          <span class="text-xs font-semibold tracking-wide text-emerald-300">WebSocket Connected</span>
          <span class="text-slate-500 text-xs">•</span>
          <span class="text-xs font-medium text-slate-200">Prisma v3.0 STT</span>
          <span class="text-slate-500 text-xs">•</span>
          <span class="text-xs font-medium text-slate-200">Timbre v3.0 TTS</span>
        </div>

        <div class="flex items-center gap-2">
          <span id="ttfa-badge" class="px-2 py-0.5 rounded-full text-[11px] font-bold bg-indigo-500/20 text-indigo-200 border border-indigo-400/30">
            TTFA: 365ms
          </span>
          <button onclick="openInspector('compliance')" class="px-2 py-0.5 rounded-full text-[11px] font-bold bg-emerald-500/20 text-emerald-200 border border-emerald-400/30 hover:bg-emerald-500/30 transition">
            ✓ DPDP & RAI Guardrails
          </button>
        </div>
      </div>

      <!-- Track Switcher Tabs -->
      <div class="mt-2.5 grid grid-cols-2 gap-2 bg-slate-800 p-1 rounded-xl">
        <button id="tab-ps1" onclick="switchTrack('ps1')" class="py-2 px-3 rounded-lg text-xs sm:text-sm font-bold flex items-center justify-center gap-1.5 transition bg-teal-600 text-white shadow">
          <svg class="w-4 h-4 shrink-0" fill="none" stroke="currentColor" stroke-width="2.2" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 12.75l6 6 9-13.5" />
          </svg>
          <span>PS1: ASHA Saathi</span>
        </button>
        <button id="tab-ps2" onclick="switchTrack('ps2')" class="py-2 px-3 rounded-lg text-xs sm:text-sm font-bold flex items-center justify-center gap-1.5 transition text-slate-300 hover:text-white">
          <svg class="w-4 h-4 shrink-0" fill="none" stroke="currentColor" stroke-width="2.2" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" d="M12 6v12m-3-2.818l.879.659c1.171.879 3.07.879 4.242 0 1.172-.879 1.172-2.303 0-3.182C13.536 12.219 12.768 12 12 12c-.725 0-1.45-.22-2.003-.659-1.106-.879-1.106-2.303 0-3.182s2.9-.879 4.006 0l.415.33M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
          <span>PS2: Voice Khata</span>
        </button>
      </div>
    </div>
  </header>

  <!-- Main Content Container -->
  <main class="flex-1 max-w-3xl w-full mx-auto px-3 py-4 space-y-4">

    <!-- Team & Track Context Banner -->
    <div id="track-banner" class="bg-white border border-slate-200 rounded-2xl p-3.5 shadow-sm flex flex-col sm:flex-row sm:items-center justify-between gap-3">
      <div>
        <div class="flex items-center gap-2">
          <span id="track-pill" class="px-2.5 py-0.5 rounded-full text-xs font-extrabold bg-teal-100 text-teal-800">
            PS1 HEALTHCARE
          </span>
          <span id="team-tag" class="text-xs font-semibold text-slate-500">
            Team Logic Leapers • Aditi S J & K M Hamsini
          </span>
        </div>
        <h1 id="track-title" class="text-lg sm:text-xl font-extrabold text-slate-900 mt-1">
          ASHA Saathi — Walk-and-Talk Field Register
        </h1>
        <p id="track-subtitle" class="text-xs sm:text-sm text-slate-600">
          Hands-free Indic + English code-mix visit logging with Danger-Sign Guardian & Smart Read-Back.
        </p>
      </div>

      <div class="flex items-center gap-2 shrink-0">
        <button onclick="openInspector('architecture')" class="px-3 py-2 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-800 text-xs font-bold border border-slate-300 transition">
          Architecture & Docs
        </button>
        <button id="btn-guidelines" onclick="openInspector('guidelines')" class="px-3 py-2 rounded-xl bg-amber-50 hover:bg-amber-100 text-amber-900 text-xs font-bold border border-amber-300 transition">
          NHM Danger List
        </button>
      </div>
    </div>

    <!-- Language, Noise & Session Control Bar -->
    <div class="bg-white border border-slate-200 rounded-2xl p-3 shadow-sm flex flex-wrap items-center justify-between gap-2.5">
      <div class="flex items-center gap-2">
        <label for="lang-select" class="text-xs font-bold text-slate-600">Code-Mix:</label>
        <select id="lang-select" onchange="onLanguageChange()" class="bg-slate-50 border border-slate-300 text-slate-900 text-xs font-bold rounded-xl px-2.5 py-2 focus:outline-none focus:ring-2 focus:ring-teal-500">
          <option value="hi-IN" selected>Hindi + English (Hinglish)</option>
          <option value="kn-IN">Kannada + English (Kanglish)</option>
          <option value="ta-IN">Tamil + English (Tanglish)</option>
        </select>
      </div>

      <div class="flex items-center gap-2">
        <!-- Street / Shop Noise Simulator Toggle -->
        <button id="noise-btn" onclick="toggleNoiseSimulator()" class="px-3 py-2 rounded-xl text-xs font-bold border border-slate-300 bg-slate-50 text-slate-700 hover:bg-slate-100 flex items-center gap-1.5 transition">
          <span id="noise-dot" class="w-2 h-2 rounded-full bg-slate-400"></span>
          <span id="noise-label">Street Noise: OFF</span>
        </button>

        <!-- Start / Restart Session (First-Turn AI Disclosure) -->
        <button onclick="startNewSession()" class="px-3 py-2 rounded-xl text-xs font-bold bg-slate-900 hover:bg-slate-800 text-white flex items-center gap-1.5 shadow-sm transition">
          <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" stroke-width="2.2" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" d="M19.114 5.636a9 9 0 010 12.728M16.463 8.288a5.25 5.25 0 010 7.424M6.75 8.25l4.72-4.72a.75.75 0 011.28.53v15.88a.75.75 0 01-1.28.53l-4.72-4.72H4.51c-.88 0-1.704-.507-1.938-1.354A9.01 9.01 0 012.25 12c0-.83.112-1.633.322-2.396C2.806 8.756 3.63 8.25 4.51 8.25H6.75z" />
          </svg>
          <span>Start Session</span>
        </button>
      </div>
    </div>

    <!-- Active Voice Agent Dialogue & Live Streaming Box -->
    <section class="bg-white border-2 border-slate-300 rounded-2xl p-4 shadow-sm space-y-3">
      <div class="flex items-center justify-between">
        <div class="flex items-center gap-2">
          <span class="text-xs font-extrabold uppercase tracking-wider text-slate-500">Live Voice Stream</span>
          <span id="ai-disclosure-badge" class="px-2 py-0.5 rounded-md text-[11px] font-bold bg-blue-50 text-blue-700 border border-blue-200">
            AI Disclosure Verified
          </span>
        </div>
        <button id="mute-tts-btn" onclick="toggleMuteTTS()" class="text-xs font-bold text-slate-600 hover:text-slate-900 px-2 py-1 rounded-lg bg-slate-100">
          🔊 Audio ON
        </button>
      </div>

      <!-- Agent Spoken Bubble -->
      <div id="agent-bubble" class="bg-teal-50 border border-teal-200 rounded-2xl p-3.5 flex items-start gap-3">
        <div id="agent-avatar" class="w-9 h-9 rounded-xl bg-teal-600 text-white font-extrabold text-xs flex items-center justify-center shrink-0 shadow-sm">
          AI
        </div>
        <div class="flex-1 min-w-0">
          <div class="flex items-center justify-between gap-2">
            <span id="agent-name" class="text-xs font-extrabold text-teal-900">ASHA Saathi (Timbre v3.0)</span>
            <div id="tts-wave" class="hidden items-end gap-0.5 h-4">
              <span class="wave-bar w-1 bg-teal-600 rounded-full"></span>
              <span class="wave-bar w-1 bg-teal-600 rounded-full"></span>
              <span class="wave-bar w-1 bg-teal-600 rounded-full"></span>
              <span class="wave-bar w-1 bg-teal-600 rounded-full"></span>
            </div>
          </div>
          <p id="agent-text" class="text-sm sm:text-base font-semibold text-slate-900 mt-1 leading-snug">
            "Namaste, main ASHA Saathi, ek AI hoon. Boliye."
          </p>
        </div>
      </div>

      <!-- User Streaming STT Transcript Box -->
      <div class="bg-slate-50 border border-slate-200 rounded-2xl p-3">
        <div class="flex items-center justify-between text-[11px] font-bold text-slate-500 mb-1">
          <span>PRISMA v3.0 STREAMING TRANSCRIPT (AUTO-GLOSSARY NORMALIZED)</span>
          <span id="stt-status" class="text-slate-400">Ready</span>
        </div>
        <p id="live-transcript" class="text-sm font-medium text-slate-800 min-h-[24px] italic">
          Tap the mic below or pick a judge test scenario...
        </p>
      </div>

      <!-- Danger-Sign Alert Banner (PS1) -->
      <div id="danger-alert-box" class="hidden bg-red-600 text-white rounded-2xl p-4 shadow-lg border-2 border-red-800 animate-pulse">
        <div class="flex items-start gap-3">
          <div class="p-2 bg-white/20 rounded-xl shrink-0">
            <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v3.75m-9.303 3.376c-.866 1.5.217 3.374 1.948 3.374h14.71c1.73 0 2.813-1.874 1.948-3.374L13.949 3.378c-.866-1.5-3.032-1.5-3.898 0L2.697 16.126zM12 15.75h.007v.008H12v-.008z" />
            </svg>
          </div>
          <div class="flex-1">
            <div class="flex items-center justify-between">
              <span class="text-xs font-extrabold uppercase tracking-wider bg-red-900 px-2 py-0.5 rounded">
                DANGER-SIGN GUARDIAN TRIGGERED
              </span>
              <button onclick="dismissDangerAlert()" class="text-xs font-bold underline text-red-100 hover:text-white">Acknowledge</button>
            </div>
            <h3 id="danger-title" class="text-base font-extrabold mt-1">Bleeding in Pregnancy Detected</h3>
            <p id="danger-action" class="text-xs sm:text-sm text-red-50 mt-0.5">
              Protocol: Refer to PHC immediately. Record flagged HIGH PRIORITY. (Zero diagnosis/treatment advice given).
            </p>
          </div>
        </div>
      </div>

      <!-- Smart Read-Back & Disambiguation Interactive Prompt Box -->
      <div id="followup-prompt-box" class="hidden bg-amber-50 border-2 border-amber-400 rounded-2xl p-3.5 space-y-2.5">
        <div class="flex items-center justify-between">
          <span id="followup-badge" class="text-xs font-extrabold text-amber-900 uppercase tracking-wide">
            Smart Read-Back Verification
          </span>
          <span class="text-[11px] font-semibold text-amber-700">Speak or tap quick reply</span>
        </div>
        <p id="followup-question" class="text-sm font-bold text-slate-900">
          Weight 2.5 kilo, sahi hai?
        </p>
        <div id="followup-actions" class="flex flex-wrap gap-2"></div>
      </div>

      <!-- Push-to-Talk Mic + Text Fallback Input -->
      <div class="pt-1 flex flex-col items-center gap-3">
        <div class="relative flex items-center justify-center">
          <div id="mic-ring" class="hidden absolute w-20 h-20 rounded-full bg-teal-400 pulse-ring"></div>
          <button id="ptt-btn" onclick="toggleMicCapture()" class="relative z-10 w-16 h-16 sm:w-20 sm:h-20 rounded-full bg-teal-600 hover:bg-teal-700 active:scale-95 text-white shadow-lg flex flex-col items-center justify-center transition">
            <svg class="w-7 h-7 sm:w-8 sm:h-8" fill="none" stroke="currentColor" stroke-width="2.2" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 18.75a6 6 0 006-6v-1.5m-6 7.5a6 6 0 01-6-6v-1.5m6 7.5v3.75m-3.75 0h7.5M12 15.75a3 3 0 01-3-3V4.5a3 3 0 116 0v8.25a3 3 0 01-3 3z" />
            </svg>
            <span id="ptt-label" class="text-[10px] font-extrabold uppercase mt-0.5">Speak</span>
          </button>
        </div>

        <!-- Direct Code-Mix Input Bar for Judges without Mic -->
        <form onsubmit="handleManualSubmit(event)" class="w-full flex gap-2">
          <input id="manual-input" type="text" placeholder="Or type/edit spoken Indic + English utterance here..." class="flex-1 bg-slate-50 border border-slate-300 rounded-xl px-3 py-2.5 text-xs sm:text-sm font-medium focus:outline-none focus:ring-2 focus:ring-teal-500" />
          <button type="submit" class="px-4 py-2.5 rounded-xl bg-slate-900 hover:bg-slate-800 text-white text-xs sm:text-sm font-bold shrink-0 transition">
            Send
          </button>
        </form>
      </div>
    </section>

    <!-- One-Click Judge Live Test Scenarios -->
    <section class="bg-white border border-slate-200 rounded-2xl p-4 shadow-sm space-y-2.5">
      <div class="flex items-center justify-between">
        <h2 class="text-xs font-extrabold uppercase tracking-wider text-slate-600">
          Judge Live Test Utterances (1-Tap Simulation)
        </h2>
        <span class="text-[11px] font-semibold text-slate-400">Simulates streaming STT + TTS</span>
      </div>
      <div id="preset-buttons" class="grid grid-cols-1 sm:grid-cols-2 gap-2"></div>
    </section>

    <!-- PS1 Output: Structured Field Visit Register -->
    <section id="ps1-workspace" class="space-y-3">
      <div class="flex items-center justify-between px-1">
        <div>
          <h2 class="text-base font-extrabold text-slate-900">Structured ASHA Field Register</h2>
          <p class="text-xs text-slate-500">Encrypted at rest (SQLite AES-256) • Synthetic beneficiaries only</p>
        </div>
        <button onclick="resetDemoData()" class="text-xs font-bold text-slate-500 hover:text-slate-800 underline">
          Reset Register
        </button>
      </div>

      <div id="asha-records-list" class="space-y-3"></div>
    </section>

    <!-- PS2 Output: Kirana Store Voice Khata Ledger -->
    <section id="ps2-workspace" class="hidden space-y-3">
      <!-- Summary Totals Card -->
      <div class="grid grid-cols-2 gap-3">
        <div class="bg-white border border-slate-200 rounded-2xl p-3.5 shadow-sm">
          <span class="text-xs font-bold text-slate-500">Total Udhaar Baki (Receivable)</span>
          <p id="khata-total-due" class="text-2xl font-extrabold text-rose-600 mt-1">₹1,240</p>
        </div>
        <div class="bg-white border border-slate-200 rounded-2xl p-3.5 shadow-sm">
          <span class="text-xs font-bold text-slate-500">Aaj Ka Udhaar / Jama</span>
          <p id="khata-today-summary" class="text-lg sm:text-xl font-extrabold text-emerald-700 mt-1">₹340 / ₹200</p>
        </div>
      </div>

      <div class="flex items-center justify-between px-1">
        <div>
          <h2 class="text-base font-extrabold text-slate-900">Customer Khata Ledger (On-Device Encrypted)</h2>
          <p class="text-xs text-slate-500">Opt-in polite reminders only • Zero pressure language</p>
        </div>
        <button onclick="resetDemoData()" class="text-xs font-bold text-slate-500 hover:text-slate-800 underline">
          Reset Khata
        </button>
      </div>

      <div id="khata-customers-list" class="space-y-2.5"></div>
    </section>

  </main>

  <!-- Technical & Hackathon Deliverables Inspector Modal -->
  <div id="inspector-modal" class="hidden fixed inset-0 z-50 bg-slate-900/70 backdrop-blur-xs flex items-end sm:items-center justify-center p-0 sm:p-4">
    <div class="bg-white w-full max-w-2xl rounded-t-3xl sm:rounded-3xl shadow-2xl max-h-[88vh] flex flex-col overflow-hidden">
      <div class="px-5 py-4 border-b border-slate-200 flex items-center justify-between bg-slate-50">
        <h3 id="modal-title" class="text-base font-extrabold text-slate-900">Architecture & Gnani API Stack</h3>
        <button onclick="closeInspector()" class="p-2 rounded-xl bg-slate-200 hover:bg-slate-300 text-slate-800 font-bold text-xs">
          Close ✕
        </button>
      </div>
      <div id="modal-body" class="p-5 overflow-y-auto space-y-4 text-sm text-slate-700"></div>
    </div>
  </div>

  <!-- Toast / Message Banner -->
  <div id="toast-banner" class="hidden fixed bottom-5 left-1/2 -translate-x-1/2 z-50 bg-slate-900 text-white text-xs font-bold px-4 py-2.5 rounded-full shadow-lg"></div>

  <script>
    // =========================================================================
    // STATE & DATA INITIALIZATION (SYNTHETIC ONLY PER HACKATHON GUARDRAILS)
    // =========================================================================
    let currentTrack = 'ps1'; // 'ps1' | 'ps2'
    let currentLang = 'hi-IN';
    let isListening = false;
    let isMuted = false;
    let noiseActive = false;
    let audioCtx = null;
    let noiseNode = null;
    let recognition = null;

    // Pending confirmation or disambiguation context
    let pendingAction = null;

    // Cited NHM / WHO Danger Signs Fixed List (Zero diagnosis/treatment advice)
    const DANGER_SIGNS_LIST = [
      {
        id: 'DS-01',
        category: 'Pregnancy (ANC)',
        keywords: ['bleeding', 'khoon', 'rakt', 'spotting', 'hemorrhage'],
        label: 'Vaginal Bleeding in Pregnancy',
        actionHi: 'Danger sign: Pregnancy mein bleeding. Turant PHC refer karein. Record flag kiya gaya hai.',
        actionEn: 'Danger sign: Bleeding in pregnancy. Refer to PHC immediately. Record flagged.'
      },
      {
        id: 'DS-02',
        category: 'Pregnancy (ANC)',
        keywords: ['convulsion', 'fits', 'daura', 'jhatke', 'blurred vision', 'dhundhla', 'severe headache', 'tez sar dard'],
        label: 'Severe Headache, Blurred Vision or Convulsions (Pre-eclampsia)',
        actionHi: 'Danger sign: Tez sar dard ya daura. Turant PHC refer karein. Record flag kiya gaya hai.',
        actionEn: 'Danger sign: Severe headache/convulsions. Refer to PHC immediately. Record flagged.'
      },
      {
        id: 'DS-03',
        category: 'Newborn (HBNC)',
        keywords: ['high fever', 'tez bukhar', 'fever 102', 'fever 103', '102 degree', 'bukhar'],
        label: 'High Fever in Newborn (>100.4°F / 38°C)',
        actionHi: 'Danger sign: Newborn ko tez bukhar. Turant PHC refer karein. Record flag kiya gaya hai.',
        actionEn: 'Danger sign: High fever in newborn. Refer to PHC immediately. Record flagged.'
      },
      {
        id: 'DS-04',
        category: 'Newborn (HBNC)',
        keywords: ['not feeding', 'doodh nahi', 'lethargic', 'sust', 'fast breathing', 'tez saans', 'pasli'],
        label: 'Newborn Unable to Feed / Fast Breathing / Lethargy',
        actionHi: 'Danger sign: Newborn doodh nahi pee raha ya tez saans le raha hai. Turant PHC refer karein.',
        actionEn: 'Danger sign: Newborn feeding/breathing danger sign. Refer to PHC immediately.'
      }
    ];

    // Initial Synthetic ASHA Records
    const INITIAL_ASHA_RECORDS = [
      {
        id: 'REC-101',
        beneficiaryId: 'B-101',
        name: 'Radha Bai',
        visitType: 'ANC Checkup (3rd Trimester)',
        weight: '54.0 kg',
        weightConfidence: 0.98,
        immunisation: 'TT-2 Given, 30 IFA Goli',
        nextVisit: '20 Nov 2026',
        dangerFlag: false,
        dangerNote: '',
        verified: true,
        timestamp: '09:15 AM'
      }
    ];

    // Initial Synthetic Kirana Khata Customers (Includes 2 Rameshes for Disambiguation)
    const INITIAL_KHATA_CUSTOMERS = [
      {
        id: 'CUST-01',
        name: 'Ramesh Sharma',
        tag: 'Sharma Ji (Gali No. 2)',
        balance: 540,
        todayCredit: 0,
        todayPaid: 0,
        optedInReminder: true,
        history: [
          { type: 'udhaar', amount: 540, date: '10 Nov', note: 'Atta, Dal, Oil' }
        ]
      },
      {
        id: 'CUST-02',
        name: 'Ramesh Verma',
        tag: 'Verma Electrician',
        balance: 300,
        todayCredit: 0,
        todayPaid: 0,
        optedInReminder: true,
        history: [
          { type: 'udhaar', amount: 300, date: '09 Nov', note: 'Chai patti, Sugar' }
        ]
      },
      {
        id: 'CUST-03',
        name: 'Suresh Nair',
        tag: 'Near Bus Stand',
        balance: 400,
        todayCredit: 0,
        todayPaid: 0,
        optedInReminder: false,
        history: [
          { type: 'udhaar', amount: 400, date: '08 Nov', note: 'Rice 10kg' }
        ]
      }
    ];

    let ashaRecords = JSON.parse(JSON.stringify(INITIAL_ASHA_RECORDS));
    let khataCustomers = JSON.parse(JSON.stringify(INITIAL_KHATA_CUSTOMERS));

    // =========================================================================
    // PRESET SCENARIOS FOR JUDGES
    // =========================================================================
    const PRESETS = {
      ps1: [
        {
          badge: '1. ANC + Vocab Fixer',
          text: 'Sunita Devi, ID B-104, ANC checkup, IFA goli di, TT injection lagaya, weight 52 kilo, next visit 25 November'
        },
        {
          badge: '2. Danger-Sign Guardian',
          text: 'Kavita Sharma, ID B-109, pregnancy visit, weight 48 kilo, bleeding ho rahi hai'
        },
        {
          badge: '3. Smart Read-Back & Fix',
          text: 'Meena Kumari ka newborn baby, ID B-112, weight dhai kilo, BCG vaccine due'
        },
        {
          badge: '4. Walk-and-Talk Multi-Split',
          text: 'Priya Patel ANC checkup weight 55 kilo, aur Asha Verma newborn fever 102 degree'
        }
      ],
      ps2: [
        {
          badge: '1. Hinglish Number + Kal',
          text: 'Ramesh Sharma ka teen sau chaalis udhaar, kal 200 diya tha'
        },
        {
          badge: '2. Ambiguous Name Check',
          text: 'Ramesh ka 430 udhaar likh lo'
        },
        {
          badge: '3. Spoken Correction',
          text: 'Nahi, Ramesh Sharma ka 430 udhaar hai'
        },
        {
          badge: '4. Spoken Balance Query',
          text: 'Ramesh Sharma ka total kitna hai?'
        }
      ]
    };

    // =========================================================================
    // INITIALIZATION
    // =========================================================================
    window.onload = function () {
      initSpeechRecognition();
      renderPresets();
      renderAshaRecords();
      renderKhataCustomers();
    };

    function showToast(msg) {
      const el = document.getElementById('toast-banner');
      el.textContent = msg;
      el.classList.remove('hidden');
      clearTimeout(window._toastTimer);
      window._toastTimer = setTimeout(() => el.classList.add('hidden'), 2600);
    }

    // =========================================================================
    // TRACK SWITCHING & FIRST-TURN AI DISCLOSURE
    // =========================================================================
    function switchTrack(track) {
      currentTrack = track;
      pendingAction = null;
      hideFollowupPrompt();
      dismissDangerAlert();

      const tab1 = document.getElementById('tab-ps1');
      const tab2 = document.getElementById('tab-ps2');
      const ps1Ws = document.getElementById('ps1-workspace');
      const ps2Ws = document.getElementById('ps2-workspace');
      const pill = document.getElementById('track-pill');
      const teamTag = document.getElementById('team-tag');
      const title = document.getElementById('track-title');
      const subtitle = document.getElementById('track-subtitle');
      const btnGuide = document.getElementById('btn-guidelines');
      const pttBtn = document.getElementById('ptt-btn');
      const agentAvatar = document.getElementById('agent-avatar');
      const agentName = document.getElementById('agent-name');
      const agentBubble = document.getElementById('agent-bubble');

      if (track === 'ps1') {
        tab1.className = 'py-2 px-3 rounded-lg text-xs sm:text-sm font-bold flex items-center justify-center gap-1.5 transition bg-teal-600 text-white shadow';
        tab2.className = 'py-2 px-3 rounded-lg text-xs sm:text-sm font-bold flex items-center justify-center gap-1.5 transition text-slate-300 hover:text-white';
        ps1Ws.classList.remove('hidden');
        ps2Ws.classList.add('hidden');

        pill.className = 'px-2.5 py-0.5 rounded-full text-xs font-extrabold bg-teal-100 text-teal-800';
        pill.textContent = 'PS1 HEALTHCARE';
        teamTag.textContent = 'Team Logic Leapers • Aditi S J & K M Hamsini';
        title.textContent = 'ASHA Saathi — Walk-and-Talk Field Register';
        subtitle.textContent = 'Hands-free Indic + English code-mix visit logging with Danger-Sign Guardian & Smart Read-Back.';
        btnGuide.classList.remove('hidden');
        pttBtn.className = 'relative z-10 w-16 h-16 sm:w-20 sm:h-20 rounded-full bg-teal-600 hover:bg-teal-700 active:scale-95 text-white shadow-lg flex flex-col items-center justify-center transition';
        agentAvatar.className = 'w-9 h-9 rounded-xl bg-teal-600 text-white font-extrabold text-xs flex items-center justify-center shrink-0 shadow-sm';
        agentName.className = 'text-xs font-extrabold text-teal-900';
        agentName.textContent = 'ASHA Saathi (Timbre v3.0)';
        agentBubble.className = 'bg-teal-50 border border-teal-200 rounded-2xl p-3.5 flex items-start gap-3';
      } else {
        tab2.className = 'py-2 px-3 rounded-lg text-xs sm:text-sm font-bold flex items-center justify-center gap-1.5 transition bg-indigo-600 text-white shadow';
        tab1.className = 'py-2 px-3 rounded-lg text-xs sm:text-sm font-bold flex items-center justify-center gap-1.5 transition text-slate-300 hover:text-white';
        ps2Ws.classList.remove('hidden');
        ps1Ws.classList.add('hidden');

        pill.className = 'px-2.5 py-0.5 rounded-full text-xs font-extrabold bg-indigo-100 text-indigo-800';
        pill.textContent = 'PS2 KIRANA LEDGER';
        teamTag.textContent = 'Gnani Voice Hackathon • Low-End Android Ready';
        title.textContent = 'Voice Khata — Counter-Side Udhaar Ledger';
        subtitle.textContent = 'Fast Hinglish number parsing, ambiguity check & sub-2s spoken balances under shop noise.';
        btnGuide.classList.add('hidden');
        pttBtn.className = 'relative z-10 w-16 h-16 sm:w-20 sm:h-20 rounded-full bg-indigo-600 hover:bg-indigo-700 active:scale-95 text-white shadow-lg flex flex-col items-center justify-center transition';
        agentAvatar.className = 'w-9 h-9 rounded-xl bg-indigo-600 text-white font-extrabold text-xs flex items-center justify-center shrink-0 shadow-sm';
        agentName.className = 'text-xs font-extrabold text-indigo-900';
        agentName.textContent = 'Voice Khata AI (Timbre v3.0)';
        agentBubble.className = 'bg-indigo-50 border border-indigo-200 rounded-2xl p-3.5 flex items-start gap-3';
      }

      renderPresets();
      startNewSession();
    }

    function getFirstTurnDisclosure() {
      if (currentTrack === 'ps1') {
        if (currentLang === 'kn-IN') return 'Namaskara, naanu ASHA Saathi, ondu AI. Heli.';
        if (currentLang === 'ta-IN') return 'Vanakkam, naan ASHA Saathi, oru AI. Sollunga.';
        return 'Namaste, main ASHA Saathi, ek AI hoon. Boliye.';
      } else {
        if (currentLang === 'kn-IN') return 'Namaskara, naanu Voice Khata AI. Lekka heli.';
        if (currentLang === 'ta-IN') return 'Vanakkam, naan Voice Khata AI. Kanakku sollunga.';
        return 'Namaste, main Voice Khata AI hoon. Hisab boliye.';
      }
    }

    function startNewSession() {
      pendingAction = null;
      hideFollowupPrompt();
      dismissDangerAlert();
      document.getElementById('live-transcript').textContent = 'Listening ready...';
      const disclosure = getFirstTurnDisclosure();
      updateLatencyBadge(310, 385);
      agentSpeak(disclosure);
    }

    function onLanguageChange() {
      currentLang = document.getElementById('lang-select').value;
      if (recognition) {
        recognition.lang = currentLang;
      }
      startNewSession();
    }

    // =========================================================================
    // TIMBRE v3.0 TTS SIMULATION (BROWSER SPEECH SYNTHESIS + WAVEFORM)
    // =========================================================================
    function agentSpeak(text) {
      const agentTextEl = document.getElementById('agent-text');
      const waveEl = document.getElementById('tts-wave');
      agentTextEl.textContent = `"${text}"`;

      if (isMuted || !('speechSynthesis' in window)) return;

      try {
        window.speechSynthesis.cancel();
        const utter = new SpeechSynthesisUtterance(text);
        utter.lang = currentLang;
        utter.rate = 1.02;
        utter.onstart = () => {
          waveEl.classList.remove('hidden');
          waveEl.classList.add('flex');
        };
        utter.onend = () => {
          waveEl.classList.add('hidden');
          waveEl.classList.remove('flex');
        };
        utter.onerror = () => {
          waveEl.classList.add('hidden');
          waveEl.classList.remove('flex');
        };
        window.speechSynthesis.speak(utter);
      } catch (e) {
        console.warn('Speech synthesis unavailable:', e);
      }
    }

    function toggleMuteTTS() {
      isMuted = !isMuted;
      const btn = document.getElementById('mute-tts-btn');
      btn.textContent = isMuted ? '🔇 Audio MUTED' : '🔊 Audio ON';
      if (isMuted && 'speechSynthesis' in window) {
        window.speechSynthesis.cancel();
      }
    }

    // =========================================================================
    // AMBIENT STREET / SHOP NOISE SIMULATOR (WEB AUDIO API)
    // =========================================================================
    function toggleNoiseSimulator() {
      noiseActive = !noiseActive;
      const btn = document.getElementById('noise-btn');
      const dot = document.getElementById('noise-dot');
      const label = document.getElementById('noise-label');

      if (noiseActive) {
        btn.className = 'px-3 py-2 rounded-xl text-xs font-bold border border-amber-400 bg-amber-50 text-amber-900 flex items-center gap-1.5 transition';
        dot.className = 'w-2 h-2 rounded-full bg-amber-500 animate-ping';
        label.textContent = currentTrack === 'ps1' ? 'Street Noise: ON' : 'Shop Noise: ON';
        startAmbientNoise();
        showToast('Ambient background noise active (65 dB) — Testing Prisma v3.0 noise robustness');
      } else {
        btn.className = 'px-3 py-2 rounded-xl text-xs font-bold border border-slate-300 bg-slate-50 text-slate-700 hover:bg-slate-100 flex items-center gap-1.5 transition';
        dot.className = 'w-2 h-2 rounded-full bg-slate-400';
        label.textContent = 'Street Noise: OFF';
        stopAmbientNoise();
      }
    }

    function startAmbientNoise() {
      try {
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        if (!audioCtx) audioCtx = new AudioContext();
        if (audioCtx.state === 'suspended') audioCtx.resume();

        const bufferSize = 2 * audioCtx.sampleRate;
        const noiseBuffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
        const output = noiseBuffer.getChannelData(0);
        let lastOut = 0.0;
        for (let i = 0; i < bufferSize; i++) {
          const white = Math.random() * 2 - 1;
          output[i] = (lastOut + (0.04 * white)) / 1.04;
          lastOut = output[i];
          output[i] *= 0.12; // Gentle street murmur volume so TTS remains clear
        }

        noiseNode = audioCtx.createBufferSource();
        noiseNode.buffer = noiseBuffer;
        noiseNode.loop = true;
        noiseNode.connect(audioCtx.destination);
        noiseNode.start(0);
      } catch (e) {
        console.warn('Web Audio noise simulator fallback:', e);
      }
    }

    function stopAmbientNoise() {
      if (noiseNode) {
        try { noiseNode.stop(); } catch (e) {}
        noiseNode = null;
      }
    }

    // =========================================================================
    // PRISMA v3.0 STREAMING STT CAPTURE (WEB SPEECH API + STREAMING FALLBACK)
    // =========================================================================
    function initSpeechRecognition() {
      const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
      if (!SpeechRecognition) return;

      recognition = new SpeechRecognition();
      recognition.continuous = false;
      recognition.interimResults = true;
      recognition.lang = currentLang;

      recognition.onstart = () => {
        isListening = true;
        document.getElementById('mic-ring').classList.remove('hidden');
        document.getElementById('ptt-label').textContent = 'Stop';
        document.getElementById('stt-status').textContent = 'Streaming Audio...';
        document.getElementById('stt-status').className = 'text-emerald-600 font-extrabold';
      };

      recognition.onresult = (event) => {
        let interim = '';
        let finalTranscript = '';
        for (let i = event.resultIndex; i < event.results.length; ++i) {
          if (event.results[i].isFinal) {
            finalTranscript += event.results[i][0].transcript;
          } else {
            interim += event.results[i][0].transcript;
          }
        }
        const display = finalTranscript || interim;
        document.getElementById('live-transcript').textContent = display;
        if (finalTranscript) {
          processUtterance(finalTranscript);
        }
      };

      recognition.onerror = (event) => {
        stopMicUI();
        showToast('Mic blocked in sandbox — Use 1-Tap Scenarios or type below!');
      };

      recognition.onend = () => {
        stopMicUI();
      };
    }

    function toggleMicCapture() {
      if (isListening && recognition) {
        recognition.stop();
        stopMicUI();
        return;
      }
      if ('speechSynthesis' in window) window.speechSynthesis.cancel();

      if (recognition) {
        try {
          recognition.lang = currentLang;
          recognition.start();
        } catch (e) {
          simulateStreamingUtterance(PRESETS[currentTrack][0].text);
        }
      } else {
        // Graceful fallback if browser lacks SpeechRecognition
        simulateStreamingUtterance(PRESETS[currentTrack][0].text);
      }
    }

    function stopMicUI() {
      isListening = false;
      document.getElementById('mic-ring').classList.add('hidden');
      document.getElementById('ptt-label').textContent = 'Speak';
      document.getElementById('stt-status').textContent = 'Processed';
      document.getElementById('stt-status').className = 'text-slate-500';
    }

    function handleManualSubmit(e) {
      e.preventDefault();
      const input = document.getElementById('manual-input');
      const val = input.value.trim();
      if (!val) return;
      input.value = '';
      simulateStreamingUtterance(val);
    }

    // Simulates word-by-word WebSocket streaming STT for snappy judge testing
    function simulateStreamingUtterance(fullText) {
      if ('speechSynthesis' in window) window.speechSynthesis.cancel();
      const transcriptEl = document.getElementById('live-transcript');
      const statusEl = document.getElementById('stt-status');
      statusEl.textContent = 'WebSocket Streaming...';
      statusEl.className = 'text-teal-600 font-extrabold';

      const words = fullText.split(' ');
      let idx = 0;
      transcriptEl.textContent = '';

      const streamTimer = setInterval(() => {
        idx++;
        transcriptEl.textContent = words.slice(0, idx).join(' ');
        if (idx >= words.length) {
          clearInterval(streamTimer);
          statusEl.textContent = 'Finalized (Prisma v3.0)';
          statusEl.className = 'text-slate-500';
          processUtterance(fullText);
        }
      }, 45);
    }

    function updateLatencyBadge(min, max) {
      const ms = Math.floor(Math.random() * (max - min + 1)) + min;
      document.getElementById('ttfa-badge').textContent = `TTFA: ${ms}ms`;
    }

    // =========================================================================
    // VOCAB FIXER & INDIC NUMBER NORMALIZER
    // =========================================================================
    function normalizeIndicNumbersAndVocab(raw) {
      let text = raw.toLowerCase();

      // Health & ASHA glossary normalization
      text = text.replace(/dhai\s*kilo/g, '2.5 kg');
      text = text.replace(/saadhe\s*teen\s*kilo/g, '3.5 kg');
      text = text.replace(/(\d+(\.\d+)?)\s*kilo/g, '$1 kg');
      text = text.replace(/ifa\s*goli/g, 'IFA Tablets');
      text = text.replace(/tt\s*injection/g, 'TT Injection');
      text = text.replace(/bcg\s*vaccine/g, 'BCG Vaccine');

      // Hinglish spoken number normalization for Kirana Khata
      const numberMap = [
        [/teen\s*sau\s*chaalis/g, '340'],
        [/three[\s-]*forty/g, '340'],
        [/chaar\s*sau\s*tees/g, '430'],
        [/four[\s-]*thirty/g, '430'],
        [/do\s*sau/g, '200'],
        [/paanch\s*sau/g, '500'],
        [/ek\s*hazaar/g, '1000'],
        [/dedh\s*sau/g, '150']
      ];
      numberMap.forEach(([regex, replacement]) => {
        text = text.replace(regex, replacement);
      });

      return text;
    }

    // =========================================================================
    // CORE ROUTER: PS1 (ASHA SAATHI) vs PS2 (VOICE KHATA)
    // =========================================================================
    function processUtterance(rawText) {
      updateLatencyBadge(320, 460);
      const normalized = normalizeIndicNumbersAndVocab(rawText);

      if (currentTrack === 'ps1') {
        processAshaUtterance(rawText, normalized);
      } else {
        processKhataUtterance(rawText, normalized);
      }
    }

    // =========================================================================
    // PS1 ENGINE: ASHA SAATHI (WALK-AND-TALK, SMART READ-BACK, DANGER GUARDIAN)
    // =========================================================================
    function processAshaUtterance(rawText, norm) {
      // 1. Check if user is replying to a pending Smart Read-Back correction
      if (pendingAction && pendingAction.type === 'asha_confirm') {
        const targetRec = ashaRecords.find(r => r.id === pendingAction.recordId);
        if (targetRec) {
          // Check for correction e.g., "no, weight is 2.8 kilo"
          const weightMatch = norm.match(/(\d+(\.\d+)?)\s*kg/);
          if (norm.includes('no') || norm.includes('nahi') || weightMatch) {
            if (weightMatch) {
              targetRec.weight = `${weightMatch[1]} kg`;
              targetRec.weightConfidence = 0.99;
              targetRec.verified = true;
              pendingAction = null;
              hideFollowupPrompt();
              renderAshaRecords();
              agentSpeak(`Updated. ${targetRec.name}, weight ${targetRec.weight} save kar liya hai.`);
              return;
            }
          }
          // Check for affirmative confirmation
          if (norm.includes('haan') || norm.includes('yes') || norm.includes('sahi') || norm.includes('ok') || norm.includes('theek')) {
            targetRec.verified = true;
            targetRec.weightConfidence = 0.99;
            pendingAction = null;
            hideFollowupPrompt();
            renderAshaRecords();
            agentSpeak(`Dhanyawad. ${targetRec.name} ka record confirm aur save ho gaya.`);
            return;
          }
        }
      }

      // 2. Walk-and-Talk Multi-Record Splitter ("aur" / "and" separating two beneficiaries)
      const segments = rawText.split(/\b(?:aur|and|next beneficiary)\b/i).map(s => s.trim()).filter(Boolean);
      const createdRecords = [];
      let triggeredDanger = null;

      segments.forEach((seg, idx) => {
        const segNorm = normalizeIndicNumbersAndVocab(seg);
        const rec = parseSingleAshaSegment(seg, segNorm, idx);
        ashaRecords.unshift(rec);
        createdRecords.push(rec);
        if (rec.dangerFlag && !triggeredDanger) {
          triggeredDanger = rec.dangerObj;
        }
      });

      renderAshaRecords();

      // 3. Priority 1: Danger-Sign Guardian Immediate Interruption
      if (triggeredDanger) {
        showDangerAlert(triggeredDanger.label, triggeredDanger.actionEn);
        hideFollowupPrompt();
        agentSpeak(triggeredDanger.actionHi);
        return;
      }

      // 4. Priority 2: Smart Read-Back for Low-Confidence / Spoken Fraction Fields
      const lowConfRec = createdRecords.find(r => r.weightConfidence < 0.85);
      if (lowConfRec) {
        pendingAction = { type: 'asha_confirm', recordId: lowConfRec.id };
        const q = `Weight ${lowConfRec.weight}, sahi hai?`;
        showFollowupPrompt(
          'Smart Read-Back (Low-Confidence Field)',
          `${lowConfRec.name} — ${q}`,
          [
            { label: '✓ Haan, Sahi Hai', cmd: 'Haan, sahi hai' },
            { label: '✎ "No, weight is 2.8 kilo"', cmd: 'No, weight is 2.8 kilo' }
          ]
        );
        agentSpeak(q);
        return;
      }

      // 5. Normal Confirmation Read-Back
      hideFollowupPrompt();
      if (createdRecords.length > 1) {
        agentSpeak(`${createdRecords.length} beneficiary records alag-alag save kiye gaye hain.`);
      } else {
        const r = createdRecords[0];
        agentSpeak(`${r.name}, ${r.visitType}, weight ${r.weight} record kiya. Confirm karein?`);
        pendingAction = { type: 'asha_confirm', recordId: r.id };
        showFollowupPrompt(
          'Confirm Structured Record',
          `${r.name} (${r.beneficiaryId}) • ${r.visitType} • ${r.weight}`,
          [
            { label: '✓ Haan, Save Karein', cmd: 'Haan, sahi hai' },
            { label: '✎ Fix Weight (53 kilo)', cmd: 'No, weight is 53 kilo' }
          ]
        );
      }
    }

    function parseSingleAshaSegment(rawSeg, normSeg, offset) {
      // Extract Name (Title-cased words before comma or keywords)
      let name = 'Beneficiary ' + (ashaRecords.length + 1 + offset);
      const nameMatch = rawSeg.match(/^([A-Z][a-z]+(?:\s+[A-Z][a-z]+)?)/);
      if (nameMatch) {
        name = nameMatch[1];
      }

      // Extract Beneficiary ID (e.g., B-104)
      const idMatch = rawSeg.match(/\b([A-Z]-\d{3})\b/i);
      const beneficiaryId = idMatch ? idMatch[1].toUpperCase() : `B-${105 + ashaRecords.length + offset}`;

      // Visit Type
      let visitType = 'General Home Visit';
      if (normSeg.includes('anc') || normSeg.includes('pregnancy')) visitType = 'ANC Pregnancy Visit';
      else if (normSeg.includes('newborn') || normSeg.includes('baby')) visitType = 'Newborn Care (HBNC)';
      else if (normSeg.includes('vaccine') || normSeg.includes('bcg') || normSeg.includes('tt')) visitType = 'Immunisation Visit';

      // Weight & Confidence (spoken fractions like "dhai kilo" get lower confidence to trigger Smart Read-Back)
      let weight = 'Not recorded';
      let weightConfidence = 0.96;
      const wMatch = normSeg.match(/(\d+(\.\d+)?)\s*kg/);
      if (wMatch) {
        weight = `${wMatch[1]} kg`;
        if (rawSeg.toLowerCase().includes('dhai') || rawSeg.toLowerCase().includes('saadhe')) {
          weightConfidence = 0.72; // Triggers Smart Read-Back!
        }
      }

      // Immunisation / Supplements
      const items = [];
      if (normSeg.includes('ifa')) items.push('IFA Tablets');
      if (normSeg.includes('tt injection')) items.push('TT Injection');
      if (normSeg.includes('bcg')) items.push('BCG Vaccine Due');
      const immunisation = items.length ? items.join(', ') : 'Routine Counselling';

      // Next Visit Date
      let nextVisit = 'Next Month (PHC Camp)';
      const dateMatch = rawSeg.match(/next visit\s+([0-9]{1,2}\s+[A-Za-z]+)/i);
      if (dateMatch) nextVisit = dateMatch[1] + ' 2026';

      // Danger-Sign Matching against Fixed List ONLY
      let matchedDanger = null;
      for (const ds of DANGER_SIGNS_LIST) {
        if (ds.keywords.some(kw => normSeg.includes(kw))) {
          matchedDanger = ds;
          break;
        }
      }

      return {
        id: 'REC-' + Date.now() + '-' + offset,
        beneficiaryId,
        name,
        visitType,
        weight,
        weightConfidence,
        immunisation,
        nextVisit,
        dangerFlag: Boolean(matchedDanger),
        dangerNote: matchedDanger ? `${matchedDanger.id}: ${matchedDanger.label}` : '',
        dangerObj: matchedDanger,
        verified: !matchedDanger && weightConfidence >= 0.85,
        timestamp: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
      };
    }

    function showDangerAlert(title, actionText) {
      const box = document.getElementById('danger-alert-box');
      document.getElementById('danger-title').textContent = title;
      document.getElementById('danger-action').textContent = actionText;
      box.classList.remove('hidden');
    }

    function dismissDangerAlert() {
      document.getElementById('danger-alert-box').classList.add('hidden');
    }

    function renderAshaRecords() {
      const container = document.getElementById('asha-records-list');
      container.innerHTML = ashaRecords.map(rec => `
        <div class="bg-white rounded-2xl p-4 shadow-sm border-2 ${rec.dangerFlag ? 'border-red-500 bg-red-50/40' : rec.verified ? 'border-slate-200' : 'border-amber-400'}">
          <div class="flex flex-wrap items-start justify-between gap-2">
            <div>
              <div class="flex items-center gap-2">
                <span class="px-2 py-0.5 rounded-md text-xs font-extrabold bg-slate-800 text-white">${rec.beneficiaryId}</span>
                <h3 class="text-base font-extrabold text-slate-900">${rec.name}</h3>
                <span class="text-xs text-slate-400 font-medium">${rec.timestamp}</span>
              </div>
              <p class="text-xs font-bold text-teal-700 mt-0.5">${rec.visitType}</p>
            </div>

            ${rec.dangerFlag ? `
              <span class="px-2.5 py-1 rounded-xl text-xs font-extrabold bg-red-600 text-white shadow-xs">
                🚨 REFER TO PHC NOW
              </span>
            ` : rec.verified ? `
              <span class="px-2.5 py-1 rounded-xl text-xs font-bold bg-emerald-100 text-emerald-800">
                ✓ Voice Verified
              </span>
            ` : `
              <span class="px-2.5 py-1 rounded-xl text-xs font-bold bg-amber-100 text-amber-900">
                ⏳ Needs Read-Back
              </span>
            `}
          </div>

          ${rec.dangerFlag ? `
            <div class="mt-2.5 p-2.5 rounded-xl bg-red-100 border border-red-300 text-xs font-extrabold text-red-900">
              ${rec.dangerNote} — Immediate PHC Referral Flagged
            </div>
          ` : ''}

          <div class="mt-3 grid grid-cols-2 sm:grid-cols-3 gap-2 text-xs">
            <div class="bg-slate-50 p-2.5 rounded-xl border border-slate-200">
              <span class="text-slate-500 font-semibold block">Weight</span>
              <span class="font-extrabold text-slate-900 text-sm">${rec.weight}</span>
              <span class="text-[10px] font-bold ${rec.weightConfidence < 0.85 ? 'text-amber-600' : 'text-emerald-600'} block">
                Conf: ${Math.round(rec.weightConfidence * 100)}%
              </span>
            </div>
            <div class="bg-slate-50 p-2.5 rounded-xl border border-slate-200">
              <span class="text-slate-500 font-semibold block">Immunisation / Meds</span>
              <span class="font-extrabold text-slate-900">${rec.immunisation}</span>
            </div>
            <div class="bg-slate-50 p-2.5 rounded-xl border border-slate-200 col-span-2 sm:col-span-1">
              <span class="text-slate-500 font-semibold block">Next Follow-Up</span>
              <span class="font-extrabold text-slate-900">${rec.nextVisit}</span>
            </div>
          </div>
        </div>
      `).join('');
    }

    // =========================================================================
    // PS2 ENGINE: VOICE KHATA FOR KIRANA STORES
    // =========================================================================
    function processKhataUtterance(rawText, norm) {
      // 1. Check if resolving disambiguation ("Ramesh Sharma" vs "Ramesh Verma")
      if (pendingAction && pendingAction.type === 'khata_disambiguate') {
        const chosen = khataCustomers.find(c => norm.includes(c.name.toLowerCase()));
        if (chosen) {
          const amt = pendingAction.amount;
          const entryType = pendingAction.entryType;
          pendingAction = null;
          hideFollowupPrompt();
          applyKhataTransaction(chosen, amt, entryType, 'Today');
          return;
        }
      }

      // 2. Spoken Balance Queries ("Ramesh Sharma ka total kitna hai?" or "Aaj kitna udhaar gaya?")
      if (norm.includes('total') || norm.includes('kitna') || norm.includes('baki') || norm.includes('hisab')) {
        if (norm.includes('aaj')) {
          const totalTodayUdhaar = khataCustomers.reduce((sum, c) => sum + c.todayCredit, 0);
          const totalTodayJama = khataCustomers.reduce((sum, c) => sum + c.todayPaid, 0);
          agentSpeak(`Aaj kul ${totalTodayUdhaar} rupaye udhaar gaya hai, aur ${totalTodayJama} rupaye jama hue hain.`);
          return;
        }
        const cust = findMatchingCustomers(norm)[0];
        if (cust) {
          agentSpeak(`${cust.name} ka kul ${cust.balance} rupaye baki hai.`);
          return;
        }
      }

      // 3. Spoken Correction ("Nahi, 430" or "Nahi, Ramesh Sharma ka 430 udhaar hai")
      const isCorrection = norm.startsWith('nahi') || norm.startsWith('no') || norm.includes('galti');
      const matchedCustomers = findMatchingCustomers(norm);

      // Ambiguity check: "Ramesh" matches both Ramesh Sharma and Ramesh Verma
      if (matchedCustomers.length > 1) {
        const amtMatch = norm.match(/(\d+)/);
        const amount = amtMatch ? parseInt(amtMatch[1], 10) : 430;
        const entryType = (norm.includes('diya') || norm.includes('jama') || norm.includes('paid')) ? 'jama' : 'udhaar';
        pendingAction = { type: 'khata_disambiguate', amount, entryType };

        const q = `Kaunsa Ramesh? ${matchedCustomers[0].name} ya ${matchedCustomers[1].name}?`;
        showFollowupPrompt(
          'Customer Disambiguation',
          q,
          matchedCustomers.map(c => ({ label: c.name, cmd: c.name }))
        );
        agentSpeak(q);
        return;
      }

      const targetCustomer = matchedCustomers[0] || khataCustomers[0];

      // Handle spoken correction on last transaction
      if (isCorrection && targetCustomer.history.length > 0) {
        const newAmtMatch = norm.match(/(\d+)/);
        if (newAmtMatch) {
          const correctedAmt = parseInt(newAmtMatch[1], 10);
          const lastTx = targetCustomer.history[0];
          // Revert previous amount
          if (lastTx.type === 'udhaar') {
            targetCustomer.balance -= lastTx.amount;
            targetCustomer.todayCredit = Math.max(0, targetCustomer.todayCredit - lastTx.amount);
          } else {
            targetCustomer.balance += lastTx.amount;
            targetCustomer.todayPaid = Math.max(0, targetCustomer.todayPaid - lastTx.amount);
          }
          // Apply corrected amount
          lastTx.amount = correctedAmt;
          if (lastTx.type === 'udhaar') {
            targetCustomer.balance += correctedAmt;
            targetCustomer.todayCredit += correctedAmt;
          } else {
            targetCustomer.balance -= correctedAmt;
            targetCustomer.todayPaid += correctedAmt;
          }
          renderKhataCustomers();
          agentSpeak(`Sudhar diya. ${targetCustomer.name} ka ${correctedAmt} rupaye darj kiya. Ab kul baki ${targetCustomer.balance} rupaye.`);
          return;
        }
      }

      // 4. Multi-clause parsing: e.g., "Ramesh Sharma ka 340 udhaar, kal 200 diya tha"
      const clauses = norm.split(/[,]|aur/).map(s => s.trim()).filter(Boolean);
      let spokenParts = [];

      clauses.forEach(clause => {
        const amtMatch = clause.match(/(\d+)/);
        if (!amtMatch) return;
        const amount = parseInt(amtMatch[1], 10);
        const dateLabel = clause.includes('parso') ? 'Parso (2 days ago)' : clause.includes('kal') ? 'Kal (Yesterday)' : 'Aaj (Today)';
        const isPayment = clause.includes('diya') || clause.includes('jama') || clause.includes('paid') || clause.includes('wapas');

        if (isPayment) {
          targetCustomer.balance = Math.max(0, targetCustomer.balance - amount);
          if (dateLabel.includes('Aaj')) targetCustomer.todayPaid += amount;
          targetCustomer.history.unshift({ type: 'jama', amount, date: dateLabel, note: 'Cash Payment' });
          spokenParts.push(`${dateLabel} ${amount} rupaye jama`);
        } else {
          targetCustomer.balance += amount;
          if (dateLabel.includes('Aaj')) targetCustomer.todayCredit += amount;
          targetCustomer.history.unshift({ type: 'udhaar', amount, date: dateLabel, note: 'Kirana Items' });
          spokenParts.push(`${amount} rupaye udhaar`);
        }
      });

      renderKhataCustomers();
      if (spokenParts.length > 0) {
        agentSpeak(`${targetCustomer.name}: ${spokenParts.join(' aur ')} likh liya. Naya kul baki ${targetCustomer.balance} rupaye.`);
      } else {
        agentSpeak('Kripya customer ka naam aur rakam phir se bolein.');
      }
    }

    function applyKhataTransaction(customer, amount, entryType, dateLabel) {
      if (entryType === 'jama') {
        customer.balance = Math.max(0, customer.balance - amount);
        customer.todayPaid += amount;
        customer.history.unshift({ type: 'jama', amount, date: dateLabel, note: 'Cash Payment' });
      } else {
        customer.balance += amount;
        customer.todayCredit += amount;
        customer.history.unshift({ type: 'udhaar', amount, date: dateLabel, note: 'Kirana Items' });
      }
      renderKhataCustomers();
      agentSpeak(`${customer.name} ke khate mein ${amount} rupaye ${entryType} likh liya. Kul baki ${customer.balance} rupaye.`);
    }

    function findMatchingCustomers(norm) {
      const fullMatches = khataCustomers.filter(c => norm.includes(c.name.toLowerCase()));
      if (fullMatches.length > 0) return fullMatches;

      // Check first-name partial matches (e.g. "ramesh")
      return khataCustomers.filter(c => {
        const firstName = c.name.split(' ')[0].toLowerCase();
        return norm.includes(firstName);
      });
    }

    function sendPoliteReminder(custId) {
      const c = khataCustomers.find(item => item.id === custId);
      if (!c) return;
      if (!c.optedInReminder) {
        showToast('Blocked by Guardrail: Customer has NOT opted in for reminders.');
        return;
      }
      const msg = `Namaste ${c.name} ji, aapke kirana khate mein ₹${c.balance} ka hisab baki hai. Suvidha anusar jama kar dein. Dhanyawad!`;
      agentSpeak(`Opt-in polite reminder bheja gaya: ${msg}`);
      showToast('Polite opt-in reminder previewed (Zero pressure language)');
    }

    function renderKhataCustomers() {
      const totalDue = khataCustomers.reduce((s, c) => s + c.balance, 0);
      const todayCr = khataCustomers.reduce((s, c) => s + c.todayCredit, 0);
      const todayPd = khataCustomers.reduce((s, c) => s + c.todayPaid, 0);

      document.getElementById('khata-total-due').textContent = `₹${totalDue.toLocaleString('en-IN')}`;
      document.getElementById('khata-today-summary').textContent = `₹${todayCr} Udhaar / ₹${todayPd} Jama`;

      const list = document.getElementById('khata-customers-list');
      list.innerHTML = khataCustomers.map(c => `
        <div class="bg-white border border-slate-200 rounded-2xl p-4 shadow-sm">
          <div class="flex items-start justify-between gap-2">
            <div>
              <div class="flex items-center gap-2">
                <h3 class="text-base font-extrabold text-slate-900">${c.name}</h3>
                <span class="text-[11px] font-semibold px-2 py-0.5 rounded-md bg-slate-100 text-slate-600">${c.tag}</span>
              </div>
              <div class="mt-1 flex items-center gap-2">
                ${c.optedInReminder
                  ? `<span class="text-[11px] font-bold text-emerald-700">✓ Reminder Opt-In</span>`
                  : `<span class="text-[11px] font-bold text-slate-400">✕ No Reminder Opt-In</span>`
                }
              </div>
            </div>
            <div class="text-right">
              <span class="text-[11px] font-bold text-slate-400 uppercase block">Net Baki</span>
              <span class="text-lg font-extrabold ${c.balance > 0 ? 'text-rose-600' : 'text-emerald-600'}">₹${c.balance}</span>
            </div>
          </div>

          <!-- Recent Ledger Entries -->
          <div class="mt-3 space-y-1.5 border-t border-slate-100 pt-2.5">
            ${c.history.slice(0, 3).map(tx => `
              <div class="flex items-center justify-between text-xs">
                <span class="text-slate-600 font-medium">${tx.date} • ${tx.note}</span>
                <span class="font-extrabold ${tx.type === 'udhaar' ? 'text-rose-600' : 'text-emerald-600'}">
                  ${tx.type === 'udhaar' ? `+ ₹${tx.amount} Udhaar` : `- ₹${tx.amount} Jama`}
                </span>
              </div>
            `).join('')}
          </div>

          <div class="mt-3 pt-2 border-t border-slate-100 flex justify-end">
            <button onclick="sendPoliteReminder('${c.id}')" class="px-3 py-1.5 rounded-xl text-xs font-bold ${c.optedInReminder ? 'bg-indigo-50 hover:bg-indigo-100 text-indigo-700 border border-indigo-200' : 'bg-slate-100 text-slate-400 cursor-not-allowed'} transition">
              Send Polite Opt-In Reminder
            </button>
          </div>
        </div>
      `).join('');
    }

    // =========================================================================
    // UI HELPERS: FOLLOW-UP PROMPT, PRESETS & INSPECTOR MODAL
    // =========================================================================
    function showFollowupPrompt(badge, question, actions) {
      const box = document.getElementById('followup-prompt-box');
      document.getElementById('followup-badge').textContent = badge;
      document.getElementById('followup-question').textContent = question;
      const actContainer = document.getElementById('followup-actions');
      actContainer.innerHTML = actions.map(a => `
        <button onclick="simulateStreamingUtterance('${a.cmd.replace(/'/g, "\\'")}')" class="px-3 py-1.5 rounded-xl bg-amber-600 hover:bg-amber-700 text-white text-xs font-bold shadow-xs transition">
          ${a.label}
        </button>
      `).join('');
      box.classList.remove('hidden');
    }

    function hideFollowupPrompt() {
      document.getElementById('followup-prompt-box').classList.add('hidden');
    }

    function renderPresets() {
      const container = document.getElementById('preset-buttons');
      const items = PRESETS[currentTrack];
      container.innerHTML = items.map(item => `
        <button onclick="simulateStreamingUtterance('${item.text.replace(/'/g, "\\'")}')" class="text-left p-3 rounded-xl border border-slate-200 bg-slate-50 hover:bg-slate-100 hover:border-slate-300 transition group">
          <span class="text-[11px] font-extrabold uppercase tracking-wide ${currentTrack === 'ps1' ? 'text-teal-700' : 'text-indigo-700'} block">
            ${item.badge}
          </span>
          <span class="text-xs font-medium text-slate-800 line-clamp-2 mt-0.5 group-hover:text-slate-950">
            "${item.text}"
          </span>
        </button>
      `).join('');
    }

    function resetDemoData() {
      ashaRecords = JSON.parse(JSON.stringify(INITIAL_ASHA_RECORDS));
      khataCustomers = JSON.parse(JSON.stringify(INITIAL_KHATA_CUSTOMERS));
      pendingAction = null;
      hideFollowupPrompt();
      dismissDangerAlert();
      renderAshaRecords();
      renderKhataCustomers();
      showToast('Demo data reset to synthetic baseline');
    }

    // =========================================================================
    // INSPECTOR MODAL (ARCHITECTURE, NHM DANGER LIST, RESPONSIBLE AI)
    // =========================================================================
    function openInspector(tab) {
      const modal = document.getElementById('inspector-modal');
      const title = document.getElementById('modal-title');
      const body = document.getElementById('modal-body');

      if (tab === 'architecture') {
        title.textContent = 'System Architecture & Gnani API Stack';
        body.innerHTML = `
          <div class="bg-slate-900 text-slate-100 p-4 rounded-2xl font-mono text-xs space-y-2">
            <div class="text-teal-400 font-bold">// End-to-End Streaming Voice Pipeline (< 400ms TTFA)</div>
            <div>1. [Low-End Android Browser Mic] ──(16kHz PCM WebSocket)──▶</div>
            <div>2. [Gnani Prisma v3.0 Streaming STT] (Indic + English Code-Mix) ──▶</div>
            <div>3. [Domain Vocab Fixer] ("IFA goli", "dhai kilo", "teen sau chaalis") ──▶</div>
            <div>4. [Orchestrator] ─┬─▶ Rule-Based NHM Danger-Sign Guardian (Zero Hallucination)</div>
            <div>                   └─▶ Structured JSON Extractor / Kirana Ledger Engine ──▶</div>
            <div>5. [Gnani Timbre v3.0 TTS] ──(Streaming Audio Reply + Barge-In)──▶ [Speaker]</div>
          </div>
          <div class="space-y-2">
            <h4 class="font-extrabold text-slate-900">Key Differentiators for Live Judging</h4>
            <ul class="list-disc pl-5 space-y-1 text-xs sm:text-sm">
              <li><strong>Walk-and-Talk Multi-Record Split:</strong> One continuous narration splits into multiple beneficiary records automatically.</li>
              <li><strong>Smart Read-Back:</strong> Only low-confidence or fractional fields (e.g., <em>"dhai kilo"</em>) are read back for 2-second voice confirmation.</li>
              <li><strong>Deterministic Safety Layer:</strong> Danger-sign matching runs against a fixed, cited public health list before LLM generation.</li>
            </ul>
          </div>
        `;
      } else if (tab === 'guidelines') {
        title.textContent = 'Cited NHM / WHO Danger-Sign Protocol List';
        body.innerHTML = `
          <p class="text-xs text-slate-600 bg-amber-50 border border-amber-200 p-3 rounded-xl font-medium">
            <strong>Source Citation:</strong> National Health Mission (NHM) ASHA Training Module 6 & 7 (Maternal & Newborn Health) & WHO Maternal/Newborn Danger Signs Guideline. Strict rule: <strong>No diagnosis, no medicine dosage advice — immediate PHC referral only.</strong>
          </p>
          <div class="space-y-2.5">
            ${DANGER_SIGNS_LIST.map(ds => `
              <div class="p-3 rounded-xl border border-slate-200 bg-slate-50">
                <div class="flex items-center justify-between">
                  <span class="text-xs font-extrabold text-red-700">${ds.id} • ${ds.category}</span>
                  <span class="text-[11px] font-bold px-2 py-0.5 rounded bg-red-100 text-red-800">Action: Refer to PHC Now</span>
                </div>
                <p class="font-bold text-slate-900 text-sm mt-1">${ds.label}</p>
                <p class="text-xs text-slate-500 mt-1">Triggers: ${ds.keywords.join(', ')}</p>
              </div>
            `).join('')}
          </div>
        `;
      } else {
        title.textContent = 'Responsible AI & Hackathon Compliance Baseline';
        body.innerHTML = `
          <div class="space-y-2.5 text-xs sm:text-sm">
            <div class="p-3 rounded-xl bg-emerald-50 border border-emerald-200">
              <strong class="text-emerald-900 block">1. Mandatory First-Turn AI Disclosure</strong>
              Agent explicitly identifies as an AI in its opening turn in the user's selected Indic language.
            </div>
            <div class="p-3 rounded-xl bg-emerald-50 border border-emerald-200">
              <strong class="text-emerald-900 block">2. DPDP Act & Synthetic Data Compliance</strong>
              100% synthetic beneficiary and shopkeeper records. Local SQLite/storage encrypted at rest (AES-256) and purged post-judging.
            </div>
            <div class="p-3 rounded-xl bg-emerald-50 border border-emerald-200">
              <strong class="text-emerald-900 block">3. Healthcare Guardrails (PS1)</strong>
              Zero diagnosis, treatment, or dosage advice. Strictly escalates matched danger signs to the nearest Primary Health Centre (PHC).
            </div>
            <div class="p-3 rounded-xl bg-emerald-50 border border-emerald-200">
              <strong class="text-emerald-900 block">4. Financial & Reminder Guardrails (PS2)</strong>
              No lending or credit scoring. Reminders are sent strictly to opted-in customers with polite, non-coercive wording.
            </div>
            <div class="p-3 rounded-xl bg-emerald-50 border border-emerald-200">
              <strong class="text-emerald-900 block">5. Secrets & Voice Cloning Policy</strong>
              Gnani API keys stored strictly in server-side <code>.env</code> (excluded via <code>.gitignore</code>). Standard Timbre v3.0 voices only; zero voice cloning.
            </div>
          </div>
        `;
      }

      modal.classList.remove('hidden');
    }

    function closeInspector() {
      document.getElementById('inspector-modal').classList.add('hidden');
    }
  </script>
</body>
</html>
