# -
<!doctype html>
<html lang="vi">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Thiệp Chúc Tết (nhạc file + pháo hoa)</title>
  <style>
    :root{
      --bg1:#070814;
      --bg2:#120a2a;
      --gold:#ffd166;
      --red:#c1121f;
      --red2:#9b0f1a;
      --text:#fff;
    }
    *{box-sizing:border-box}
    html,body{height:100%; margin:0; overflow:hidden; font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;}
    body{
      background: radial-gradient(1200px 900px at 50% 30%, #1b1142 0%, var(--bg2) 35%, var(--bg1) 100%);
      color: var(--text);
    }

    /* Fullscreen fireworks canvas */
    #fx{
      position:fixed; inset:0;
      width:100vw; height:100vh;
      display:block;
      pointer-events:none;
      z-index:1;
    }

    /* Start overlay */
    .overlay{
      position:fixed; inset:0;
      display:flex; align-items:center; justify-content:center;
      background: rgba(0,0,0,.45);
      backdrop-filter: blur(8px);
      z-index: 10;
      transition: opacity .45s ease, transform .45s ease;
    }
    .overlay.hide{opacity:0; transform: scale(1.02); pointer-events:none;}
    .startCard{
      width:min(560px, 92vw);
      padding: 22px 22px 18px;
      border-radius: 22px;
      border: 1px solid rgba(255,255,255,.14);
      background: rgba(255,255,255,.08);
      box-shadow: 0 16px 40px rgba(0,0,0,.45);
      text-align:center;
    }
    .startCard h1{margin:0 0 8px; font-size: clamp(22px, 3.2vw, 34px); font-weight: 850;}
    .startCard p{margin:0 0 14px; color: rgba(255,255,255,.86)}
    .btn{
      appearance:none; border:0; cursor:pointer;
      padding: 12px 18px; border-radius: 999px;
      font-weight: 900; color:#2b1b00;
      background: linear-gradient(135deg, #ffd166, #ffef9a);
      box-shadow: 0 10px 24px rgba(255,209,102,.25);
    }
    .btn:active{transform: translateY(1px)}
    .hintSmall{margin-top:10px; font-size:12px; opacity:.75}

    /* Main */
    .stage{
      position:relative;
      height:100%;
      display:flex;
      align-items:center;
      justify-content:center;
      padding: 22px;
      z-index:2;
    }
    .wrap{
      width: min(980px, 96vw);
      display:grid;
      grid-template-columns: 1fr min(320px, 36vw);
      gap: 18px;
      align-items:stretch;
    }

    /* Center red card */
    .card{
      border-radius: 26px;
      border: 1px solid rgba(255,255,255,.12);
      box-shadow: 0 18px 50px rgba(0,0,0,.45);
      overflow:hidden;
      transform: translateY(10px) scale(.98);
      opacity: 0;
      transition: opacity .55s ease, transform .55s ease;
      background: rgba(255,255,255,.06);
    }
    .card.show{opacity:1; transform: translateY(0) scale(1);}

    .cardInner{
      min-height: 420px;
      border-radius: 26px;
      background: radial-gradient(900px 400px at 50% 20%, rgba(255,255,255,.14), rgba(255,255,255,0) 55%),
                  linear-gradient(135deg, var(--red), var(--red2));
      position:relative;
      padding: 22px 22px 18px;
    }
    .ornTop{
      position:absolute; inset: 10px 12px auto 12px;
      display:flex; justify-content:space-between; align-items:center;
      pointer-events:none; opacity:.9;
      gap:10px;
    }
    .pill{
      font-size:12px;
      background: rgba(0,0,0,.22);
      border: 1px solid rgba(255,255,255,.14);
      padding: 6px 10px;
      border-radius: 999px;
      white-space:nowrap;
    }
    .title{
      margin: 44px 0 8px;
      font-size: clamp(28px, 4vw, 46px);
      font-weight: 950;
      letter-spacing: .5px;
      text-shadow: 0 10px 26px rgba(0,0,0,.35);
    }
    .sub{
      margin: 0 0 14px;
      font-size: clamp(14px, 1.6vw, 18px);
      opacity:.96;
      line-height:1.55;
    }
    .divider{
      height:1px;
      background: rgba(255,255,255,.18);
      margin: 16px 0 14px;
    }

    /* Controls inside card */
    .controls{
      display:grid;
      gap: 10px;
      margin-top: 10px;
      background: rgba(0,0,0,.18);
      border: 1px solid rgba(255,255,255,.14);
      border-radius: 18px;
      padding: 12px 12px 10px;
    }
    .controlsRow{
      display:flex;
      align-items:center;
      gap: 10px;
      justify-content:space-between;
      flex-wrap: wrap;
    }
    .label{font-size: 13px; opacity:.92; display:flex; gap:8px; align-items:center;}
    input[type="range"]{width: min(360px, 72vw); accent-color: var(--gold);}
    .smallBtn{
      appearance:none;
      border: 1px solid rgba(255,255,255,.18);
      background: rgba(255,255,255,.10);
      color: rgba(255,255,255,.95);
      padding: 8px 12px;
      border-radius: 999px;
      font-weight: 900;
      cursor:pointer;
    }
    .smallBtn:active{transform: translateY(1px)}
    .footer{margin-top: 14px; font-size: 12px; opacity: .85;}

    /* Right peach blossom panel */
    .side{
      border-radius: 26px;
      border: 1px solid rgba(255,255,255,.12);
      background: rgba(255,255,255,.06);
      box-shadow: 0 18px 50px rgba(0,0,0,.35);
      overflow:hidden;
      transform: translateY(10px) scale(.98);
      opacity: 0;
      transition: opacity .55s ease .06s, transform .55s ease .06s;
    }
    .side.show{opacity:1; transform: translateY(0) scale(1);}
    .sideHeader{
      padding: 12px 14px;
      display:flex; align-items:center; justify-content:space-between;
      border-bottom: 1px solid rgba(255,255,255,.10);
    }
    .sideHeader .t{font-weight:950}
    #tree{
      width:100%;
      height:100%;
      display:block;
      background: radial-gradient(500px 380px at 60% 30%, rgba(255,77,166,.14), rgba(0,0,0,0) 65%);
    }

    @media (max-width: 820px){
      .wrap{grid-template-columns: 1fr;}
      .side{min-height: 320px;}
    }

    .tapHint{
      position:fixed;
      left:50%;
      bottom: 14px;
      transform: translateX(-50%);
      z-index: 3;
      font-size: 13px;
      opacity: 0;
      transition: opacity .45s ease;
      background: rgba(0,0,0,.25);
      border: 1px solid rgba(255,255,255,.10);
      padding: 8px 12px;
      border-radius: 999px;
      pointer-events:none;
    }
    .tapHint.show{opacity:.9;}
  </style>
