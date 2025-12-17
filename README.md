<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Holiday Postcard</title>
  <style>
    :root{
      --bg1:#0f2f22;
      --bg2:#0b1d15;
      --paper:#f6f1e7;
      --paper2:#efe6d7;
      --ink:#123024;
      --accent:#caa34a;
      --shadow: 0 20px 60px rgba(0,0,0,.35);
      --radius: 22px;
    }

    *{ box-sizing:border-box; }
    body{
      margin:0;
      min-height:100vh;
      display:grid;
      place-items:center;
      color:var(--paper);
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Arial, "Apple Color Emoji","Segoe UI Emoji";
      background: radial-gradient(1200px 600px at 20% 10%, rgba(202,163,74,.18), transparent 60%),
                  radial-gradient(900px 500px at 90% 90%, rgba(255,255,255,.06), transparent 55%),
                  linear-gradient(160deg, var(--bg1), var(--bg2));
      padding:32px 16px;
      overflow-x:hidden;
    }

    .frame{
      width:min(900px, 100%);
      display:grid;
      gap:18px;
      justify-items:center;
    }

    .title{
      text-align:center;
      max-width:70ch;
      line-height:1.25;
    }
    .title h1{
      margin:0 0 6px 0;
      font-size:clamp(22px, 3vw, 30px);
      letter-spacing:.2px;
      font-weight:750;
    }
    .title p{
      margin:0;
      opacity:.9;
      font-size:clamp(14px, 2vw, 16px);
    }

    .envelope-wrap{
      width:min(760px, 100%);
      aspect-ratio: 16 / 10;
      position:relative;
      filter: drop-shadow(var(--shadow));
      border-radius: var(--radius);
    }

    .envelope-btn{
      all:unset;
      width:100%;
      height:100%;
      cursor:pointer;
      display:block;
      position:relative;
      border-radius: var(--radius);
      outline: none;
    }
    .envelope-btn:focus-visible{
      box-shadow: 0 0 0 4px rgba(202,163,74,.55);
    }

    /* Confetti layer */
    .confetti{
      position:absolute;
      inset:-12% -6% -8% -6%;
      pointer-events:none;
      overflow:hidden;
      border-radius: var(--radius);
      z-index: 50;
    }
    .confetti i{
      position:absolute;
      top: -18px;
      width: 10px;
      height: 16px;
      border-radius: 2px;
      opacity:0;
      transform: translateY(0) rotate(0deg);
    }
    .open .confetti i{
      opacity:1;
      animation: fall var(--dur, 1200ms) ease-out forwards;
    }
    @keyframes fall{
      0%   { transform: translate(var(--x,0px), 0px) rotate(0deg); opacity:0; }
      10%  { opacity:1; }
      100% { transform: translate(calc(var(--x,0px) * 1.2), 520px) rotate(var(--r, 260deg)); opacity:0; }
    }

    /* Card reveal */
    .card{
      position:absolute;
      inset: 10% 8% 14% 8%;
      background: linear-gradient(180deg, rgba(246,241,231,.98), rgba(239,230,215,.98));
      border-radius: 18px;
      overflow:hidden;
      transform: translateY(22%);
      opacity:0;
      transition: transform 800ms cubic-bezier(.2,.9,.2,1), opacity 500ms ease;
      z-index: 1;
    }

    .card-topbar{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:12px;
      padding:14px 16px;
      background: rgba(18,48,36,.08);
      border-bottom: 1px solid rgba(18,48,36,.12);
      color: var(--ink);
    }
    .headline{
      font-weight:900;
      letter-spacing:.3px;
      font-size: 16px;
      display:flex;
      align-items:center;
      gap:10px;
    }
    .dot{
      width:10px;height:10px;border-radius:999px;background:var(--accent);
      box-shadow: 0 0 0 4px rgba(202,163,74,.25);
    }
    .hint{
      font-size:13px;
      opacity:.85;
      font-weight:600;
    }

    .media{
      position:relative;
      width:100%;
      height: calc(100% - 52px);
      background:#0b1d15;
    }

    video{
      width:100%;
      height:100%;
      object-fit:cover;
      display:block;
    }

    /* Envelope */
    .env{
      position:absolute;
      inset:0;
      border-radius: var(--radius);
      background:
        radial-gradient(900px 420px at 30% 30%, rgba(255,255,255,.08), transparent 60%),
        linear-gradient(180deg, rgba(246,241,231,.2), rgba(0,0,0,0));
      z-index: 3;
      overflow:hidden;
    }

    .env-base{
      position:absolute;
      inset: 18% 6% 10% 6%;
      border-radius: 18px;
      background: linear-gradient(180deg, rgba(246,241,231,.96), rgba(239,230,215,.96));
      z-index: 3;
      box-shadow: inset 0 0 0 1px rgba(18,48,36,.12);
    }

    .seal{
      position:absolute;
      left:50%;
      top:62%;
      transform: translate(-50%,-50%);
      width:86px;height:86px;border-radius:999px;
      background: radial-gradient(circle at 30% 30%, rgba(255,255,255,.35), rgba(255,255,255,0) 55%),
                  radial-gradient(circle at 70% 70%, rgba(0,0,0,.08), rgba(0,0,0,0) 60%),
                  linear-gradient(180deg, rgba(202,163,74,.95), rgba(154,118,37,.95));
      box-shadow: 0 16px 40px rgba(0,0,0,.28);
      display:grid;
      place-items:center;
      color:#1b1206;
      font-weight:900;
      letter-spacing:.8px;
      z-index: 6;
      transition: transform 600ms ease, opacity 400ms ease;
    }
    .seal span{ font-size:13px; opacity:.9; }

    .flap{
      position:absolute;
      left:6%;
      right:6%;
      top:18%;
      height:54%;
      background: linear-gradient(180deg, rgba(246,241,231,.98), rgba(239,230,215,.98));
      clip-path: polygon(0 0, 100% 0, 50% 78%);
      transform-origin: 50% 0%;
      transform: rotateX(0deg);
      z-index: 7;
      box-shadow: inset 0 0 0 1px rgba(18,48,36,.10);
      transition: transform 900ms cubic-bezier(.2,.9,.2,1);
      border-top-left-radius: 18px;
      border-top-right-radius: 18px;
    }

    .inner-shade{
      position:absolute;
      inset: 18% 6% 10% 6%;
      border-radius: 18px;
      background: radial-gradient(700px 280px at 50% 70%, rgba(18,48,36,.18), transparent 60%);
      z-index: 4;
      pointer-events:none;
      opacity:.55;
      transition: opacity 500ms ease;
    }

    .cta{
      margin-top:2px;
      font-size:14px;
      opacity:.9;
      text-align:center;
    }

    /* Open state */
    .open .flap{ transform: rotateX(165deg); }
    .open .seal{ opacity:0; transform: translate(-50%,-50%) scale(.85); }
    .open .inner-shade{ opacity:.25; }
    .open .card{
      opacity:1;
      transform: translateY(0%);
      z-index: 5;
    }

    @media (prefers-reduced-motion: reduce){
      .flap, .seal, .card, .inner-shade{ transition:none !important; }
      .open .confetti i{ animation: none !important; opacity:0 !important; }
    }
  </style>
