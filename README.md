    # mo<!DOCTYPE html>
<html lang="sk">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Moja Nemčina</title>
  <style>
    :root {
      --bg: #0f172a;
      --card: #1e293b;
      --card-2: #334155;
      --text: #f8fafc;
      --muted: #94a3b8;
      --accent: #38bdf8;
      --ok: #22c55e;
      --bad: #ef4444;
    }
    * { box-sizing: border-box; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; }
    body { margin: 0; background: var(--bg); color: var(--text); padding: 16px 16px 84px; }
    h1 { margin: 0 0 4px; font-size: 1.5rem; }
    .sub { color: var(--muted); font-size: 0.9rem; margin-bottom: 16px; }
    .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 14px; }
    .card { background: var(--card); border-radius: 14px; padding: 14px; margin-bottom: 12px; border: 1px solid rgba(255,255,255,0.06); }
    .stat-num { font-size: 1.4rem; font-weight: 700; color: var(--accent); }
    .stat-lbl { font-size: 0.8rem; color: var(--muted); }
    label { display: block; font-size: 0.82rem; color: var(--muted); margin-top: 8px; margin-bottom: 4px; }
    input, select, textarea, button {
      width: 100%; border-radius: 10px; border: 1px solid rgba(255,255,255,0.12);
      background: var(--card-2); color: var(--text); padding: 10px 12px; font-size: 0.95rem;
    }
    textarea { min-height: 64px; resize: vertical; }
    button { background: var(--accent); color: #06202a; font-weight: 700; border: none; margin-top: 12px; cursor: pointer; }
    button.secondary { background: var(--card-2); color: var(--text); border: 1px solid rgba(255,255,255,0.15); margin-top: 6px; }
    button.small { width: auto; padding: 6px 10px; font-size: 0.82rem; margin-top: 0; }
    .row { display: flex; justify-content: space-between; align-items: center; gap: 8px; }
    .de { font-size: 1.1rem; font-weight: 700; }
    .sk { color: #cbd5e1; margin-top: 2px; }
    .cat { display: inline-block; font-size: 0.72rem; background: rgba(56,189,248,0.16); color: var(--accent); padding: 2px 8px; border-radius: 999px; margin-top: 6px; }
    .example { font-size: 0.85rem; color: var(--muted); margin-top: 6px; border-left: 2px solid var(--accent); padding-left: 8px; }
    .nav {
      position: fixed; bottom: 0; left: 0; right: 0; background: #0b1220;
      display: grid; grid-template-columns: repeat(4, 1fr); border-top: 1px solid rgba(255,255,255,0.1);
    }
    .nav button { background: none; color: var(--muted); border-radius: 0; margin: 0; padding: 10px 4px; font-size: 0.78rem; }
    .nav button.active { color: var(--accent); font-weight: 700; }
    .hidden { display: none; }
    .msg-ok { color: var(--ok); font-weight: 700; margin-top: 8px; }
    .msg-bad { color: var(--bad); font-weight: 700; margin-top: 8px; }
  </style>
</head>
<body>
  <h1>Moja Nemčina 🇩🇪</h1>
  <div class="sub">Slovník, vety, výslovnosť a kvíz (A1–A2 + REWE)</div>

  <!-- DOMOV -->
  <section id="tab-home">
    <div class="grid-2">
      <div class="card">
        <div class="stat-num" id="todayCount">0 / 25</div>
        <div class="stat-lbl">Denný cieľ slovíčok</div>
      </div>
      <div class="card">
        <div class="stat-num" id="totalWords">0</div>
        <div class="stat-lbl">Uložených položiek</div>
      </div>
      <div class="card">
        <div class="stat-num" id="stat7">0</div>
        <div class="stat-lbl">Aktivita za 7 dní</div>
      </div>
      <div class="card">
        <div class="stat-num" id="stat30">0</div>
        <div class="stat-lbl">Aktivita za 30 dní</div>
      </div>
    </div>
    <div class="card">
      <strong>Rýchly tip</strong>
      <p class="sub" style="margin:6px 0 0;">Ťukni na 🔊 pri ktoromkoľvek nemeckom výraze a telefón ti ho prečíta po nemecky.</p>
    </div>
  </section>

  <!-- SLOVNÍK A VETY -->
  <section id="tab-list" class="hidden">
    <div class="card">
      <input type="text" id="searchInput" placeholder="Hľadať nemecky alebo slovensky..." oninput="renderList()" />
      <label for="filterCat">Kategória</label>
      <select id="filterCat" onchange="renderList()">
        <option value="ALL">Všetky kategórie</option>
        <option value="REWE">REWE</option>
        <option value="Práca">Práca</option>
        <option value="Obchod">Obchod</option>
        <option value="Domácnosť">Domácnosť</option>
        <option value="Cestovanie">Cestovanie</option>
        <option value="Bežná komunikácia">Bežná komunikácia</option>
      </select>
    </div>
    <div id="itemsContainer"></div>
  </section>

  <!-- PRIDAŤ -->
  <section id="tab-add" class="hidden">
    <div class="card">
      <strong>Pridať nové slovíčko alebo vetu</strong>
      <label for="deInput">Nemecky (slovo alebo veta)</label>
      <input type="text" id="deInput" placeholder="napr. die Palette / Ich hole die Ware" />

      <label for="skInput">Slovensky (preklad)</label>
      <input type="text" id="skInput" placeholder="napr. paleta / Beriem tovar" />

      <label for="catInput">Kategória</label>
      <select id="catInput">
        <option value="REWE">REWE</option>
        <option value="Práca">Práca</option>
        <option value="Obchod">Obchod</option>
        <option value="Domácnosť">Domácnosť</option>
        <option value="Cestovanie">Cestovanie</option>
        <option value="Bežná komunikácia">Bežná komunikácia</option>
      </select>

      <label for="exInput">Príkladová veta (voliteľné)</label>
      <textarea id="exInput" placeholder="napr. Kannst du mir bitte helfen?"></textarea>

      <button onclick="addItem()">Uložiť do slovníka</button>
      <div id="addFeedback"></div>
    </div>
  </section>

  <!-- KVÍZ -->
  <section id="tab-quiz" class="hidden">
    <div class="card">
      <div class="row">
        <strong>Precvičovanie / Kvíz</strong>
        <button class="small secondary" onclick="nextQuiz()">Nová otázka</button>
      </div>
      <div id="quizQuestion" style="font-size:1.15rem; font-weight:700; margin-top:12px;"></div>
      <div id="quizHint" class="sub" style="margin-top:4px;"></div>
      <input type="text" id="quizAnswer" placeholder="Napíš preklad..." />
      <button onclick="checkQuiz()">Skontrolovať</button>
      <button class="secondary" onclick="revealQuiz()">Ukázať odpoveď</button>
      <div id="quizResult"></div>
    </div>
  </section>

  <nav class="nav">
    <button id="btn-home" class="active" onclick="switchTab('home')">Domov</button>
    <button id="btn-list" onclick="switchTab('list')">Slovník</button>
    <button id="btn-add" onclick="switchTab('add')">Pridať</button>
    <button id="btn-quiz" onclick="switchTab('quiz')">Kvíz</button>
  </nav>

  <script>
    const STORAGE_KEY = "moja_nemcina_data_v1";
    const LOG_KEY = "moja_nemcina_logs_v1";

    const DEFAULT_ITEMS = [
      { id: 1, de: "die Schicht", sk: "zmena (v práci)", cat: "REWE", ex: "Ich habe heute Spätschicht.", learned: false },
      { id: 2, de: "das Trockensortiment", sk: "suchý sortiment", cat: "REWE", ex: "Wir arbeiten im Trockensortiment.", learned: false },
      { id: 3, de: "der Wagen / der Hubwagen", sk: "vozík / paletový vozík", cat: "REWE", ex: "Bitte bring den Wagen nach vorne.", learned: false },
      { id: 4, de: "Wann fängst du an?", sk: "Kedy začínaš?", cat: "Práca", ex: "Ich fange um 14:30 Uhr an.", learned: false },
      { id: 5, de: "Ich habe Feierabend.", sk: "Mám po práci / padla.", cat: "Práca", ex: "Endlich Feierabend!", learned: false },
      { id: 6, de: "Wie viel kostet das?", sk: "Koľko to stojí?", cat: "Obchod", ex: "Wie viel kostet das zusammen?", learned: false },
      { id: 7, de: "Ich möchte bezahlen.", sk: "Chcel by som zaplatiť.", cat: "Obchod", ex: "Kann ich mit Karte bezahlen?", learned: false },
      { id: 8, de: "Guten Morgen, wie geht es dir?", sk: "Dobré ráno, ako sa máš?", cat: "Bežná komunikácia", ex: "Mir geht es gut, danke!", learned: false }
    ];

    function loadItems() {
      const raw = localStorage.getItem(STORAGE_KEY);
      if (!raw) {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(DEFAULT_ITEMS));
        return [...DEFAULT_ITEMS];
      }
      return JSON.parse(raw);
    }

    function saveItems(items) {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(items));
    }

    function loadLogs() {
      return JSON.parse(localStorage.getItem(LOG_KEY) || "[]");
    }

    function addLog() {
      const logs = loadLogs();
      logs.push(Date.now());
      localStorage.setItem(LOG_KEY, JSON.stringify(logs));
    }

    function speakGerman(text) {
      if (!("speechSynthesis" in window)) return;
      window.speechSynthesis.cancel();
      const utter = new SpeechSynthesisUtterance(text);
      utter.lang = "de-DE";
      utter.rate = 0.95;
      window.speechSynthesis.speak(utter);
    }

    function switchTab(tab) {
      ["home", "list", "add", "quiz"].forEach(t => {
        document.getElementById("tab-" + t).classList.toggle("hidden", t !== tab);
        document.getElementById("btn-" + t).classList.toggle("active", t === tab);
      });
      if (tab === "home") renderStats();
      if (tab === "list") renderList();
      if (tab === "quiz") nextQuiz();
    }

    function renderStats() {
      const items = loadItems();
      const logs = loadLogs();
      const now = Date.now();
      const dayMs = 24 * 60 * 60 * 1000;
      const startOfToday = new Date();
      startOfToday.setHours(0, 0, 0, 0);

      const todayCount = logs.filter(ts => ts >= startOfToday.getTime()).length;
      const count7 = logs.filter(ts => ts >= now - 7 * dayMs).length;
      const count30 = logs.filter(ts => ts >= now - 30 * dayMs).length;

      document.getElementById("todayCount").textContent = `${todayCount} / 25`;
      document.getElementById("totalWords").textContent = items.length;
      document.getElementById("stat7").textContent = count7;
      document.getElementById("stat30").textContent = count30;
    }

    function addItem() {
      const de = document.getElementById("deInput").value.trim();
      const sk = document.getElementById("skInput").value.trim();
      const cat = document.getElementById("catInput").value;
      const ex = document.getElementById("exInput").value.trim();
      const fb = document.getElementById("addFeedback");

      if (!de || !sk) {
        fb.innerHTML = '<div class="msg-bad">Vyplň nemecký výraz aj slovenský preklad.</div>';
        return;
      }

      const items = loadItems();
      items.unshift({ id: Date.now(), de, sk, cat, ex, learned: false });
      saveItems(items);
      addLog();

      document.getElementById("deInput").value = "";
      document.getElementById("skInput").value = "";
      document.getElementById("exInput").value = "";
      fb.innerHTML = '<div class="msg-ok">Uložené!</div>';
      renderStats();
    }

    function deleteItem(id) {
      const items = loadItems().filter(x => x.id !== id);
      saveItems(items);
      renderList();
      renderStats();
    }

    function renderList() {
      const q = document.getElementById("searchInput").value.trim().toLowerCase();
      const cat = document.getElementById("filterCat").value;
      const container = document.getElementById("itemsContainer");
      const items = loadItems().filter(item => {
        const matchCat = cat === "ALL" || item.cat === cat;
        const matchText = !q || item.de.toLowerCase().includes(q) || item.sk.toLowerCase().includes(q);
        return matchCat && matchText;
      });

      if (!items.length) {
        container.innerHTML = '<div class="card"><span class="sub">Zatiaľ tu nie sú žiadne položky.</span></div>';
        return;
      }

      container.innerHTML = items.map(item => `
        <div class="card">
          <div class="row">
            <div class="de">${escapeHtml(item.de)}</div>
            <div style="display:flex; gap:6px;">
              <button class="small secondary" onclick="speakGerman(${JSON.stringify(item.de)})">🔊</button>
              <button class="small secondary" onclick="deleteItem(${item.id})">✕</button>
            </div>
          </div>
          <div class="sk">${escapeHtml(item.sk)}</div>
          ${item.ex ? `<div class="example">${escapeHtml(item.ex)}</div>` : ""}
          <span class="cat">${escapeHtml(item.cat)}</span>
        </div>
      `).join("");
    }

    let currentQuiz = null;
    let askGerman = true;

    function nextQuiz() {
      const items = loadItems();
      const res = document.getElementById("quizResult");
      const ans = document.getElementById("quizAnswer");
      res.innerHTML = "";
      ans.value = "";

      if (!items.length) {
        document.getElementById("quizQuestion").textContent = "Najprv pridaj slovíčka.";
        return;
      }

      currentQuiz = items[Math.floor(Math.random() * items.length)];
      askGerman = Math.random() > 0.5;
      document.getElementById("quizQuestion").textContent = askGerman ? currentQuiz.de : currentQuiz.sk;
      document.getElementById("quizHint").textContent = askGerman
        ? `Prelož do slovenčiny (${currentQuiz.cat})`
        : `Prelož do nemčiny (${currentQuiz.cat})`;
    }

    function checkQuiz() {
      if (!currentQuiz) return;
      const userVal = document.getElementById("quizAnswer").value.trim().toLowerCase();
      const target = (askGerman ? currentQuiz.sk : currentQuiz.de).toLowerCase();
      const res = document.getElementById("quizResult");

      if (!userVal) {
        res.innerHTML = '<div class="msg-bad">Napíš odpoveď.</div>';
        return;
      }

      addLog();
      if (target.includes(userVal) || userVal.includes(target)) {
        res.innerHTML = `<div class="msg-ok">Správne! (${escapeHtml(currentQuiz.de)} = ${escapeHtml(currentQuiz.sk)})</div>`;
      } else {
        res.innerHTML = `<div class="msg-bad">Nie celkom. Správne je: ${escapeHtml(currentQuiz.de)} — ${escapeHtml(currentQuiz.sk)}</div>`;
      }
      renderStats();
    }

    function revealQuiz() {
      if (!currentQuiz) return;
      document.getElementById("quizResult").innerHTML =
        `<div class="sk" style="margin-top:8px;"><strong>${escapeHtml(currentQuiz.de)}</strong> — ${escapeHtml(currentQuiz.sk)}</div>`;
    }

    function escapeHtml(str) {
      return String(str)
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;");
    }

    renderStats();
  </script>
</body>
</html>
ja-nemcina