</head>
<body>
  <!-- Fireworks canvas -->
  <canvas id="fx"></canvas>

  <!-- Nhạc nền: đổi file tại src -->
  <audio id="bgm" loop preload="auto">
    <source src="Download.mp3" type="audio/mpeg" />
    <!-- Nếu dùng wav: <source src="tet.wav" type="audio/wav" /> -->
  </audio>

  <div class="stage">
    <div class="wrap">
      <!-- Thiệp đỏ ở giữa -->
      <section id="card" class="card" aria-label="Thiệp chúc Tết">
        <div class="cardInner">
          <div class="ornTop">
            <div class="pill">🧧 Thiệp Chúc Tết</div>
            <div class="pill">✨ Click để bắn pháo hoa</div>
          </div>

          <h1 class="title">Chúc Mừng Năm Mới!</h1>
          <p class="sub">
            Chúc bạn vui trong sức khỏe
            Trẻ trong tâm hồn 
            Trưởng thành mọi lĩnh vực💕
          </p>

          <div class="divider"></div>

          <!-- Thanh điều chỉnh âm nhạc trong thiệp -->
          <div class="controls">
            <div class="controlsRow">
              <div class="label">🎵 Nhạc nền</div>
              <button id="toggleMusic" class="smallBtn" type="button">Tạm dừng</button>
            </div>
            <div class="controlsRow">
              <div class="label">🔊 Âm lượng</div>
              <input id="vol" type="range" min="0" max="100" value="35" />
              <div class="label"><span id="volTxt">35%</span></div>
            </div>
          </div>

          <div class="footer">Tip: bấm nhiều lần để pháo hoa dày hơn 🎆</div>
        </div>
      </section>

      <!-- Cây hoa đào bên phải -->
      <aside id="side" class="side" aria-label="Cây hoa đào">
        <div class="sideHeader">
          <div class="t">🌸 Hoa đào</div>
          <div style="font-size:12px; opacity:.85;">Xuân về rực rỡ</div>
        </div>
        <canvas id="tree"></canvas>
      </aside>
    </div>
  </div>

  <!-- Start overlay -->
  <div id="overlay" class="overlay">
    <div class="startCard">
      <h1>Thiệp Chúc Tết 🎆</h1>
      <p>Bấm <b>Start</b> để mở thiệp, bật nhạc, và click quanh màn hình để bắn pháo hoa.</p>
      <button id="startBtn" class="btn" type="button">Start</button>
      <div class="hintSmall">Đổi nhạc: chỉ cần thay <b>tet.mp3</b> bằng file khác (và sửa tên trong code nếu cần).</div>
    </div>
  </div>

  <div id="tapHint" class="tapHint">✨ Click/chạm bất kỳ vị trí nào để nổ pháo hoa</div>

