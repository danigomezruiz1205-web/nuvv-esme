<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Jardín de Kittys y Sol Kuromi 🌸✨</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@500;600;700&display=swap" rel="stylesheet">
    <style>
        * {
            box-sizing: border-box;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        html, body {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            background: #231138;
            font-family: 'Quicksand', sans-serif;
        }

        #gardenCanvas {
            display: block;
            width: 100%;
            height: 100%;
            cursor: pointer;
        }

        #resetBtn {
            position: fixed;
            bottom: 1.25rem;
            right: 1.25rem;
            z-index: 10;
            padding: 0.52rem 1.1rem;
            border-radius: 999px;
            background: rgba(38, 16, 56, 0.56);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 200, 232, 0.48);
            color: rgba(255, 244, 250, 0.94);
            font-family: 'Quicksand', sans-serif;
            font-size: 0.8rem;
            font-weight: 700;
            letter-spacing: 0.04em;
            cursor: pointer;
            box-shadow: 0 6px 22px rgba(32, 10, 48, 0.35);
            transition: all 0.55s cubic-bezier(0.22, 1, 0.36, 1);
        }

        #resetBtn.initial-center {
            bottom: auto;
            right: auto;
            top: 56%;
            left: 50%;
            transform: translate(-50%, -50%);
            padding: 0.95rem 2.1rem;
            font-size: 1.15rem;
            background: linear-gradient(135deg, rgba(190, 24, 93, 0.82), rgba(126, 34, 206, 0.82));
            border: 2px solid rgba(254, 240, 138, 0.82);
            box-shadow: 0 0 32px rgba(255, 105, 180, 0.65), 0 12px 28px rgba(24, 6, 40, 0.55);
        }

        #resetBtn.initial-center:hover {
            transform: translate(-50%, -53%) scale(1.05);
            box-shadow: 0 0 42px rgba(254, 240, 138, 0.85), 0 14px 32px rgba(24, 6, 40, 0.65);
        }

        #resetBtn:not(.initial-center):hover {
            background: rgba(157, 23, 77, 0.68);
            border-color: rgba(253, 230, 138, 0.85);
            color: #ffffff;
            transform: translateY(-2px);
        }

        #resetBtn:active {
            transform: scale(0.96);
        }
    </style>
