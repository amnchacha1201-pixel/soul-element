
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Soul Element Sphere — Living</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;1,300&family=EB+Garamond:ital@0;1&display=swap');

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      background: #0e0c0a;
      color: #d8cfc4;
      font-family: 'EB Garamond', serif;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 40px 20px;
    }

    h1 {
      font-family: 'Cormorant Garamond', serif;
      font-weight: 300;
      font-size: 1.6rem;
      letter-spacing: 0.15em;
      color: #e8ddd0;
      margin-bottom: 6px;
    }

    .subtitle {
      font-style: italic;
      font-size: 0.85rem;
      color: #5a5248;
      letter-spacing: 0.08em;
      margin-bottom: 40px;
    }

    .layout {
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 100%;
      max-width: 600px;
    }

    .controls {
      background: rgba(255, 255, 255, 0.04);
      border: 1px solid rgba(255, 255, 255, 0.08);
      border-radius: 12px;
      padding: 28px;
      width: 260px;
    }

    .controls h2 {
      font-family: 'Cormorant Garamond', serif;
      font-weight: 300;
      font-size: 1rem;
      letter-spacing: 0.12em;
      color: #8a7e70;
      margin-bottom: 20px;
      text-transform: uppercase;
    }

    .element-row {
      display: flex;
      align-items: center;
      margin-bottom: 14px;
      gap: 10px;
    }

    .element-dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      flex-shrink: 0;
    }

    .element-label {
      width: 50px;
      font-size: 0.82rem;
      letter-spacing: 0.06em;
      color: #9a9088;
    }

    .element-row input[type="number"] {
      width: 55px;
      background: rgba(255, 255, 255, 0.06);
      border: 1px solid rgba(255, 255, 255, 0.12);
      border-radius: 6px;
      color: #d8cfc4;
      font-family: 'EB Garamond', serif;
      font-size: 0.9rem;
      padding: 4px 8px;
      text-align: center;
      outline: none;
    }

    .element-pct {
      font-size: 0.75rem;
      color: #9a9088;
      width: 40px;
      text-align: right;
    }

    .btn {
      margin-top: 16px;
      width: 100%;
      padding: 10px;
      background: rgba(200, 180, 150, 0.08);
      border: 1px solid rgba(200, 180, 150, 0.18);
      border-radius: 8px;
      color: #c8b890;
      font-family: 'Cormorant Garamond', serif;
      font-size: 0.95rem;
      letter-spacing: 0.1em;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .btn:hover {
      background: rgba(200, 180, 150, 0.14);
      border-color: rgba(200, 180, 150, 0.32);
    }

    .btn-secondary {
      margin-top: 8px;
    }

    .canvas-wrap {
      margin-top: 40px;
      margin-bottom: 60px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 14px;
    }

    canvas {
      border-radius: 50%;
      display: block;
    }

    .note {
      font-size: 0.75rem;
      font-style: italic;
      color: #5a5248;
      text-align: center;
      line-height: 1.6;
    }
  </style>
</head>
<body>
  <h1>Soul Element Sphere</h1>
  <p class="subtitle">From Blueprint to Landscape.</p>

  <div class="layout">
    <div class="controls">
      <h2>Five Elements</h2>

      <div class="element-row">
        <div class="element-dot" style="background:#5F7F6B;"></div>
        <span class="element-label">Wood</span>
        <input type="number" id="wood" value="3" min="0" max="10" oninput="rebuild()">
        <span class="element-pct" id="wood-pct">—</span>
      </div>

      <div class="element-row">
        <div class="element-dot" style="background:#D27A55;"></div>
        <span class="element-label">Fire</span>
        <input type="number" id="fire" value="2" min="0" max="10" oninput="rebuild()">
        <span class="element-pct" id="fire-pct">—</span>
      </div>

      <div class="element-row">
        <div class="element-dot" style="background:#C8A96E;"></div>
        <span class="element-label">Earth</span>
        <input type="number" id="earth" value="2" min="0" max="10" oninput="rebuild()">
        <span class="element-pct" id="earth-pct">—</span>
      </div>

      <div class="element-row">
        <div class="element-dot" style="background:#556C86;"></div>
        <span class="element-label">Water</span>
        <input type="number" id="water" value="2" min="0" max="10" oninput="rebuild()">
        <span class="element-pct" id="water-pct">—</span>
      </div>

      <div class="element-row">
        <div class="element-dot" style="background:#C8CDD4;"></div>
        <span class="element-label">Metal</span>
        <input type="number" id="metal" value="1" min="0" max="10" oninput="rebuild()">
        <span class="element-pct" id="metal-pct">—</span>
      </div>

      <button class="btn" onclick="rebuild()">Regenerate</button>
      <button class="btn btn-secondary" onclick="toggleTestSpeed()">Toggle Test Speed</button>
    </div>

    <div class="canvas-wrap">
      <canvas id="sphere" width="500" height="500" style="width:100%;max-width:460px;"></canvas>
      <p class="note">静かに、呼吸している。</p>
    </div>
  </div>

  <script>
    const COLORS = {
      wood:  { r: 0x5F, g: 0x7F, b: 0x6B },
      fire:  { r: 0xD2, g: 0x7A, b: 0x55 },
      earth: { r: 0xC8, g: 0xA9, b: 0x6E },
      water: { r: 0x55, g: 0x6C, b: 0x86 },
      metal: { r: 0xC8, g: 0xCD, b: 0xD4 }
    };

    const KEYS = ['wood', 'fire', 'earth', 'water', 'metal'];

    const canvas = document.getElementById('sphere');
    const ctx = canvas.getContext('2d');
    const W = canvas.width;
    const H = canvas.height;
    const cx = W / 2;
    const cy = H / 2;
    const R = W * 0.44;

    const NORMAL_ROT_SPEED = (Math.PI * 2) / (33.75 * 60);
    const TEST_ROT_SPEED   = (Math.PI * 2) / (20 * 60);
    let rotSpeed = NORMAL_ROT_SPEED;
    let isTestSpeed = false;

    let baseAngle = Math.random() * Math.PI * 2;
    let bakedBaseAngle = 0;
    let segments = [];
    let stars = [];
    let mists = [];
    let baseImageData = null;
    let rafId = null;
    let frame = 0;

    function buildBaseImage(startAngle) {
      const imgData = ctx.createImageData(W, H);
      const d = imgData.data;
      const blurZone = Math.PI * 0.11;
      const activeSegs = segments.filter(s => s.pct > 0.001);

      for (let py = 0; py < H; py++) {
        for (let px = 0; px < W; px++) {
          const dx = px - cx;
          const dy = py - cy;
          const dist = Math.sqrt(dx * dx + dy * dy);
          if (dist > R) continue;

          const rawAngle = Math.atan2(dy, dx);
          const normAngle = ((rawAngle - startAngle) % (Math.PI * 2) + Math.PI * 2) % (Math.PI * 2);

          let wr = 0, wg = 0, wb = 0, totalW = 0;

          activeSegs.forEach(seg => {
            const segStart = ((seg.start - startAngle) % (Math.PI * 2) + Math.PI * 2) % (Math.PI * 2);
            const segMid = segStart + seg.pct * Math.PI;

            let angDist = Math.abs(normAngle - segMid);
            if (angDist > Math.PI) angDist = Math.PI * 2 - angDist;

            const halfSpan = seg.pct * Math.PI;
            const outerDist = Math.max(0, angDist - halfSpan);
            const w = Math.exp(-(outerDist * outerDist) / (blurZone * blurZone * 0.5));

            const c = COLORS[seg.key];
            wr += c.r * w;
            wg += c.g * w;
            wb += c.b * w;
            totalW += w;
          });

          let r = 60, g = 55, b = 50;
          if (totalW > 0) {
            r = wr / totalW;
            g = wg / totalW;
            b = wb / totalW;
          }

          const alpha = 0.58 + (dist / R) * 0.30;
          const i = (py * W + px) * 4;
          d[i] = r;
          d[i + 1] = g;
          d[i + 2] = b;
          d[i + 3] = Math.round(alpha * 255);
        }
      }

      return imgData;
    }

    function buildStars() {
      stars = Array.from({ length: 320 }, () => ({
        angleBaked: Math.random() * Math.PI * 2,
        radFrac: Math.pow(Math.random(), 0.6) * 0.90,
        size: Math.random() * 1.4 + 0.15,
        baseAlpha: Math.random() * 0.28 + 0.06,
        driftSpeed: (Math.random() - 0.5) * 0.00008,
        twinklePhase: Math.random() * Math.PI * 2,
        twinkleSpeed: Math.random() * 0.008 + 0.003
      }));
    }

    function buildMists() {
      mists = [];

      segments.forEach(seg => {
        if (seg.pct < 0.001) return;

        const count = Math.max(10, Math.ceil(seg.pct * 140));

        for (let i = 0; i < count; i++) {
          const t = (i + Math.random() * 0.5) / count;
          const angleBaked = seg.start + (seg.end - seg.start) * t;

          for (let ri = 0; ri < 4; ri++) {
            const rFrac = Math.max(
              0.05,
              Math.min(0.95, 0.08 + ri * 0.22 + (Math.random() - 0.5) * 0.10)
            );

            mists.push({
              angleBaked,
              rFrac,
              blobBaseR: R * (0.10 + Math.random() * 0.18),
              baseAlpha: 0.032 + Math.random() * 0.040,
              color: { ...COLORS[seg.key] },
              breathPhase: Math.random() * Math.PI * 2,
              breathSpeed: Math.random() * 0.006 + 0.003,
              breathAmount: 0.25 + Math.random() * 0.35,
              pulsePhase: Math.random() * Math.PI * 2,
              pulseSpeed: Math.random() * 0.004 + 0.002,
              pulseAmount: 0.008 + Math.random() * 0.006
            });
          }
        }
      });
    }

    function colorAt(absAngle) {
      const normAngle = ((absAngle - baseAngle) % (Math.PI * 2) + Math.PI * 2) % (Math.PI * 2);
      const activeSegs = segments.filter(s => s.pct > 0.001);
      if (!activeSegs.length) return { r: 180, g: 175, b: 165 };

      let wr = 0, wg = 0, wb = 0, totalW = 0;

      activeSegs.forEach(seg => {
        const span = seg.pct * Math.PI * 2;
        const segMid = ((seg.start - baseAngle + span / 2) % (Math.PI * 2) + Math.PI * 2) % (Math.PI * 2);

        let d = Math.abs(normAngle - segMid);
        if (d > Math.PI) d = Math.PI * 2 - d;

        const w = Math.exp(-(d * d) / (Math.PI * 0.12 * Math.PI * 0.12 * 0.5));
        const c = COLORS[seg.key];

        wr += c.r * w;
        wg += c.g * w;
        wb += c.b * w;
        totalW += w;
      });

      if (!totalW) return { r: 180, g: 175, b: 165 };
      return { r: wr / totalW, g: wg / totalW, b: wb / totalW };
    }

    function draw(frame) {
      ctx.clearRect(0, 0, W, H);

      // Layer 1: Base color field
      ctx.save();
      ctx.beginPath();
      ctx.arc(cx, cy, R, 0, Math.PI * 2);
      ctx.clip();
      if (baseImageData) ctx.putImageData(baseImageData, 0, 0);
      ctx.restore();

      // Layer 2: Mist
      ctx.save();
      ctx.beginPath();
      ctx.arc(cx, cy, R * 1.01, 0, Math.PI * 2);
      ctx.clip();

      const rotationOffset = baseAngle - bakedBaseAngle;

      mists.forEach(m => {
        const currentAngle = m.angleBaked + rotationOffset;
        const bx = cx + Math.cos(currentAngle) * m.rFrac * R;
        const by = cy + Math.sin(currentAngle) * m.rFrac * R;

        const breathCycle = Math.sin(frame * m.breathSpeed + m.breathPhase);
        const alpha = Math.min(0.12, m.baseAlpha * (1 + m.breathAmount * breathCycle));

        const pulseCycle = Math.sin(frame * m.pulseSpeed + m.pulsePhase);
        const blobR = m.blobBaseR * (1 + m.pulseAmount * pulseCycle);

        const c = m.color;
        const grad = ctx.createRadialGradient(bx, by, 0, bx, by, blobR);
        grad.addColorStop(0, `rgba(${c.r},${c.g},${c.b},${alpha})`);
        grad.addColorStop(0.55, `rgba(${c.r},${c.g},${c.b},${alpha * 0.28})`);
        grad.addColorStop(1, `rgba(${c.r},${c.g},${c.b},0)`);

        ctx.fillStyle = grad;
        ctx.fillRect(0, 0, W, H);
      });

      ctx.restore();

      // Layer 3: 3D shading
      ctx.save();
      ctx.beginPath();
      ctx.arc(cx, cy, R, 0, Math.PI * 2);
      ctx.clip();

      const hl = ctx.createRadialGradient(cx - R * 0.3, cy - R * 0.3, 0, cx - R * 0.05, cy - R * 0.05, R * 0.88);
      hl.addColorStop(0, 'rgba(255,252,248,0.34)');
      hl.addColorStop(0.32, 'rgba(255,252,248,0.12)');
      hl.addColorStop(0.65, 'rgba(255,252,248,0.02)');
      hl.addColorStop(1, 'rgba(255,252,248,0)');
      ctx.fillStyle = hl;
      ctx.fillRect(0, 0, W, H);

      const sh = ctx.createRadialGradient(cx + R * 0.22, cy + R * 0.22, R * 0.18, cx + R * 0.08, cy + R * 0.08, R);
      sh.addColorStop(0, 'rgba(12,8,5,0)');
      sh.addColorStop(0.48, 'rgba(12,8,5,0.09)');
      sh.addColorStop(1, 'rgba(12,8,5,0.24)');
      ctx.fillStyle = sh;
      ctx.fillRect(0, 0, W, H);

      ctx.restore();

      // Layer 4: Stars
      ctx.save();
      ctx.beginPath();
      ctx.arc(cx, cy, R * 0.97, 0, Math.PI * 2);
      ctx.clip();

      stars.forEach(s => {
        const sphereCoupling = baseAngle * 0.35;
        const starAngle = s.angleBaked + sphereCoupling + frame * s.driftSpeed;
        const r = s.radFrac * R;
        const px = cx + Math.cos(starAngle) * r;
        const py = cy + Math.sin(starAngle) * r;

        const twinkle = 1 + 0.25 * Math.sin(frame * s.twinkleSpeed + s.twinklePhase);
        const alpha = Math.min(0.55, s.baseAlpha * twinkle);
        const col = colorAt(starAngle);
        const br = 55 + 55 * twinkle;

        ctx.save();
        ctx.globalAlpha = alpha;
        ctx.fillStyle = `rgb(${Math.min(255, (col.r + br) | 0)},${Math.min(255, (col.g + br) | 0)},${Math.min(255, (col.b + br) | 0)})`;
        ctx.beginPath();
        ctx.arc(px, py, s.size * twinkle, 0, Math.PI * 2);
        ctx.fill();
        ctx.restore();
      });

      ctx.restore();

      // Layer 5: Clean edge
      ctx.save();
      ctx.globalCompositeOperation = 'destination-out';
      ctx.beginPath();
      ctx.rect(0, 0, W, H);
      ctx.arc(cx, cy, R * 0.994, 0, Math.PI * 2, true);
      ctx.fill();
      ctx.restore();

      // Inner rim glow
      ctx.save();
      ctx.beginPath();
      ctx.arc(cx, cy, R * 0.994, 0, Math.PI * 2);
      ctx.clip();

      const rim = ctx.createRadialGradient(cx, cy, R * 0.87, cx, cy, R * 0.994);
      rim.addColorStop(0, 'rgba(255,252,248,0)');
      rim.addColorStop(1, 'rgba(255,252,248,0.07)');
      ctx.fillStyle = rim;
      ctx.fillRect(0, 0, W, H);

      ctx.restore();
    }

    function loop() {
      frame++;
      baseAngle += rotSpeed;

      if (frame % 12 === 0) {
        baseImageData = buildBaseImage(baseAngle);
      }

      draw(frame);
      rafId = requestAnimationFrame(loop);
    }

    function rebuild() {
      cancelAnimationFrame(rafId);

      const vals = KEYS.map(k => Math.max(0, parseFloat(document.getElementById(k).value) || 0));
      const total = vals.reduce((a, b) => a + b, 0);
      const pcts = vals.map(v => total > 0 ? v / total : 0);

      KEYS.forEach((k, i) => {
        const p = total > 0 ? Math.round(pcts[i] * 100) : 0;
        document.getElementById(k + '-pct').textContent = p + '%';
      });

      baseAngle = Math.random() * Math.PI * 2;
      bakedBaseAngle = baseAngle;

      let angle = baseAngle;
      segments = KEYS.map((k, i) => {
        const span = pcts[i] * Math.PI * 2;
        const seg = { key: k, start: angle, end: angle + span, pct: pcts[i] };
        angle += span;
        return seg;
      });

      buildStars();
      buildMists();
      baseImageData = buildBaseImage(baseAngle);
      frame = 0;
      loop();
    }

    function toggleTestSpeed() {
      isTestSpeed = !isTestSpeed;
      rotSpeed = isTestSpeed ? TEST_ROT_SPEED : NORMAL_ROT_SPEED;
    }

    rebuild();
  </script>
</body>
</html>