</head>

<body>
  <main class="frame">
    <div class="title">
      <h1>Holiday Postcard</h1>
      <p>Click the envelope to open.</p>
    </div>

    <div class="envelope-wrap" id="wrap">
      <button class="envelope-btn" id="toggle" aria-label="Open envelope" aria-expanded="false">

        <!-- Confetti lives above everything -->
        <div class="confetti" id="confetti" aria-hidden="true"></div>

        <div class="card" aria-hidden="true" id="card">
          <div class="card-topbar">
            <div class="headline"><span class="dot" aria-hidden="true"></span><span>Happy Holidays</span></div>
            <div class="hint" id="hint">Sound is off</div>
          </div>
          <div class="media">
            <!-- Replace src with your MP4 asset filename -->
            <video id="revealVideo" playsinline muted loop preload="metadata">
              <source src="Green Foliage Holiday Virtual Background.mp4" type="video/mp4" />
              Your browser does not support the video tag.
            </video>
          </div>
        </div>

        <div class="env" aria-hidden="true">
          <div class="env-base"></div>
          <div class="inner-shade"></div>
          <div class="flap"></div>
          <div class="seal"><span>OPEN</span></div>
        </div>

      </button>
    </div>

    <div class="cta">Tip: Click again to close.</div>
  </main>

  <script>
    const wrap = document.getElementById("wrap");
    const btn = document.getElementById("toggle");
    const video = document.getElementById("revealVideo");
    const hint = document.getElementById("hint");
    const card = document.getElementById("card");
    const confetti = document.getElementById("confetti");

    // Create confetti pieces once
    const COLORS = [
      "#caa34a", // gold
      "#e9d8a6", // light gold
      "#d64550", // red
      "#1f6f54", // green
      "#ffffff"  // white
    ];

    function makeConfetti(count = 70){
      confetti.innerHTML = "";
      const w = confetti.clientWidth || 760;

      for (let i = 0; i < count; i++){
        const piece = document.createElement("i");
        const left = Math.random() * 100;
        const x = (Math.random() - 0.5) * (w * 0.35);
        const r = (Math.random() * 520 + 140) + "deg";
        const dur = (Math.random() * 700 + 900) + "ms";

        piece.style.left = left + "%";
        piece.style.background = COLORS[Math.floor(Math.random() * COLORS.length)];
        piece.style.setProperty("--x", x.toFixed(1) + "px");
        piece.style.setProperty("--r", r);
        piece.style.setProperty("--dur", dur);

        // slight variety
        piece.style.width = (Math.random() * 6 + 6).toFixed(0) + "px";
        piece.style.height = (Math.random() * 10 + 10).toFixed(0) + "px";
        piece.style.opacity = "0";

        confetti.appendChild(piece);
      }
    }

    // Retrigger confetti animation by forcing reflow
    function burstConfetti(){
      // rebuild for randomness each burst
      makeConfetti(75);
      // force a reflow so animation restarts reliably
      void confetti.offsetHeight;
    }

    function setOpen(nextOpen){
      wrap.classList.toggle("open", nextOpen);
      btn.setAttribute("aria-expanded", String(nextOpen));
      card.setAttribute("aria-hidden", String(!nextOpen));

      if (nextOpen){
        burstConfetti();
        video.currentTime = 0;
        const p = video.play();
        if (p && typeof p.catch === "function"){
          p.catch(() => { hint.textContent = "Tap the video if it does not start"; });
        }
      } else {
        video.pause();
        hint.textContent = "Sound is off";
      }
    }

    // Ensure confetti has dimensions
    window.addEventListener("load", () => makeConfetti(75));
    window.addEventListener("resize", () => makeConfetti(75));

    btn.addEventListener("click", () => {
      const isOpen = wrap.classList.contains("open");
      setOpen(!isOpen);
    });

    setOpen(false);
  </script>
</body>
</html>