<script>
(() => {
  // ---------- FX Canvas (fireworks) ----------
  const fx = document.getElementById('fx');
  const fctx = fx.getContext('2d');

  function resizeFX(){
    const dpr = Math.max(1, Math.min(2, devicePixelRatio || 1));
    fx.width = Math.floor(innerWidth * dpr);
    fx.height = Math.floor(innerHeight * dpr);
    fx.style.width = innerWidth + 'px';
    fx.style.height = innerHeight + 'px';
    fctx.setTransform(dpr,0,0,dpr,0,0);
  }
  addEventListener('resize', resizeFX, {passive:true});
  resizeFX();

  const rand = (a,b)=> a + Math.random()*(b-a);
  const clamp = (x,a,b)=> Math.max(a, Math.min(b,x));

  const stars = Array.from({length: 120}, () => ({
    x: Math.random()*innerWidth,
    y: Math.random()*innerHeight,
    r: rand(0.6, 1.8),
    tw: rand(0, Math.PI*2),
    s: rand(0.4, 1.2)
  }));

  function drawStars(t){
    for(const s of stars){
      const tw = (Math.sin(t*0.001*s.s + s.tw)+1)/2;
      fctx.globalAlpha = 0.18 + tw*0.55;
      fctx.fillStyle = 'white';
      fctx.beginPath();
      fctx.arc(s.x, s.y, s.r*(0.6+tw*0.9), 0, Math.PI*2);
      fctx.fill();
    }
    fctx.globalAlpha = 1;
  }

  const sparks = [];
  function burst(x,y){
    const n = Math.floor(rand(50, 95));
    const baseHue = rand(0, 360);
    for(let i=0;i<n;i++){
      const a = (i/n)*Math.PI*2 + rand(-0.08, 0.08);
      const sp = rand(2.2, 7.1);
      sparks.push({
        x,y,
        vx: Math.cos(a)*sp,
        vy: Math.sin(a)*sp,
        life: rand(40, 72),
        age: 0,
        r: rand(1.2, 2.5),
        hue: (baseHue + rand(-28, 28) + 360) % 360,
        trail: []
      });
    }
  }

  function updateFireworks(){
    fctx.save();
    fctx.globalCompositeOperation = 'lighter';

    for(let i=sparks.length-1;i>=0;i--){
      const p = sparks[i];
      p.age++;
      p.vx *= 0.985;
      p.vy = p.vy*0.985 + 0.045;
      p.x += p.vx; p.y += p.vy;

      p.trail.push([p.x,p.y]);
      if(p.trail.length > 10) p.trail.shift();

      const a = 1 - p.age/p.life;
      const alpha = clamp(a, 0, 1);

      fctx.lineWidth = p.r*2.2;
      fctx.strokeStyle = `hsla(${p.hue}, 95%, 70%, ${alpha*0.55})`;
      fctx.beginPath();
      for(let k=0;k<p.trail.length;k++){
        const [tx,ty] = p.trail[k];
        if(k===0) fctx.moveTo(tx,ty); else fctx.lineTo(tx,ty);
      }
      fctx.stroke();

      fctx.fillStyle = `hsla(${p.hue}, 95%, 70%, ${alpha})`;
      fctx.beginPath();
      fctx.arc(p.x, p.y, p.r*1.8, 0, Math.PI*2);
      fctx.fill();

      if(p.age >= p.life || p.y > innerHeight+40 || p.x < -60 || p.x > innerWidth+60){
        sparks.splice(i,1);
      }
    }
    fctx.restore();
  }

  // ---------- Tree Canvas (right) ----------
  const tree = document.getElementById('tree');
  const tctx = tree.getContext('2d');
  let blossoms = [];

  function resizeTree(){
    const side = document.getElementById('side');
    const rect = side.getBoundingClientRect();
    const headerH = 49;
    const w = rect.width;
    const h = Math.max(260, rect.height - headerH);

    const dpr = Math.max(1, Math.min(2, devicePixelRatio || 1));
    tree.width = Math.floor(w * dpr);
    tree.height = Math.floor(h * dpr);
    tree.style.width = w + 'px';
    tree.style.height = h + 'px';
    tctx.setTransform(dpr,0,0,dpr,0,0);

    seedBlossoms(w,h);
  }

  function seedBlossoms(w,h){
    blossoms = [];
    const cx = w*0.55;
    const baseY = h*0.93;
    const count = Math.floor(clamp(w*h/2200, 90, 180));
    for(let i=0;i<count;i++){
      const a = Math.random()*Math.PI*2;
      const rx = w*0.33, ry = h*0.28;
      const r = Math.sqrt(Math.random());
      const x = cx + Math.cos(a)*rx*r + rand(-10,10);
      const y = baseY - h*0.50 + Math.sin(a)*ry*r + rand(-10,10);
      blossoms.push({
        x,y,
        size: rand(3.2, 7.6),
        rot: rand(0, Math.PI*2),
        hue: rand(322, 340),
        sat: rand(70, 92),
        lit: rand(55, 72),
        sway: rand(0.7, 1.4),
        phase: rand(0, Math.PI*2)
      });
    }
  }

  function drawPetal(ctx,x,y,sz,rot,c1,c2){
    ctx.save();
    ctx.translate(x,y);
    ctx.rotate(rot);
    const g = ctx.createRadialGradient(0,0,0, 0,0,sz*1.6);
    g.addColorStop(0, c2);
    g.addColorStop(1, c1);
    ctx.fillStyle = g;
    for(let i=0;i<5;i++){
      ctx.rotate((Math.PI*2)/5);
      ctx.beginPath();
      ctx.moveTo(0,0);
      ctx.quadraticCurveTo(sz*0.7, -sz*0.2, sz*1.15, 0);
      ctx.quadraticCurveTo(sz*0.7, sz*0.2, 0,0);
      ctx.closePath();
      ctx.fill();
    }
    ctx.fillStyle = 'rgba(255, 209, 102, .95)';
    ctx.beginPath();
    ctx.arc(0,0, sz*0.22, 0, Math.PI*2);
    ctx.fill();
    ctx.restore();
  }

  function drawTree(t){
    const w = parseFloat(tree.style.width) || tree.getBoundingClientRect().width;
    const h = parseFloat(tree.style.height) || tree.getBoundingClientRect().height;

    tctx.clearRect(0,0,w,h);

    const bg = tctx.createRadialGradient(w*0.65, h*0.35, 0, w*0.65, h*0.35, w*0.9);
    bg.addColorStop(0, 'rgba(255,77,166,.10)');
    bg.addColorStop(1, 'rgba(0,0,0,0)');
    tctx.fillStyle = bg;
    tctx.fillRect(0,0,w,h);

    const cx = w*0.55;
    const baseY = h*0.93;
    const trunkW = clamp(w*0.05, 10, 20);

    tctx.save();
    tctx.lineCap = 'round';
    tctx.lineJoin = 'round';
    tctx.strokeStyle = 'rgba(92, 52, 32, 0.95)';
    tctx.lineWidth = trunkW;
    tctx.beginPath();
    tctx.moveTo(cx, baseY);
    tctx.bezierCurveTo(cx-14, baseY-70, cx+18, baseY-135, cx-2, baseY-220);
    tctx.stroke();

    function branch(x0,y0, x1,y1, x2,y2, w2, alpha){
      tctx.strokeStyle = `rgba(110, 62, 38, ${alpha})`;
      tctx.lineWidth = w2;
      tctx.beginPath();
      tctx.moveTo(x0,y0);
      tctx.quadraticCurveTo(x1,y1, x2,y2);
      tctx.stroke();
    }

    const sway = Math.sin(t*0.0012)*6;
    const topX = cx-2 + sway*0.25;
    const topY = baseY-220;

    branch(topX, topY+30, topX-70+sway, topY-18, topX-115+sway*1.1, topY-88, trunkW*0.58, 0.9);
    branch(topX, topY+26, topX+58+sway*0.6, topY-20, topX+105+sway*0.8, topY-95, trunkW*0.52, 0.9);

    for(let i=0;i<7;i++){
      const side = i%2===0 ? -1 : 1;
      const bx = topX + side*rand(28, 110) + sway*rand(0.3, 0.9);
      const by = topY + rand(-95, -8);
      const ex = bx + side*rand(18, 54);
      const ey = by + rand(-26, 16);
      branch(bx, by, (bx+ex)/2, (by+ey)/2, ex, ey, rand(3.5,6.2), 0.62);
    }
    tctx.restore();

    for(const b of blossoms){
      const drift = Math.sin(t*0.001 + b.phase) * b.sway;
      const c1 = `hsla(${b.hue}, ${b.sat}%, ${b.lit}%, .90)`;
      const c2 = `hsla(${b.hue}, ${b.sat}%, ${clamp(b.lit+18, 0, 90)}%, .95)`;
      drawPetal(tctx, b.x + drift, b.y, b.size, b.rot + drift*0.01, c1, c2);
    }
  }

  // ---------- Music (FILE AUDIO) ----------
  const bgm = document.getElementById('bgm');
  let isPlaying = true;
  let currentVol01 = 0.35;

  function ensureMusicStart(){
    // Trình duyệt cần thao tác người dùng => gọi ở Start / click
    bgm.play().catch(()=>{});
  }
  function setVolume(v01){
    bgm.volume = Math.max(0, Math.min(1, v01));
  }
  function toggleMusic(){
    isPlaying = !isPlaying;
    if(isPlaying) bgm.play().catch(()=>{});
    else bgm.pause();
    document.getElementById('toggleMusic').textContent = isPlaying ? 'Tạm dừng' : 'Phát nhạc';
  }

  // ---------- UI wiring ----------
  const overlay = document.getElementById('overlay');
  const startBtn = document.getElementById('startBtn');
  const card = document.getElementById('card');
  const side = document.getElementById('side');
  const tapHint = document.getElementById('tapHint');
  const vol = document.getElementById('vol');
  const volTxt = document.getElementById('volTxt');

  function syncVol(){
    const v = parseInt(vol.value, 10);
    volTxt.textContent = v + '%';
    currentVol01 = v / 100;
    setVolume(currentVol01);
  }
  vol.addEventListener('input', syncVol);

  document.getElementById('toggleMusic').addEventListener('click', () => {
    ensureMusicStart();
    toggleMusic();
  });

  function start(){
    syncVol();
    ensureMusicStart();

    overlay.classList.add('hide');
    card.classList.add('show');
    side.classList.add('show');
    tapHint.classList.add('show');
    setTimeout(() => tapHint.classList.remove('show'), 4200);

    setTimeout(() => resizeTree(), 80);
  }
  startBtn.addEventListener('click', start);

  // fireworks input (after started)
  function pointerXY(e){
    const rect = fx.getBoundingClientRect();
    if(e.touches && e.touches[0]){
      return { x: e.touches[0].clientX - rect.left, y: e.touches[0].clientY - rect.top };
    }
    return { x: e.clientX - rect.left, y: e.clientY - rect.top };
  }

  function onBoom(e){
    if(!card.classList.contains('show')) return;
    const {x,y} = pointerXY(e);

    // tránh nổ ngay giữa thiệp cho dễ nhìn
    const cx = innerWidth*0.42;
    const cy = innerHeight*0.50;
    const d = Math.hypot(x-cx, y-cy);
    const px = d < 120 ? x + rand(-140,140) : x;
    const py = d < 120 ? y + rand(-110,110) : y;

    burst(px,py);
    if(Math.random()<0.65) burst(px + rand(-50,50), py + rand(-40,40));

    // click cũng “kích” autoplay nếu user chưa bấm Start
    ensureMusicStart();
  }

  addEventListener('mousedown', onBoom);
  addEventListener('touchstart', onBoom, {passive:true});

  addEventListener('resize', () => {
    for(const s of stars){ s.x = Math.random()*innerWidth; s.y = Math.random()*innerHeight; }
    setTimeout(resizeTree, 60);
  }, {passive:true});

  // ---------- Animation loop ----------
  function frame(t){
    fctx.fillStyle = 'rgba(0,0,0,0.22)';
    fctx.fillRect(0,0,innerWidth,innerHeight);
    drawStars(t);
    updateFireworks();
    if(side.classList.contains('show')) drawTree(t);
    requestAnimationFrame(frame);
  }
  requestAnimationFrame(frame);

})();
</script>
</body>
</html>
