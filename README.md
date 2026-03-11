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