</head>
<body>

    <canvas id="gardenCanvas"></canvas>
    <button id="resetBtn" class="initial-center" type="button">Plantar jardín</button>

    <script>
        const canvas = document.getElementById('gardenCanvas');
        const ctx = canvas.getContext('2d');
        const resetBtn = document.getElementById('resetBtn');

        let width = 0;
        let height = 0;
        let dpr = 1;

        const BOW_PALETTES = [
            { bow: '#ff3b8d', center: '#ffb8d9', outerPetal: '#ff6ba8', innerPetal: '#ffd6e9', glow: 'rgba(255, 77, 148, 0.45)', isRose: true },
            { bow: '#a855f7', center: '#f3e8ff', outerPetal: '#c084fc', innerPetal: '#f5d0fe', glow: 'rgba(192, 132, 252, 0.45)', isRose: false },
            { bow: '#fb7185', center: '#ffe4e6', outerPetal: '#fda4af', innerPetal: '#fff1f2', glow: 'rgba(251, 113, 133, 0.45)', isRose: true },
            { bow: '#f97316', center: '#ffedd5', outerPetal: '#fdba74', innerPetal: '#fff7ed', glow: 'rgba(251, 146, 60, 0.42)', isRose: true },
            { bow: '#e879f9', center: '#fae8ff', outerPetal: '#f0abfc', innerPetal: '#ffffff', glow: 'rgba(232, 121, 249, 0.45)', isRose: false },
            { bow: '#f43f5e', center: '#fde68a', outerPetal: '#fb7185', innerPetal: '#ffe4e6', glow: 'rgba(244, 63, 94, 0.45)', isRose: true }
        ];

        const flowers = [];
        const seeds = [];
        const particles = [];
        const fireflies = [];
        const butterflies = [];
        const grassBlades = [];
        const stars = [];
        let gardenStarted = false;

        const pointer = {
            x: -1000,
            y: -1000,
            vx: 0,
            vy: 0,
            active: false
        };

        function resize() {
            dpr = Math.min(window.devicePixelRatio || 1, 2);
            width = window.innerWidth;
            height = window.innerHeight;
            canvas.width = width * dpr;
            canvas.height = height * dpr;
            ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
            initAmbientElements();
        }

        function getHillHeight(x, layer) {
            const nx = x / Math.max(width, 1);
            if (layer === 0) {
                return height * 0.72 + Math.sin(nx * Math.PI * 1.5 + 0.35) * (height * 0.042) + Math.cos(nx * Math.PI * 3.0) * (height * 0.015);
            }
            if (layer === 1) {
                return height * 0.80 + Math.cos(nx * Math.PI * 1.35 - 0.45) * (height * 0.036) + Math.sin(nx * Math.PI * 2.5 + 1.1) * (height * 0.018);
            }
            return height * 0.88 + Math.sin(nx * Math.PI * 1.7 + 1.0) * (height * 0.028) + Math.cos(nx * Math.PI * 2.3) * (height * 0.012);
        }

        function getMountainHeight(x, layer) {
            const nx = x / Math.max(width, 1);
            if (layer === 0) {
                return height * 0.56
                    - Math.sin(nx * Math.PI * 2.1 + 0.2) * (height * 0.085)
                    - Math.sin(nx * Math.PI * 5.2 + 1.1) * (height * 0.032)
                    - Math.cos(nx * Math.PI * 9.0) * (height * 0.012);
            }
            return height * 0.63
                - Math.cos(nx * Math.PI * 1.9 - 0.4) * (height * 0.068)
                - Math.sin(nx * Math.PI * 4.6 + 0.6) * (height * 0.026)
                - Math.cos(nx * Math.PI * 8.2) * (height * 0.01);
        }

        function initAmbientElements() {
            stars.length = 0;
            const starCount = Math.floor((width * height) / 16000);
            for (let i = 0; i < starCount; i++) {
                stars.push({
                    x: Math.random() * width,
                    y: Math.random() * (height * 0.46),
                    r: Math.random() * 1.5 + 0.5,
                    phase: Math.random() * Math.PI * 2,
                    speed: 0.9 + Math.random() * 1.8
                });
            }

            if (fireflies.length === 0) {
                const count = width < 640 ? 28 : 48;
                for (let i = 0; i < count; i++) {
                    fireflies.push(createFirefly(true));
                }
            }

            if (butterflies.length === 0) {
                const bCount = width < 640 ? 5 : 9;
                for (let i = 0; i < bCount; i++) {
                    butterflies.push(createButterfly(true));
                }
            }

            grassBlades.length = 0;
            const bladeCount = Math.floor(width / 6);
            for (let i = 0; i < bladeCount; i++) {
                const gx = (i / Math.max(1, bladeCount - 1)) * width + (Math.random() - 0.5) * 8;
                const layer = i % 2 === 0 ? 1 : 2;
                const gy = getHillHeight(gx, layer) + Math.random() * (height * 0.08);
                grassBlades.push({
                    x: gx,
                    y: Math.min(height, gy),
                    h: (layer === 1 ? 12 : 18) + Math.random() * 16,
                    lean: (Math.random() - 0.5) * 0.5,
                    phase: Math.random() * Math.PI * 2,
                    layer
                });
            }
        }

        function createButterfly(randomPos = false) {
            const palette = ['#ffd6ec', '#e9d5ff', '#fde68a', '#fbcfe8', '#fed7aa'];
            return {
                x: randomPos ? Math.random() * width : (Math.random() < 0.5 ? -30 : width + 30),
                y: height * (0.45 + Math.random() * 0.42),
                vx: (Math.random() < 0.5 ? 1 : -1) * (22 + Math.random() * 24),
                vy: (Math.random() - 0.5) * 12,
                size: 6 + Math.random() * 4.5,
                flapPhase: Math.random() * Math.PI * 2,
                flapSpeed: 9 + Math.random() * 4,
                wanderPhase: Math.random() * Math.PI * 2,
                color: palette[Math.floor(Math.random() * palette.length)]
            };
        }

        function createFirefly(randomY = false) {
            const isBokeh = Math.random() < 0.26;
            return {
                x: Math.random() * width,
                y: randomY ? Math.random() * height : height + 20,
                vx: (Math.random() - 0.5) * 18,
                vy: -(9 + Math.random() * 20),
                size: isBokeh ? (9 + Math.random() * 15) : (1.8 + Math.random() * 2.5),
                isBokeh,
                alpha: 0.25 + Math.random() * 0.65,
                phase: Math.random() * Math.PI * 2,
                color: Math.random() < 0.5 ? '#fde68a' : (Math.random() < 0.5 ? '#ffd6ec' : '#e9d5ff')
            };
        }

        function spawnHeartBurst(x, y, count = 8, tint = '#ff69b4') {
            for (let i = 0; i < count; i++) {
                const angle = -Math.PI / 2 + (Math.random() - 0.5) * 2.2;
                const speed = 28 + Math.random() * 55;
                particles.push({
                    type: Math.random() < 0.6 ? 'heart' : 'sparkle',
                    x,
                    y,
                    vx: Math.cos(angle) * speed,
                    vy: Math.sin(angle) * speed - 12,
                    size: 4 + Math.random() * 4.5,
                    alpha: 1,
                    decay: 0.55 + Math.random() * 0.45,
                    rotation: (Math.random() - 0.5) * 0.6,
                    rotSpeed: (Math.random() - 0.5) * 2.5,
                    color: i % 3 === 0 ? '#fde68a' : tint
                });
            }
        }

        function drawHeart(x, y, size, color, alpha, rotation = 0) {
            ctx.save();
            ctx.translate(x, y);
            ctx.rotate(rotation);
            ctx.globalAlpha = Math.max(0, alpha);
            ctx.fillStyle = color;
            ctx.beginPath();
            const s = size * 0.55;
            ctx.moveTo(0, s * 0.45);
            ctx.bezierCurveTo(0, 0, -s, 0, -s, s * 0.45);
            ctx.bezierCurveTo(-s, s * 0.95, 0, s * 1.35, 0, s * 1.65);
            ctx.bezierCurveTo(0, s * 1.35, s, s * 0.95, s, s * 0.45);
            ctx.bezierCurveTo(s, 0, 0, 0, 0, s * 0.45);
            ctx.fill();
            ctx.restore();
        }

        function drawSparkle(x, y, size, color, alpha) {
            ctx.save();
            ctx.translate(x, y);
            ctx.globalAlpha = Math.max(0, alpha);
            ctx.fillStyle = color;
            ctx.beginPath();
            for (let i = 0; i < 4; i++) {
                const angle = (i * Math.PI) / 2;
                ctx.lineTo(Math.cos(angle) * size, Math.sin(angle) * size);
                ctx.lineTo(Math.cos(angle + Math.PI / 4) * (size * 0.28), Math.sin(angle + Math.PI / 4) * (size * 0.28));
            }
            ctx.closePath();
            ctx.fill();
            ctx.restore();
        }

        class FallingSeed {
            constructor(targetX, targetY, delay = 0, autoBloomOpts = null) {
                this.startX = targetX + (Math.random() - 0.5) * 90;
                this.startY = Math.max(-30, targetY - (140 + Math.random() * 140));
                this.targetX = targetX;
                this.targetY = targetY;
                this.x = this.startX;
                this.y = this.startY;
                this.delay = delay;
                this.elapsed = 0;
                this.duration = 0.85 + Math.random() * 0.35;
                this.done = false;
                this.autoBloomOpts = autoBloomOpts;
                this.trail = [];
            }

            update(dt) {
                if (this.delay > 0) {
                    this.delay -= dt;
                    return;
                }
                this.elapsed += dt;
                const t = Math.min(1, this.elapsed / this.duration);
                const ease = t * t * (3 - 2 * t);

                const sway = Math.sin(t * Math.PI * 2) * 14 * (1 - t);
                this.x = this.startX + (this.targetX - this.startX) * ease + sway;
                this.y = this.startY + (this.targetY - this.startY) * ease;

                this.trail.push({ x: this.x, y: this.y, alpha: 1 });
                if (this.trail.length > 10) this.trail.shift();
                for (let p of this.trail) p.alpha *= 0.86;

                if (t >= 1 && !this.done) {
                    this.done = true;
                    spawnHeartBurst(this.targetX, this.targetY, 6, '#ffd6ec');
                    flowers.push(new KittyFlower(this.targetX, this.targetY, this.autoBloomOpts));
                }
            }

            draw() {
                if (this.delay > 0 || this.done) return;
                for (let i = 0; i < this.trail.length; i++) {
                    const pt = this.trail[i];
                    const r = (i / this.trail.length) * 3.2;
                    ctx.save();
                    ctx.globalAlpha = pt.alpha * 0.65;
                    ctx.fillStyle = '#ffe4f3';
                    ctx.beginPath();
                    ctx.arc(pt.x, pt.y, r, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.restore();
                }

                ctx.save();
                ctx.shadowBlur = 14;
                ctx.shadowColor = '#ff85d0';
                ctx.fillStyle = '#ffffff';
                ctx.beginPath();
                ctx.arc(this.x, this.y, 4, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            }
        }

        class KittyFlower {
            constructor(x, baseY, opts = {}) {
                this.x = x;
                this.baseY = baseY;
                const isMobile = width < 640;
                const depthScale = opts.depthScale || (0.82 + Math.random() * 0.36);

                this.maxHeight = (opts.maxHeight || (isMobile ? (120 + Math.random() * 115) : (150 + Math.random() * 155))) * depthScale;
                this.headRadius = (opts.headRadius || (isMobile ? 23 : 28)) * depthScale;
                this.palette = opts.palette || BOW_PALETTES[Math.floor(Math.random() * BOW_PALETTES.length)];
                this.curveDir = opts.curveDir !== undefined ? opts.curveDir : (Math.random() < 0.5 ? -1 : 1);
                this.curveAmount = (14 + Math.random() * 22) * this.curveDir;

                this.age = 0;
                this.stemDuration = 1.35 + Math.random() * 0.45;
                this.bloomDelay = this.stemDuration * 0.72;
                this.bloomDuration = 1.05;
                this.bloomedBurstDone = false;

                this.swayPhase = Math.random() * Math.PI * 2;
                this.swaySpeed = 1.15 + Math.random() * 0.55;
                this.pointerOffset = 0;
                this.pointerVel = 0;

                this.leaves = [
                    { t: 0.30, side: -1, size: 22 * depthScale, angle: -0.65 },
                    { t: 0.56, side: 1, size: 25 * depthScale, angle: 0.62 }
                ];

                this.thorns = [
                    { t: 0.22, side: 1 },
                    { t: 0.44, side: -1 },
                    { t: 0.68, side: 1 }
                ];
            }

            update(dt, time) {
                this.age += dt;

                const stemProgress = Math.min(1, this.age / this.stemDuration);
                const easeStem = 1 - Math.pow(1 - stemProgress, 3);
                const currentH = this.maxHeight * easeStem;

                const tipX = this.x + this.curveAmount * easeStem + this.pointerOffset;
                const tipY = this.baseY - currentH;

                const dx = tipX - pointer.x;
                const dy = tipY - pointer.y;
                const dist = Math.hypot(dx, dy);
                const influenceRadius = 110;

                if (dist < influenceRadius && dist > 0.1) {
                    const force = (1 - dist / influenceRadius) * 28;
                    const pushDir = dx >= 0 ? 1 : -1;
                    this.pointerVel += pushDir * force * dt * 16;
                }

                this.pointerVel += -this.pointerOffset * 18 * dt;
                this.pointerVel *= Math.pow(0.06, dt);
                this.pointerOffset += this.pointerVel * dt;

                const bloomRaw = Math.max(0, (this.age - this.bloomDelay) / this.bloomDuration);
                if (bloomRaw >= 0.65 && !this.bloomedBurstDone) {
                    this.bloomedBurstDone = true;
                    spawnHeartBurst(tipX, tipY - this.headRadius * 0.3, 7, this.palette.bow);
                }
            }

            easeOutBack(x) {
                const c1 = 1.70158;
                const c3 = c1 + 1;
                return 1 + c3 * Math.pow(x - 1, 3) + c1 * Math.pow(x - 1, 2);
            }

            draw(time) {
                const stemProgress = Math.min(1, this.age / this.stemDuration);
                const easeStem = 1 - Math.pow(1 - stemProgress, 3);
                const currentH = this.maxHeight * easeStem;

                const wind = Math.sin(time * this.swaySpeed + this.swayPhase) * 9 * easeStem;
                const totalSway = wind + this.pointerOffset;

                const startX = this.x;
                const startY = this.baseY;
                const endX = this.x + this.curveAmount * easeStem + totalSway;
                const endY = this.baseY - currentH;
                const cpX = this.x + this.curveAmount * 0.25 * easeStem + totalSway * 0.35;
                const cpY = this.baseY - currentH * 0.52;

                // Tallo curvo con degradado verde esmeralda cálido
                const stemGrad = ctx.createLinearGradient(startX, startY, endX, endY);
                stemGrad.addColorStop(0, '#065f46');
                stemGrad.addColorStop(0.55, '#10b981');
                stemGrad.addColorStop(1, '#6ee7b7');

                ctx.save();
                ctx.strokeStyle = stemGrad;
                ctx.lineWidth = 4.2 * (this.headRadius / 26);
                ctx.lineCap = 'round';
                ctx.beginPath();
                ctx.moveTo(startX, startY);
                ctx.quadraticCurveTo(cpX, cpY, endX, endY);
                ctx.stroke();
                ctx.restore();

                // Pequeñas espinas suaves de rosal en el tallo
                if (this.palette.isRose) {
                    for (const thorn of this.thorns) {
                        if (easeStem > thorn.t + 0.08) {
                            const t = thorn.t / easeStem;
                            const tx = (1 - t) * (1 - t) * startX + 2 * (1 - t) * t * cpX + t * t * endX;
                            const ty = (1 - t) * (1 - t) * startY + 2 * (1 - t) * t * cpY + t * t * endY;
                            const thSize = 4.5 * (this.headRadius / 26);
                            ctx.save();
                            ctx.translate(tx, ty);
                            ctx.fillStyle = '#34d399';
                            ctx.beginPath();
                            ctx.moveTo(0, -thSize * 0.5);
                            ctx.lineTo(thorn.side * thSize * 1.4, -thSize * 0.15);
                            ctx.lineTo(0, thSize * 0.45);
                            ctx.closePath();
                            ctx.fill();
                            ctx.restore();
                        }
                    }
                }

                // Hojas laterales que brotan a lo largo del tallo
                for (const leaf of this.leaves) {
                    if (easeStem > leaf.t) {
                        const leafGrow = Math.min(1, (easeStem - leaf.t) / 0.28);
                        const leafEase = this.easeOutBack(leafGrow);
                        const t = leaf.t / easeStem;
                        const lx = (1 - t) * (1 - t) * startX + 2 * (1 - t) * t * cpX + t * t * endX;
                        const ly = (1 - t) * (1 - t) * startY + 2 * (1 - t) * t * cpY + t * t * endY;
                        const leafWind = Math.sin(time * 2.1 + this.swayPhase + leaf.t * 4) * 0.12;
                        this.drawLeaf(lx, ly, leaf.size * leafEase, leaf.angle + leafWind + totalSway * 0.008, leaf.side);
                    }
                }

                // Capullo y flor de Hello Kitty
                const bloomRaw = Math.max(0, Math.min(1, (this.age - this.bloomDelay) / this.bloomDuration));
                const headTilt = (totalSway * 0.011) + Math.sin(time * 1.6 + this.swayPhase) * 0.05;

                if (bloomRaw <= 0.15) {
                    const budScale = Math.min(1, stemProgress * 1.4);
                    this.drawBud(endX, endY, this.headRadius * 0.42 * budScale, headTilt);
                } else {
                    const openProgress = Math.min(1, (bloomRaw - 0.15) / 0.85);
                    const elasticScale = this.easeOutBack(openProgress);
                    this.drawKittyBlossom(endX, endY, this.headRadius * elasticScale, openProgress, headTilt);
                }
            }

            drawLeaf(x, y, size, angle, side) {
                if (size <= 0.5) return;
                ctx.save();
                ctx.translate(x, y);
                ctx.rotate(angle);

                const grad = ctx.createLinearGradient(0, 0, side * size * 1.4, -size * 0.4);
                grad.addColorStop(0, '#059669');
                grad.addColorStop(1, '#6ee7b7');

                ctx.fillStyle = grad;
                ctx.beginPath();
                ctx.moveTo(0, 0);
                ctx.quadraticCurveTo(side * size * 0.7, -size * 0.65, side * size * 1.45, -size * 0.12);
                ctx.quadraticCurveTo(side * size * 0.75, size * 0.45, 0, 0);
                ctx.fill();

                ctx.strokeStyle = 'rgba(209, 250, 229, 0.45)';
                ctx.lineWidth = 1;
                ctx.beginPath();
                ctx.moveTo(0, 0);
                ctx.quadraticCurveTo(side * size * 0.65, -size * 0.12, side * size * 1.25, -size * 0.1);
                ctx.stroke();

                ctx.restore();
            }

            drawBud(x, y, r, angle) {
                ctx.save();
                ctx.translate(x, y);
                ctx.rotate(angle);
                ctx.fillStyle = this.palette.outerPetal;
                ctx.beginPath();
                ctx.ellipse(0, -r * 0.6, r * 0.78, r * 1.18, 0, 0, Math.PI * 2);
                ctx.fill();

                ctx.fillStyle = '#10b981';
                ctx.beginPath();
                ctx.moveTo(-r * 0.75, 0);
                ctx.quadraticCurveTo(-r * 0.85, -r * 0.95, 0, -r * 0.42);
                ctx.quadraticCurveTo(r * 0.85, -r * 0.95, r * 0.75, 0);
                ctx.closePath();
                ctx.fill();
                ctx.restore();
            }

            drawKittyBlossom(x, y, r, openProgress, angle) {
                if (r <= 1) return;
                ctx.save();
                ctx.translate(x, y);
                ctx.rotate(angle);

                // Halo cálido detrás de la rosa/flor
                const glowGrad = ctx.createRadialGradient(0, -r * 0.1, r * 0.2, 0, -r * 0.1, r * 2.35);
                glowGrad.addColorStop(0, this.palette.glow);
                glowGrad.addColorStop(1, 'rgba(0, 0, 0, 0)');
                ctx.fillStyle = glowGrad;
                ctx.beginPath();
                ctx.arc(0, -r * 0.1, r * 2.35, 0, Math.PI * 2);
                ctx.fill();

                // Capa exterior de pétalos de rosa grandes y envolventes
                const outerCount = 10;
                const outerDist = r * (0.78 + 0.44 * openProgress);
                const outerRx = r * 0.52;
                const outerRy = r * (0.48 + 0.42 * openProgress);

                for (let i = 0; i < outerCount; i++) {
                    const pAngle = (i / outerCount) * Math.PI * 2;
                    ctx.save();
                    ctx.rotate(pAngle);
                    ctx.translate(0, -outerDist);

                    const pGrad = ctx.createLinearGradient(0, outerRy * 0.5, 0, -outerRy);
                    pGrad.addColorStop(0, this.palette.bow);
                    pGrad.addColorStop(0.55, this.palette.outerPetal);
                    pGrad.addColorStop(1, '#fff5fa');

                    ctx.fillStyle = pGrad;
                    ctx.beginPath();
                    ctx.moveTo(0, outerRy * 0.6);
                    ctx.bezierCurveTo(-outerRx * 1.25, outerRy * 0.3, -outerRx * 1.15, -outerRy * 0.95, 0, -outerRy);
                    ctx.bezierCurveTo(outerRx * 1.15, -outerRy * 0.95, outerRx * 1.25, outerRy * 0.3, 0, outerRy * 0.6);
                    ctx.fill();

                    ctx.strokeStyle = 'rgba(255, 255, 255, 0.42)';
                    ctx.lineWidth = 1;
                    ctx.stroke();
                    ctx.restore();
                }

                // Segunda capa interior de pétalos intercalados en tono pastel luminoso
                const innerCount = 8;
                const innerDist = r * (0.64 + 0.30 * openProgress);
                const innerRx = r * 0.44;
                const innerRy = r * (0.38 + 0.32 * openProgress);

                for (let i = 0; i < innerCount; i++) {
                    const pAngle = ((i + 0.5) / innerCount) * Math.PI * 2;
                    ctx.save();
                    ctx.rotate(pAngle);
                    ctx.translate(0, -innerDist);

                    const inGrad = ctx.createLinearGradient(0, innerRy * 0.4, 0, -innerRy);
                    inGrad.addColorStop(0, this.palette.outerPetal);
                    inGrad.addColorStop(0.65, this.palette.innerPetal);
                    inGrad.addColorStop(1, '#ffffff');

                    ctx.fillStyle = inGrad;
                    ctx.beginPath();
                    ctx.ellipse(0, 0, innerRx, innerRy, 0, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.restore();
                }

                // Cabeza icónica de Hello Kitty en el corazón de la flor
                const rx = r * 1.14;
                const ry = r * 0.89;

                ctx.save();
                ctx.fillStyle = '#ffffff';
                ctx.strokeStyle = '#6b2152';
                ctx.lineWidth = Math.max(1.4, r * 0.06);
                ctx.lineJoin = 'round';

                // Oreja izquierda
                ctx.beginPath();
                ctx.moveTo(-rx * 0.86, -ry * 0.35);
                ctx.quadraticCurveTo(-rx * 0.94, -ry * 1.18, -rx * 0.78, -ry * 1.24);
                ctx.quadraticCurveTo(-rx * 0.54, -ry * 1.16, -rx * 0.32, -ry * 0.86);
                ctx.closePath();
                ctx.fill();
                ctx.stroke();

                // Oreja derecha
                ctx.beginPath();
                ctx.moveTo(rx * 0.86, -ry * 0.35);
                ctx.quadraticCurveTo(rx * 0.94, -ry * 1.18, rx * 0.78, -ry * 1.24);
                ctx.quadraticCurveTo(rx * 0.54, -ry * 1.16, rx * 0.32, -ry * 0.86);
                ctx.closePath();
                ctx.fill();
                ctx.stroke();

                // Óvalo principal de la cara
                ctx.beginPath();
                ctx.ellipse(0, 0, rx, ry, 0, 0, Math.PI * 2);
                ctx.fill();
                ctx.stroke();
                ctx.restore();

                // Interior rosado suave en oreja izquierda
                ctx.fillStyle = '#ffd6ec';
                ctx.beginPath();
                ctx.moveTo(-rx * 0.76, -ry * 0.52);
                ctx.lineTo(-rx * 0.74, -ry * 1.04);
                ctx.lineTo(-rx * 0.42, -ry * 0.84);
                ctx.closePath();
                ctx.fill();

                // Mejillas sonrojadas rosa pastel
                ctx.fillStyle = 'rgba(255, 133, 192, 0.55)';
                ctx.beginPath();
                ctx.ellipse(-rx * 0.56, ry * 0.22, rx * 0.15, ry * 0.11, 0, 0, Math.PI * 2);
                ctx.ellipse(rx * 0.56, ry * 0.22, rx * 0.15, ry * 0.11, 0, 0, Math.PI * 2);
                ctx.fill();

                // Ojitos negros ovalados con destello blanco
                ctx.fillStyle = '#1f0b18';
                ctx.beginPath();
                ctx.ellipse(-rx * 0.44, ry * 0.06, rx * 0.075, ry * 0.13, 0, 0, Math.PI * 2);
                ctx.ellipse(rx * 0.44, ry * 0.06, rx * 0.075, ry * 0.13, 0, 0, Math.PI * 2);
                ctx.fill();

                ctx.fillStyle = '#ffffff';
                ctx.beginPath();
                ctx.arc(-rx * 0.46, ry * 0.01, r * 0.032, 0, Math.PI * 2);
                ctx.arc(rx * 0.42, ry * 0.01, r * 0.032, 0, Math.PI * 2);
                ctx.fill();

                // Naricita ovalada amarilla dorada
                ctx.fillStyle = '#facc15';
                ctx.strokeStyle = '#d97706';
                ctx.lineWidth = Math.max(1, r * 0.04);
                ctx.beginPath();
                ctx.ellipse(0, ry * 0.23, rx * 0.11, ry * 0.09, 0, 0, Math.PI * 2);
                ctx.fill();
                ctx.stroke();

                // Tres bigotes a cada lado
                ctx.strokeStyle = '#3b0d26';
                ctx.lineWidth = Math.max(1.3, r * 0.055);
                ctx.lineCap = 'round';

                const whiskerY = [-ry * 0.05, ry * 0.14, ry * 0.33];
                const whiskerTilt = [-0.14, 0, 0.14];

                for (let i = 0; i < 3; i++) {
                    // Izquierda
                    ctx.beginPath();
                    ctx.moveTo(-rx * 0.78, whiskerY[i]);
                    ctx.lineTo(-rx * 1.14, whiskerY[i] + whiskerTilt[i] * r);
                    ctx.stroke();

                    // Derecha
                    ctx.beginPath();
                    ctx.moveTo(rx * 0.78, whiskerY[i]);
                    ctx.lineTo(rx * 1.14, whiskerY[i] + whiskerTilt[i] * r);
                    ctx.stroke();
                }

                // Moño clásico en su oreja derecha (arriba a la derecha)
                ctx.save();
                ctx.translate(rx * 0.56, -ry * 0.76);
                ctx.rotate(0.42);

                const bowW = r * 0.36;
                const bowH = r * 0.28;

                ctx.fillStyle = this.palette.bow;
                ctx.strokeStyle = '#4c0528';
                ctx.lineWidth = Math.max(1.2, r * 0.05);

                // Ala izquierda del moño
                ctx.beginPath();
                ctx.moveTo(-bowW * 0.15, 0);
                ctx.bezierCurveTo(-bowW * 1.15, -bowH * 1.25, -bowW * 1.35, bowH * 1.25, -bowW * 0.15, 0);
                ctx.fill();
                ctx.stroke();

                // Ala derecha del moño
                ctx.beginPath();
                ctx.moveTo(bowW * 0.15, 0);
                ctx.bezierCurveTo(bowW * 1.15, -bowH * 1.25, bowW * 1.35, bowH * 1.25, bowW * 0.15, 0);
                ctx.fill();
                ctx.stroke();

                // Nudo central del moño
                ctx.fillStyle = this.palette.center;
                ctx.beginPath();
                ctx.arc(0, 0, r * 0.15, 0, Math.PI * 2);
                ctx.fill();
                ctx.stroke();

                ctx.restore();
                ctx.restore();
            }
        }

        function plantInitialGarden() {
            gardenStarted = true;
            resetBtn.classList.remove('initial-center');
            flowers.length = 0;
            seeds.length = 0;
            particles.length = 0;

            const isMobile = width < 640;
            const count = isMobile ? 7 : 12;
            const margin = width * 0.08;
            const usableW = width - margin * 2;

            for (let i = 0; i < count; i++) {
                const ratio = count === 1 ? 0.5 : i / (count - 1);
                const jitter = (Math.random() - 0.5) * (usableW / count) * 0.55;
                const x = Math.max(margin, Math.min(width - margin, margin + ratio * usableW + jitter));

                const hillLayer = i % 3;
                const baseY = getHillHeight(x, hillLayer) + (8 + Math.random() * 18);
                const depthScale = hillLayer === 0 ? 0.80 : (hillLayer === 1 ? 0.96 : 1.12);

                seeds.push(new FallingSeed(x, baseY, i * 0.24, {
                    depthScale,
                    palette: BOW_PALETTES[i % BOW_PALETTES.length]
                }));
            }
        }

        function drawKuromiSun(x, y, radius, time) {
            ctx.save();

            // 1. Gran aura atmosférica cálida y lila iluminando el cielo
            const outerAura = ctx.createRadialGradient(x, y, radius * 0.4, x, y, radius * 5.2);
            outerAura.addColorStop(0, 'rgba(255, 245, 220, 0.72)');
            outerAura.addColorStop(0.22, 'rgba(253, 224, 150, 0.36)');
            outerAura.addColorStop(0.52, 'rgba(244, 114, 182, 0.16)');
            outerAura.addColorStop(0.8, 'rgba(192, 132, 252, 0.06)');
            outerAura.addColorStop(1, 'rgba(0, 0, 0, 0)');
            ctx.fillStyle = outerAura;
            ctx.beginPath();
            ctx.arc(x, y, radius * 5.2, 0, Math.PI * 2);
            ctx.fill();

            // 2. Rayos solares giratorios dorados y rosa pastel
            const rayCount = 16;
            for (let i = 0; i < rayCount; i++) {
                const baseAngle = (i / rayCount) * Math.PI * 2 + time * 0.14;
                const isLong = i % 2 === 0;
                const rayLen = radius * (isLong ? 2.55 : 1.95) + Math.sin(time * 2.2 + i) * 7;
                const spread = isLong ? 0.11 : 0.08;

                const rayGrad = ctx.createRadialGradient(x, y, radius * 0.7, x, y, rayLen);
                rayGrad.addColorStop(0, isLong ? 'rgba(254, 240, 138, 0.58)' : 'rgba(251, 207, 232, 0.45)');
                rayGrad.addColorStop(1, 'rgba(254, 240, 138, 0)');

                ctx.fillStyle = rayGrad;
                ctx.beginPath();
                ctx.moveTo(x, y);
                ctx.lineTo(x + Math.cos(baseAngle - spread) * rayLen, y + Math.sin(baseAngle - spread) * rayLen);
                ctx.lineTo(x + Math.cos(baseAngle + spread) * rayLen, y + Math.sin(baseAngle + spread) * rayLen);
                ctx.closePath();
                ctx.fill();
            }

            // 3. Disco solar brillante detrás de Kuromi
            const sunDiscGrad = ctx.createRadialGradient(x, y, radius * 0.15, x, y, radius * 1.32);
            sunDiscGrad.addColorStop(0, '#fffbeb');
            sunDiscGrad.addColorStop(0.62, '#fde68a');
            sunDiscGrad.addColorStop(0.88, '#f9a8d4');
            sunDiscGrad.addColorStop(1, 'rgba(249, 168, 212, 0)');
            ctx.fillStyle = sunDiscGrad;
            ctx.beginPath();
            ctx.arc(x, y, radius * 1.32, 0, Math.PI * 2);
            ctx.fill();

            // 4. Cabecita de Kuromi dentro del Sol con suave flotación
            const bobY = Math.sin(time * 1.8) * 3.5;
            const tilt = Math.sin(time * 1.2) * 0.045;

            ctx.translate(x, y + bobY);
            ctx.rotate(tilt);

            const r = radius * 0.78;
            const rx = r * 1.16;
            const ry = r * 0.94;

            // Cuello de bufón lila con puntas y bolitas rosas debajo
            ctx.fillStyle = '#d8b4fe';
            ctx.strokeStyle = '#2e1065';
            ctx.lineWidth = 1.6;
            ctx.beginPath();
            ctx.moveTo(-rx * 0.68, ry * 0.78);
            ctx.lineTo(-rx * 0.36, ry * 1.15);
            ctx.lineTo(0, ry * 0.85);
            ctx.lineTo(rx * 0.36, ry * 1.15);
            ctx.lineTo(rx * 0.68, ry * 0.78);
            ctx.closePath();
            ctx.fill();
            ctx.stroke();

            // Bolitas rosas del cuello de bufón
            ctx.fillStyle = '#ff69b4';
            ctx.beginPath();
            ctx.arc(-rx * 0.36, ry * 1.18, r * 0.10, 0, Math.PI * 2);
            ctx.arc(rx * 0.36, ry * 1.18, r * 0.10, 0, Math.PI * 2);
            ctx.fill();

            // Orejas puntiagudas largas de la capucha de bufón de Kuromi
            const hoodGrad = ctx.createLinearGradient(0, -ry * 1.8, 0, ry);
            hoodGrad.addColorStop(0, '#3b1559');
            hoodGrad.addColorStop(0.55, '#240b38');
            hoodGrad.addColorStop(1, '#160624');

            ctx.fillStyle = hoodGrad;
            ctx.strokeStyle = '#f5d0fe';
            ctx.lineWidth = 1.8;

            // Oreja izquierda de bufón
            ctx.beginPath();
            ctx.moveTo(-rx * 0.76, -ry * 0.28);
            ctx.quadraticCurveTo(-rx * 1.25, -ry * 1.25, -rx * 1.18, -ry * 1.75);
            ctx.quadraticCurveTo(-rx * 0.55, -ry * 1.35, -rx * 0.25, -ry * 0.82);
            ctx.closePath();
            ctx.fill();
            ctx.stroke();

            // Oreja derecha de bufón
            ctx.beginPath();
            ctx.moveTo(rx * 0.76, -ry * 0.28);
            ctx.quadraticCurveTo(rx * 1.25, -ry * 1.25, rx * 1.18, -ry * 1.75);
            ctx.quadraticCurveTo(rx * 0.55, -ry * 1.35, rx * 0.25, -ry * 0.82);
            ctx.closePath();
            ctx.fill();
            ctx.stroke();

            // Pompones rosa brillante en las puntas de las orejas
            ctx.fillStyle = '#ff4da6';
            ctx.beginPath();
            ctx.arc(-rx * 1.20, -ry * 1.78, r * 0.16, 0, Math.PI * 2);
            ctx.arc(rx * 1.20, -ry * 1.78, r * 0.16, 0, Math.PI * 2);
            ctx.fill();
            ctx.stroke();

            // Capucha principal redondeada negra/morada oscura
            ctx.fillStyle = hoodGrad;
            ctx.beginPath();
            ctx.ellipse(0, -ry * 0.05, rx * 1.06, ry * 0.98, 0, 0, Math.PI * 2);
            ctx.fill();
            ctx.stroke();

            // Carita blanca de Kuromi con flequillo de pico en la frente
            ctx.fillStyle = '#ffffff';
            ctx.beginPath();
            ctx.moveTo(-rx * 0.82, ry * 0.10);
            ctx.quadraticCurveTo(-rx * 0.72, -ry * 0.34, -rx * 0.26, -ry * 0.26);
            ctx.lineTo(0, -ry * 0.10);
            ctx.lineTo(rx * 0.26, -ry * 0.26);
            ctx.quadraticCurveTo(rx * 0.72, -ry * 0.34, rx * 0.82, ry * 0.10);
            ctx.quadraticCurveTo(rx * 0.84, ry * 0.74, 0, ry * 0.76);
            ctx.quadraticCurveTo(-rx * 0.84, ry * 0.74, -rx * 0.82, ry * 0.10);
            ctx.closePath();
            ctx.fill();

            // Calaverita rosa kawaii en la frente de la capucha
            ctx.save();
            ctx.translate(0, -ry * 0.52);
            ctx.fillStyle = '#ff69b4';
            ctx.beginPath();
            ctx.ellipse(0, -r * 0.03, r * 0.17, r * 0.13, 0, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillRect(-r * 0.09, r * 0.06, r * 0.18, r * 0.08);
            // Ojitos de la calaverita
            ctx.fillStyle = '#240b38';
            ctx.beginPath();
            ctx.arc(-r * 0.06, -r * 0.02, r * 0.03, 0, Math.PI * 2);
            ctx.arc(r * 0.06, -r * 0.02, r * 0.03, 0, Math.PI * 2);
            ctx.fill();
            ctx.restore();

            // Mejillas sonrojadas rosa pastel
            ctx.fillStyle = 'rgba(255, 105, 180, 0.52)';
            ctx.beginPath();
            ctx.ellipse(-rx * 0.55, ry * 0.34, rx * 0.14, ry * 0.10, 0, 0, Math.PI * 2);
            ctx.ellipse(rx * 0.55, ry * 0.34, rx * 0.14, ry * 0.10, 0, 0, Math.PI * 2);
            ctx.fill();

            // Ojitos pícaros kawaii con pestañas
            ctx.fillStyle = '#1f092b';
            ctx.beginPath();
            ctx.ellipse(-rx * 0.40, ry * 0.18, rx * 0.095, ry * 0.15, -0.08, 0, Math.PI * 2);
            ctx.ellipse(rx * 0.40, ry * 0.18, rx * 0.095, ry * 0.15, 0.08, 0, Math.PI * 2);
            ctx.fill();

            // Pestañitas laterales
            ctx.strokeStyle = '#1f092b';
            ctx.lineWidth = Math.max(1.5, r * 0.045);
            ctx.lineCap = 'round';
            ctx.beginPath();
            ctx.moveTo(-rx * 0.47, ry * 0.10);
            ctx.lineTo(-rx * 0.58, ry * 0.05);
            ctx.moveTo(rx * 0.47, ry * 0.10);
            ctx.lineTo(rx * 0.58, ry * 0.05);
            ctx.stroke();

            // Destellos en los ojos
            ctx.fillStyle = '#ffffff';
            ctx.beginPath();
            ctx.arc(-rx * 0.42, ry * 0.12, r * 0.038, 0, Math.PI * 2);
            ctx.arc(rx * 0.38, ry * 0.12, r * 0.038, 0, Math.PI * 2);
            ctx.fill();

            // Naricita rosa tierna
            ctx.fillStyle = '#ff69b4';
            ctx.beginPath();
            ctx.ellipse(0, ry * 0.32, rx * 0.08, ry * 0.065, 0, 0, Math.PI * 2);
            ctx.fill();

            // Sonrisita traviesa en forma de "w"
            ctx.strokeStyle = '#1f092b';
            ctx.lineWidth = Math.max(1.5, r * 0.045);
            ctx.beginPath();
            ctx.moveTo(-rx * 0.16, ry * 0.44);
            ctx.quadraticCurveTo(-rx * 0.08, ry * 0.54, 0, ry * 0.45);
            ctx.quadraticCurveTo(rx * 0.08, ry * 0.54, rx * 0.16, ry * 0.44);
            ctx.stroke();

            ctx.restore();
        }

        function drawSkyAndLandscape(time) {
            // 1. Cielo de hora mágica: violeta aterciopelado -> lavanda -> rosa melocotón -> dorado suave
            const skyGrad = ctx.createLinearGradient(0, 0, 0, height * 0.76);
            skyGrad.addColorStop(0.0, '#22103b');
            skyGrad.addColorStop(0.28, '#4c266e');
            skyGrad.addColorStop(0.56, '#9d4d88');
            skyGrad.addColorStop(0.80, '#f4978e');
            skyGrad.addColorStop(1.0, '#fde68a');

            ctx.fillStyle = skyGrad;
            ctx.fillRect(0, 0, width, height);

            // Estrellitas tenues en la parte alta del cielo violeta
            for (const s of stars) {
                const twinkle = 0.25 + 0.75 * (0.5 + 0.5 * Math.sin(time * s.speed + s.phase));
                ctx.save();
                ctx.globalAlpha = twinkle * 0.75;
                ctx.fillStyle = '#fff5fa';
                ctx.beginPath();
                ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            }

            // Nubes pastel suaves flotando lentamente
            const cloudPositions = [
                { baseX: width * 0.15, y: height * 0.20, scale: 1.0, speed: 6 },
                { baseX: width * 0.52, y: height * 0.14, scale: 0.85, speed: 4.5 },
                { baseX: width * 0.84, y: height * 0.25, scale: 1.1, speed: 5.5 }
            ];
            for (const c of cloudPositions) {
                const cx = ((c.baseX + time * c.speed) % (width + 240)) - 120;
                ctx.save();
                ctx.translate(cx, c.y);
                ctx.scale(c.scale, c.scale);
                const cGrad = ctx.createLinearGradient(0, -25, 0, 20);
                cGrad.addColorStop(0, 'rgba(255, 232, 245, 0.34)');
                cGrad.addColorStop(1, 'rgba(253, 186, 116, 0.12)');
                ctx.fillStyle = cGrad;
                ctx.beginPath();
                ctx.arc(0, 0, 34, 0, Math.PI * 2);
                ctx.arc(28, -6, 28, 0, Math.PI * 2);
                ctx.arc(-28, -4, 26, 0, Math.PI * 2);
                ctx.arc(52, 4, 20, 0, Math.PI * 2);
                ctx.arc(-50, 5, 19, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            }

            // 2. Sol-Kuromi radiante en el cielo
            const isMobile = width < 640;
            const sunX = isMobile ? width * 0.5 : width * 0.24;
            const sunY = isMobile ? height * 0.21 : height * 0.22;
            const sunRadius = isMobile ? 42 : 54;
            drawKuromiSun(sunX, sunY, sunRadius, time);

            // 3. Cordilleras lejanas en tonos lavanda y violeta ahumado con neblina brillante
            const mtnLayers = [
                { top: 'rgba(109, 67, 145, 0.72)', bot: 'rgba(196, 140, 198, 0.45)', layer: 0 },
                { top: 'rgba(84, 52, 122, 0.85)', bot: 'rgba(168, 118, 178, 0.65)', layer: 1 }
            ];

            for (const m of mtnLayers) {
                const mGrad = ctx.createLinearGradient(0, height * 0.42, 0, height * 0.72);
                mGrad.addColorStop(0, m.top);
                mGrad.addColorStop(1, m.bot);

                ctx.save();
                ctx.beginPath();
                ctx.moveTo(0, height);
                for (let x = 0; x <= width; x += 16) {
                    ctx.lineTo(x, getMountainHeight(x, m.layer));
                }
                ctx.lineTo(width, height);
                ctx.closePath();
                ctx.fillStyle = mGrad;
                ctx.fill();
                ctx.restore();
            }

            // Neblina dorada-rosada al pie de las montañas
            const mistGrad = ctx.createLinearGradient(0, height * 0.56, 0, height * 0.74);
            mistGrad.addColorStop(0, 'rgba(254, 240, 138, 0)');
            mistGrad.addColorStop(0.5, 'rgba(254, 215, 170, 0.38)');
            mistGrad.addColorStop(1, 'rgba(255, 228, 243, 0.12)');
            ctx.fillStyle = mistGrad;
            ctx.fillRect(0, height * 0.56, width, height * 0.18);

            // 4. Lago cristalino reflejando el cielo dorado/rosa y el Sol-Kuromi
            const lakeTop = height * 0.66;
            const lakeBot = height * 0.79;
            const lakeGrad = ctx.createLinearGradient(0, lakeTop, 0, lakeBot);
            lakeGrad.addColorStop(0, '#f9a8d4');
            lakeGrad.addColorStop(0.35, '#c084fc');
            lakeGrad.addColorStop(0.75, '#4c1d95');
            lakeGrad.addColorStop(1, '#1e1b4b');
            ctx.fillStyle = lakeGrad;
            ctx.fillRect(0, lakeTop, width, lakeBot - lakeTop);

            // Reflejo dorado ondulante en el agua bajo el Sol-Kuromi
            ctx.save();
            for (let i = 0; i < 14; i++) {
                const ry = lakeTop + 4 + i * ((lakeBot - lakeTop) / 14);
                const waveW = (38 - i * 1.6) + Math.sin(time * 2.4 + i) * 10;
                const offsetX = Math.sin(time * 1.9 + i * 0.9) * 12;
                ctx.fillStyle = `rgba(254, 240, 138, ${0.42 - i * 0.022})`;
                ctx.beginPath();
                ctx.ellipse(sunX + offsetX, ry, Math.max(8, waveW), 2.2, 0, 0, Math.PI * 2);
                ctx.fill();
            }
            ctx.restore();

            // 5. Tres colinas verdes aterciopeladas bañadas por la luz dorada del atardecer
            const hillStyles = [
                { fillTop: '#15803d', fillMid: '#065f46', fillBot: '#022c22', rim: 'rgba(254, 240, 138, 0.45)' },
                { fillTop: '#166534', fillMid: '#064e3b', fillBot: '#022c22', rim: 'rgba(253, 186, 116, 0.38)' },
                { fillTop: '#14532d', fillMid: '#042f2e', fillBot: '#011815', rim: 'rgba(255, 200, 232, 0.35)' }
            ];

            for (let layer = 0; layer < 3; layer++) {
                const style = hillStyles[layer];
                const grad = ctx.createLinearGradient(0, height * 0.68, 0, height);
                grad.addColorStop(0, style.fillTop);
                grad.addColorStop(0.45, style.fillMid);
                grad.addColorStop(1, style.fillBot);

                ctx.save();
                ctx.beginPath();
                ctx.moveTo(0, height);
                for (let x = 0; x <= width; x += 16) {
                    ctx.lineTo(x, getHillHeight(x, layer));
                }
                ctx.lineTo(width, height);
                ctx.closePath();
                ctx.fillStyle = grad;
                ctx.fill();

                // Filo dorado iluminado por el Sol-Kuromi en la cresta del prado
                ctx.strokeStyle = style.rim;
                ctx.lineWidth = 2.4;
                ctx.beginPath();
                for (let x = 0; x <= width; x += 16) {
                    const y = getHillHeight(x, layer);
                    if (x === 0) ctx.moveTo(x, y);
                    else ctx.lineTo(x, y);
                }
                ctx.stroke();
                ctx.restore();
            }

            // 6. Briznas de pasto mecido por la brisa
            ctx.save();
            ctx.lineCap = 'round';
            for (const b of grassBlades) {
                const sway = Math.sin(time * 2.1 + b.phase) * 5 + b.lean * 6;
                ctx.strokeStyle = b.layer === 1 ? 'rgba(52, 211, 153, 0.42)' : 'rgba(110, 231, 183, 0.36)';
                ctx.lineWidth = 1.6;
                ctx.beginPath();
                ctx.moveTo(b.x, b.y);
                ctx.quadraticCurveTo(b.x + sway * 0.4, b.y - b.h * 0.55, b.x + sway, b.y - b.h);
                ctx.stroke();
            }
            ctx.restore();
        }

        function updateAndDrawButterflies(dt, time) {
            for (let i = 0; i < butterflies.length; i++) {
                const b = butterflies[i];
                b.x += b.vx * dt;
                b.y += (b.vy + Math.sin(time * 2.2 + b.wanderPhase) * 18) * dt;

                if (b.x < -50) b.x = width + 40;
                if (b.x > width + 50) b.x = -40;
                if (b.y < height * 0.32) b.vy = Math.abs(b.vy);
                if (b.y > height * 0.86) b.vy = -Math.abs(b.vy);

                const wingSpan = Math.abs(Math.sin(time * b.flapSpeed + b.flapPhase));
                const dir = b.vx >= 0 ? 1 : -1;

                ctx.save();
                ctx.translate(b.x, b.y);
                ctx.rotate(dir * 0.18);

                ctx.shadowBlur = 10;
                ctx.shadowColor = b.color;
                ctx.fillStyle = b.color;

                // Ala superior
                ctx.beginPath();
                ctx.ellipse(-dir * b.size * 0.2, -b.size * 0.35 * wingSpan, b.size * 0.85, Math.max(1.5, b.size * 0.65 * wingSpan), -0.3 * dir, 0, Math.PI * 2);
                ctx.fill();

                // Ala inferior
                ctx.beginPath();
                ctx.ellipse(-dir * b.size * 0.15, b.size * 0.25 * wingSpan, b.size * 0.65, Math.max(1.2, b.size * 0.48 * wingSpan), 0.3 * dir, 0, Math.PI * 2);
                ctx.fill();

                ctx.restore();
            }
        }

        function updateAndDrawFireflies(dt, time) {
            for (let i = 0; i < fireflies.length; i++) {
                const f = fireflies[i];
                f.x += (f.vx + Math.sin(time * 1.2 + f.phase) * 14) * dt;
                f.y += f.vy * dt;

                if (f.y < -30) {
                    fireflies[i] = createFirefly(false);
                    continue;
                }
                if (f.x < -30) f.x = width + 20;
                if (f.x > width + 30) f.x = -20;

                const pulse = 0.45 + 0.55 * Math.sin(time * 2.4 + f.phase);
                ctx.save();
                ctx.globalAlpha = f.alpha * pulse;
                if (f.isBokeh) {
                    const grad = ctx.createRadialGradient(f.x, f.y, 0, f.x, f.y, f.size);
                    grad.addColorStop(0, f.color);
                    grad.addColorStop(1, 'rgba(255, 255, 255, 0)');
                    ctx.fillStyle = grad;
                    ctx.beginPath();
                    ctx.arc(f.x, f.y, f.size, 0, Math.PI * 2);
                    ctx.fill();
                } else {
                    ctx.shadowBlur = 10;
                    ctx.shadowColor = f.color;
                    ctx.fillStyle = f.color;
                    ctx.beginPath();
                    ctx.arc(f.x, f.y, f.size, 0, Math.PI * 2);
                    ctx.fill();
                }
                ctx.restore();
            }
        }

        function updateAndDrawParticles(dt) {
            for (let i = particles.length - 1; i >= 0; i--) {
                const p = particles[i];
                p.x += p.vx * dt;
                p.y += p.vy * dt;
                p.vy -= 14 * dt;
                p.vx *= Math.pow(0.25, dt);
                p.rotation += p.rotSpeed * dt;
                p.alpha -= p.decay * dt;

                if (p.alpha <= 0) {
                    particles.splice(i, 1);
                    continue;
                }

                if (p.type === 'heart') {
                    drawHeart(p.x, p.y, p.size, p.color, p.alpha, p.rotation);
                } else {
                    drawSparkle(p.x, p.y, p.size * 0.75, p.color, p.alpha);
                }
            }
        }

        function plantAtLocation(clientX, clientY) {
            const minGroundY = getHillHeight(clientX, 0) + 4;
            const maxGroundY = height - 14;
            const targetY = Math.max(minGroundY, Math.min(maxGroundY, clientY));

            const depthRatio = (targetY - minGroundY) / Math.max(1, maxGroundY - minGroundY);
            const depthScale = 0.8 + depthRatio * 0.35;

            seeds.push(new FallingSeed(clientX, targetY, 0, { depthScale }));
            spawnHeartBurst(clientX, clientY, 5, '#ffb3d9');
        }

        let lastTrailTime = 0;
        canvas.addEventListener('pointermove', (e) => {
            pointer.x = e.clientX;
            pointer.y = e.clientY;
            pointer.active = true;

            const now = performance.now();
            if (now - lastTrailTime > 55) {
                lastTrailTime = now;
                particles.push({
                    type: Math.random() < 0.35 ? 'heart' : 'sparkle',
                    x: e.clientX + (Math.random() - 0.5) * 10,
                    y: e.clientY + (Math.random() - 0.5) * 10,
                    vx: (Math.random() - 0.5) * 16,
                    vy: -15 - Math.random() * 15,
                    size: 4 + Math.random() * 3,
                    alpha: 0.85,
                    decay: 1.1,
                    rotation: (Math.random() - 0.5) * 0.5,
                    rotSpeed: (Math.random() - 0.5) * 2,
                    color: Math.random() < 0.5 ? '#ffd6ec' : '#fde68a'
                });
            }
        });

        canvas.addEventListener('pointerdown', (e) => {
            pointer.x = e.clientX;
            pointer.y = e.clientY;
            pointer.active = true;
            if (!gardenStarted) return;
            plantAtLocation(e.clientX, e.clientY);
        });

        canvas.addEventListener('pointerleave', () => {
            pointer.active = false;
            pointer.x = -1000;
            pointer.y = -1000;
        });

        resetBtn.addEventListener('click', (e) => {
            e.stopPropagation();
            plantInitialGarden();
        });

        window.addEventListener('resize', resize);

        let lastTime = performance.now();
        function animate(now) {
            requestAnimationFrame(animate);

            const dt = Math.min(0.05, (now - lastTime) / 1000);
            lastTime = now;
            const time = now / 1000;

            drawSkyAndLandscape(time);
            updateAndDrawFireflies(dt, time);
            updateAndDrawButterflies(dt, time);

            // Actualizar y dibujar semillas cayendo
            for (let i = seeds.length - 1; i >= 0; i--) {
                seeds[i].update(dt);
                seeds[i].draw();
                if (seeds[i].done) {
                    seeds.splice(i, 1);
                }
            }

            // Ordenar flores por profundidad vertical (las más bajas van al frente)
            flowers.sort((a, b) => a.baseY - b.baseY);

            for (const flower of flowers) {
                flower.update(dt, time);
                flower.draw(time);
            }

            updateAndDrawParticles(dt);
        }

        window.onload = function () {
            resize();
            requestAnimationFrame(animate);
        };
    </script>
</body>
</html>
