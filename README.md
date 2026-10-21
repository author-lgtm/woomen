# woomen
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>iPhone</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  html, body { height: 100%; background: #0e0e10; }
  body {
    display: flex; align-items: center; justify-content: center;
    font-family: -apple-system, "SF Pro Text", "Helvetica Neue", Arial, sans-serif;
    color: #000;
  }

  /* ---------- Frame HP ---------- */
  .phone {
    position: relative; overflow: hidden; background: #000;
    width: 100vw; height: 100dvh;
  }
  @media (min-width: 520px) {
    .phone { width: 390px; height: min(844px, 96dvh); border-radius: 48px;
             box-shadow: 0 0 0 10px #1c1c1f, 0 30px 80px rgba(0,0,0,.6); }
  }

  .screen { position: absolute; inset: 0; transition: opacity .35s ease, transform .35s ease; }
  .screen.off { opacity: 0; pointer-events: none; transform: scale(1.04); }

  /* ---------- Status bar ---------- */
  .status {
    position: absolute; top: 0; left: 0; right: 0; z-index: 20; height: 50px;
    padding: 14px 28px 0; display: flex; justify-content: space-between; align-items: flex-start;
    font-weight: 600; font-size: 15px; pointer-events: none;
  }
  .status .icons { display: flex; gap: 6px; align-items: center; }
  .light .status { color: #fff; }
  .dark  .status { color: #000; }
  .status svg { fill: currentColor; }

  .home-ind {
    position: absolute; bottom: 8px; left: 50%; transform: translateX(-50%);
    width: 134px; height: 5px; border-radius: 3px; z-index: 30; pointer-events: none;
  }
  .light .home-ind { background: rgba(255,255,255,.9); }
  .dark  .home-ind { background: rgba(0,0,0,.85); }

  /* ---------- Lock screen ---------- */
  #lock { background: #222 center/cover no-repeat; color: #fff; }
  #lock::before {
    content: ""; position: absolute; inset: 0;
    background: linear-gradient(rgba(0,0,0,.25), rgba(0,0,0,.05) 40%, rgba(0,0,0,.45));
  }
  .lock-top { position: absolute; top: 92px; left: 0; right: 0; text-align: center; }
  .lock-date { font-size: 20px; font-weight: 500; opacity: .92; }
  .lock-time { font-size: 92px; font-weight: 600; letter-spacing: -2px; line-height: 1.05; margin-top: 2px; }

  .notif {
    position: absolute; left: 14px; right: 14px; bottom: 150px;
    background: rgba(40,40,44,.55); backdrop-filter: blur(24px); -webkit-backdrop-filter: blur(24px);
    border-radius: 22px; padding: 12px 14px; display: flex; gap: 11px; align-items: center;
  }
  .notif .ic { width: 38px; height: 38px; border-radius: 9px; background: #34c759; flex: none;
               display: grid; place-items: center; }
  .notif .ic svg { width: 22px; height: 22px; fill: #fff; }
  .notif b { display: block; font-size: 15px; font-weight: 600; }
  .notif span { display: block; font-size: 14px; opacity: .9; margin-top: 1px;
                display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }
  .notif small { margin-left: auto; align-self: flex-start; font-size: 12px; opacity: .7; }

  .unlock-hint {
    position: absolute; bottom: 34px; left: 0; right: 0; text-align: center;
    font-size: 15px; font-weight: 500; opacity: .85; animation: pulse 2.4s ease-in-out infinite;
  }
  @keyframes pulse { 50% { opacity: .35; } }
  @media (prefers-reduced-motion: reduce) { .unlock-hint { animation: none; } .screen { transition: none; } }

  /* ---------- Keypad ---------- */
  #pass {
    background: rgba(18,18,22,.55); backdrop-filter: blur(30px); -webkit-backdrop-filter: blur(30px);
    color: #fff; display: flex; flex-direction: column; align-items: center; padding-top: 110px; z-index: 15;
  }
  #pass h2 { font-size: 19px; font-weight: 500; }
  .dots { display: flex; gap: 20px; margin: 26px 0 40px; }
  .dots i { width: 13px; height: 13px; border-radius: 50%; border: 1.5px solid #fff; transition: background .1s; }
  .dots i.on { background: #fff; }
  .dots.shake { animation: shake .4s; }
  @keyframes shake { 20%,60% { transform: translateX(-10px); } 40%,80% { transform: translateX(10px); } }
  .pad { display: grid; grid-template-columns: repeat(3, 78px); gap: 14px 22px; }
  .key {
    width: 78px; height: 78px; border-radius: 50%; border: 0; color: #fff; cursor: pointer;
    background: rgba(255,255,255,.16); font-size: 33px; font-weight: 400; line-height: 1;
    display: flex; flex-direction: column; align-items: center; justify-content: center; font-family: inherit;
  }
  .key small { font-size: 10px; letter-spacing: 2px; font-weight: 600; margin-top: 2px; height: 11px; }
  .key:active { background: rgba(255,255,255,.5); }
  .key.blank { visibility: hidden; }
  .pass-cancel {
    position: absolute; right: 36px; bottom: 54px; background: none; border: 0;
    color: #fff; font-size: 17px; font-family: inherit; cursor: pointer;
  }

  /* ---------- Chat ---------- */
  #chat { background: #fff; }
  .chat-head {
    position: absolute; top: 0; left: 0; right: 0; z-index: 10; padding-top: 50px; height: 124px;
    background: rgba(247,247,249,.88); backdrop-filter: blur(20px); -webkit-backdrop-filter: blur(20px);
    border-bottom: .5px solid rgba(0,0,0,.18); display: flex; flex-direction: column; align-items: center;
  }
  .back { position: absolute; left: 10px; top: 56px; border: 0; background: none; color: #007aff;
          font-size: 17px; display: flex; align-items: center; cursor: pointer; font-family: inherit; }
  .back svg { width: 14px; height: 22px; fill: none; stroke: #007aff; stroke-width: 3; stroke-linecap: round; stroke-linejoin: round; }
  .avatar {
    width: 46px; height: 46px; border-radius: 50%; background: #a5a5aa center/cover no-repeat;
    color: #fff; font-size: 20px; font-weight: 600; display: grid; place-items: center; margin-top: -2px;
  }
  .chat-head .cname { font-size: 11.5px; margin-top: 4px; color: #111; display: flex; align-items: center; gap: 3px; }
  .chat-head .cname svg { width: 5px; height: 9px; fill: none; stroke: #999; stroke-width: 2.4; stroke-linecap: round; stroke-linejoin: round; }

  .msgs {
    position: absolute; inset: 124px 0 84px; overflow-y: auto; padding: 10px 14px 12px;
    display: flex; flex-direction: column; gap: 3px; scroll-behavior: smooth;
  }
  .stamp { text-align: center; font-size: 11.5px; color: #8a8a8e; margin: 10px 0 8px; }
  .stamp b { font-weight: 600; }
  .row { display: flex; flex-direction: column; }
  .row.me { align-items: flex-end; }
  .row.them { align-items: flex-start; }
  .row.gap { margin-top: 8px; }
  .bubble {
    max-width: 76%; padding: 7px 13px 8px; border-radius: 18px; font-size: 17px; line-height: 1.25;
    word-wrap: break-word; white-space: pre-wrap;
    animation: pop .22s ease-out;
  }
  @keyframes pop { from { transform: scale(.85); opacity: 0; } }
  .them .bubble { background: #e9e9eb; color: #000; border-bottom-left-radius: 5px; }
  .me .bubble { background: var(--mine, #007aff); color: #fff; border-bottom-right-radius: 5px; }
  .bubble.img { padding: 0; background: none !important; overflow: hidden; }
  .bubble.img img { display: block; max-width: 100%; width: 230px; border-radius: 18px; cursor: zoom-in; }
  .bubble.img + .bubble { margin-top: 3px; }
  .caption { font-size: 11px; color: #8a8a8e; margin: 2px 4px 0; font-weight: 500; }

  .typing { align-self: flex-start; background: #e9e9eb; border-radius: 18px; padding: 12px 15px; display: flex; gap: 4px; margin-top: 8px; }
  .typing i { width: 8px; height: 8px; border-radius: 50%; background: #9b9ba0; animation: blink 1.2s infinite; }
  .typing i:nth-child(2) { animation-delay: .2s; } .typing i:nth-child(3) { animation-delay: .4s; }
  @keyframes blink { 0%,60%,100% { opacity: .3; } 30% { opacity: 1; } }

  .inputbar {
    position: absolute; left: 0; right: 0; bottom: 0; height: 84px; padding: 8px 12px 0;
    display: flex; gap: 9px; align-items: flex-start; background: #fff;
  }
  .plus { width: 34px; height: 34px; border-radius: 50%; background: #e9e9eb; display: grid; place-items: center; flex: none; }
  .plus svg { width: 16px; height: 16px; stroke: #7a7a80; stroke-width: 2.4; stroke-linecap: round; }
  .field {
    flex: 1; height: 34px; border: 1px solid #c9c9ce; border-radius: 17px; padding: 0 14px;
    display: flex; align-items: center; justify-content: space-between; color: #b0b0b5; font-size: 16px;
  }
  .field svg { width: 15px; height: 18px; fill: #8e8e93; }

  /* ---------- Viewer gambar ---------- */
  #viewer { position: absolute; inset: 0; z-index: 50; background: rgba(0,0,0,.94); display: none;
            align-items: center; justify-content: center; }
  #viewer.on { display: flex; }
  #viewer img { max-width: 100%; max-height: 100%; }
</style>
</head>
<body>

<div class="phone" id="phone">

  <!-- LOCK SCREEN -->
  <section class="screen light" id="lock">
    <div class="status"><span class="clock"></span>
      <div class="icons">
        <svg width="18" height="12" viewBox="0 0 18 12"><rect x="0" y="8" width="3" height="4" rx="1"/><rect x="5" y="5" width="3" height="7" rx="1"/><rect x="10" y="2.5" width="3" height="9.5" rx="1"/><rect x="15" y="0" width="3" height="12" rx="1"/></svg>
        <svg width="26" height="12" viewBox="0 0 26 12"><rect x=".5" y=".5" width="22" height="11" rx="3.5" fill="none" stroke="currentColor" opacity=".45"/><rect x="2" y="2" width="19" height="8" rx="2"/><rect x="23.5" y="4" width="2" height="4" rx="1" opacity=".5"/></svg>
      </div>
    </div>
    <div class="lock-top"><div class="lock-date" id="lockDate"></div><div class="lock-time clock"></div></div>
    <div class="notif" id="notif">
      <div class="ic"><svg viewBox="0 0 24 24"><path d="M12 3C6.5 3 2 6.6 2 11c0 2.4 1.3 4.5 3.4 6L4.5 21l4-2c1.1.3 2.2.5 3.5.5 5.5 0 10-3.6 10-8S17.500 3 12 3z"/></svg></div>
      <div style="min-width:0"><b id="notifTitle"></b><span id="notifText"></span></div>
      <small id="notifTime"></small>
    </div>
    <div class="unlock-hint">Ketuk untuk membuka</div>
    <div class="home-ind"></div>
  </section>

  <!-- KEYPAD -->
  <section class="screen light off" id="pass">
    <div class="status"><span class="clock"></span><span></span></div>
    <h2 id="passTitle">Masukkan Kode Sandi</h2>
    <div class="dots" id="dots"></div>
    <div class="pad" id="pad"></div>
    <button class="pass-cancel" id="passCancel">Batal</button>
    <div class="home-ind"></div>
  </section>

  <!-- CHAT -->
  <section class="screen dark off" id="chat">
    <div class="status"><span class="clock"></span>
      <div class="icons">
        <svg width="18" height="12" viewBox="0 0 18 12"><rect x="0" y="8" width="3" height="4" rx="1"/><rect x="5" y="5" width="3" height="7" rx="1"/><rect x="10" y="2.5" width="3" height="9.5" rx="1"/><rect x="15" y="0" width="3" height="12" rx="1"/></svg>
        <svg width="26" height="12" viewBox="0 0 26 12"><rect x=".5" y=".5" width="22" height="11" rx="3.5" fill="none" stroke="currentColor" opacity=".45"/><rect x="2" y="2" width="19" height="8" rx="2"/><rect x="23.5" y="4" width="2" height="4" rx="1" opacity=".5"/></svg>
      </div>
    </div>
    <div class="chat-head">
      <button class="back" id="back"><svg viewBox="0 0 14 22"><path d="M11 2 3 11l8 9"/></svg></button>
      <div class="avatar" id="avatar"></div>
      <div class="cname"><span id="cname"></span><svg viewBox="0 0 5 9"><path d="M1 1l3 3.500-3 3.500"/></svg></div>
    </div>
    <div class="msgs" id="msgs"></div>
    <div class="inputbar">
      <div class="plus"><svg viewBox="0 0 16 16"><path d="M8 2v12M2 8h12"/></svg></div>
      <div class="field"><span id="placeholder">iMessage</span>
        <svg viewBox="0 0 15 18"><rect x="4.500" y="0" width="6" height="11" rx="3"/><path d="M1.500 8.500a6 6 0 0 0 12 0" fill="none" stroke="#8e8e93" stroke-width="1.600"/><rect x="6.800" y="14" width="1.400" height="4"/></svg>
      </div>
    </div>
    <div class="home-ind"></div>
  </section>

  <div id="viewer"><img alt=""></div>
</div>

<script>
/* =====================================================================
   ✏️  EDIT BAGIAN INI SAJA — semua isi web ada di sini
   ===================================================================== */
const CONFIG = {

  // ---- Umum ----
  passcode: "0606",               // kode untuk membuka HP
  fixedTime: "",                  // contoh "21:41" (kosong = jam asli HP pengunjung)
  myBubbleColor: "#007aff",       // biru iMessage. Hijau SMS: "#34c759"

  // ---- Lock screen ----
  lock: {
    wallpaper: "https://images.unsplash.com/photo-1519681393784-d120267933ba?w=900", // ganti: "images/wallpaper.jpg"
    dateText: "",                 // kosong = tanggal otomatis. Contoh: "Rabu, 21 Oktober"
    notifTitle: "Nama Kontak",
    notifText: "Kamu sudah baca pesanku belum?",
    notifTime: "sekarang",
    wrongCodeText: "Kode salah, coba lagi"
  },

  // ---- Chat ----
  chat: {
    contactName: "Nama Kontak",
    contactAvatar: "",            // gambar profil, contoh "images/avatar.jpg" (kosong = huruf pertama nama)
    placeholder: "iMessage",
    stamp: "Hari ini 21.41",      // tulisan waktu di tengah atas chat (kosong = sembunyikan)
    autoPlay: true,               // true = pesan muncul satu per satu, false = langsung semua
    backReplays: true,            // tombol "<" kembali ke lock screen

    /* Daftar pesan, urut dari atas ke bawah.
       from : "them" (abu-abu, kiri)  atau  "me" (biru, kanan)
       text : isi pesan (boleh kosong kalau hanya gambar)
       image: link/gambar (opsional), contoh "images/foto1.jpg"
       wait : jeda sebelum pesan muncul, dalam milidetik (opsional)
       status: tulisan kecil di bawah pesan, contoh "Dibaca 21.42" (opsional)  */
    messages: [
      { from: "them", text: "Hai! 👋", wait: 600 },
      { from: "them", text: "Aku punya sesuatu buat kamu" },
      { from: "me",   text: "Apa tuh?", wait: 1200 },
      { from: "them", text: "Lihat foto ini", image: "https://images.unsplash.com/photo-1501785888041-af3ef285b470?w=700" },
      { from: "them", text: "Bagus kan?" },
      { from: "me",   text: "Bagus banget!! ❤️", wait: 1400, status: "Dibaca 21.42" }
    ]
  }
};
/* ===================================================================== */


/* ---------- Mesin template (tidak perlu diubah) ---------- */
const $ = (s) => document.querySelector(s);
const sleep = (ms) => new Promise(r => setTimeout(r, ms));
const lockEl = $("#lock"), passEl = $("#pass"), chatEl = $("#chat");
let playId = 0;

document.documentElement.style.setProperty("--mine", CONFIG.myBubbleColor);

/* Jam */
function tick() {
  let t = CONFIG.fixedTime;
  if (!t) {
    const d = new Date();
    t = String(d.getHours()).padStart(2, "0") + ":" + String(d.getMinutes()).padStart(2, "0");
  }
  document.querySelectorAll(".clock").forEach(e => e.textContent = t);
}
tick(); setInterval(tick, 1000);

/* Lock screen */
const L = CONFIG.lock;
lockEl.style.backgroundImage = `url("${L.wallpaper}")`;
$("#lockDate").textContent = L.dateText ||
  new Date().toLocaleDateString("id-ID", { weekday: "long", day: "numeric", month: "long" });
$("#notifTitle").textContent = L.notifTitle;
$("#notifText").textContent = L.notifText;
$("#notifTime").textContent = L.notifTime;
if (!L.notifTitle && !L.notifText) $("#notif").style.display = "none";

/* Keypad */
const letters = { 2: "ABC", 3: "DEF", 4: "GHI", 5: "JKL", 6: "MNO", 7: "PQRS", 8: "TUV", 9: "WXYZ" };
const pad = $("#pad"), dots = $("#dots");
let entered = "";

function buildDots() {
  dots.innerHTML = "";
  for (let i = 0; i < CONFIG.passcode.length; i++) dots.appendChild(document.createElement("i"));
}
function paintDots() {
  [...dots.children].forEach((d, i) => d.classList.toggle("on", i < entered.length));
}
["1","2","3","4","5","6","7","8","9","","0",""].forEach(k => {
  const b = document.createElement("button");
  b.className = "key" + (k === "" ? " blank" : "");
  b.innerHTML = k === "" ? "" : `${k}<small>${letters[k] || ""}</small>`;
  if (k !== "") b.addEventListener("click", () => press(k));
  pad.appendChild(b);
});

function press(k) {
  if (entered.length >= CONFIG.passcode.length) return;
  entered += k; paintDots();
  if (entered.length === CONFIG.passcode.length) {
    setTimeout(() => {
      if (entered === CONFIG.passcode) { unlock(); }
      else {
        $("#passTitle").textContent = L.wrongCodeText;
        dots.classList.add("shake");
        setTimeout(() => { dots.classList.remove("shake"); entered = ""; paintDots();
          $("#passTitle").textContent = "Masukkan Kode Sandi"; }, 450);
      }
    }, 140);
  }
}

lockEl.addEventListener("click", () => { entered = ""; buildDots(); paintDots(); passEl.classList.remove("off"); });
$("#passCancel").addEventListener("click", () => passEl.classList.add("off"));
window.addEventListener("keydown", (e) => {
  if (!passEl.classList.contains("off") && /^[0-9]$/.test(e.key)) press(e.key);
});

/* Buka -> chat */
function unlock() {
  passEl.classList.add("off"); lockEl.classList.add("off"); chatEl.classList.remove("off");
  playChat();
}
function relock() {
  playId++;
  chatEl.classList.add("off"); lockEl.classList.remove("off");
}

/* Chat */
const C = CONFIG.chat;
$("#cname").textContent = C.contactName;
$("#placeholder").textContent = C.placeholder;
const av = $("#avatar");
if (C.contactAvatar) av.style.backgroundImage = `url("${C.contactAvatar}")`;
else av.textContent = (C.contactName || "?").trim().charAt(0).toUpperCase();
if (C.backReplays) $("#back").addEventListener("click", relock);

const msgsEl = $("#msgs");
const scrollDown = () => { msgsEl.scrollTop = msgsEl.scrollHeight; };

function addMessage(m, prev) {
  const row = document.createElement("div");
  row.className = "row " + m.from + (prev && prev.from !== m.from ? " gap" : "");
  if (m.image) {
    const b = document.createElement("div"); b.className = "bubble img";
    const img = document.createElement("img"); img.src = m.image; img.alt = "";
    img.addEventListener("load", scrollDown);
    img.addEventListener("click", () => { $("#viewer img").src = m.image; $("#viewer").classList.add("on"); });
    b.appendChild(img); row.appendChild(b);
  }
  if (m.text) {
    const b = document.createElement("div"); b.className = "bubble"; b.textContent = m.text; row.appendChild(b);
  }
  if (m.status) {
    const c = document.createElement("div"); c.className = "caption"; c.textContent = m.status; row.appendChild(c);
  }
  msgsEl.appendChild(row); scrollDown();
}

async function playChat() {
  const id = ++playId;
  msgsEl.innerHTML = "";
  if (C.stamp) {
    const s = document.createElement("div"); s.className = "stamp";
    s.innerHTML = "<b></b>"; s.firstChild.textContent = C.stamp; msgsEl.appendChild(s);
  }
  let prev = null;
  for (const m of C.messages) {
    if (id !== playId) return;
    if (C.autoPlay) {
      const wait = m.wait ?? 900;
      if (m.from === "them") {
        const t = document.createElement("div"); t.className = "typing";
        t.innerHTML = "<i></i><i></i><i></i>"; msgsEl.appendChild(t); scrollDown();
        await sleep(Math.min(wait, 1400)); t.remove();
      } else { await sleep(wait); }
      if (id !== playId) return;
    }
    addMessage(m, prev); prev = m;
  }
}

$("#viewer").addEventListener("click", () => $("#viewer").classList.remove("on"));
buildDots();
</script>
</body>
</html>
