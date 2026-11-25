
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Kitty Mágica ✨💖</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@500;600;700&family=Quicksand:wght@600;700&display=swap" rel="stylesheet">
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
            background: #2b0f3a;
            font-family: 'Fredoka', 'Quicksand', sans-serif;
        }

        #magicCanvas {
            display: block;
            width: 100%;
            height: 100%;
            cursor: pointer;
            touch-action: none;
        }
    </style>
</head>
<body>

    <canvas id="magicCanvas"></canvas>

    <script>
        const canvas = document.getElementById('magicCanvas');
        const ctx = canvas.getContext('2d');

        let width = 0;
        let height = 0;
        let dpr = 1;

        const CUTE_MESSAGES = [
            "linda",
            "nuvvvv",
            "la mas nubb",
            "prooo",
            "ñeñeñe",
            "hola",
            "malaaaa",
            "bonitaaaa",
            "que prroo la mas pro"
        ];

        let messageIndex = 0;

        const clouds = [];
        const stars = [];
        const sparkles = [];
        const floatingHearts = [];
        const speechBubbles = [];
        const ambientPetals = [];

        const pointer = {
            x: 0,
            y: 0,
            active: false
        };

        // Estado de la Kitty central interactiva
        const kitty = {
            x: 0,
            y: 0,
            baseX: 0,
            baseY: 0,
            scaleX: 1,
            scaleY: 1,
            velScaleX: 0,
            velScaleY: 0,
            tilt: 0,
            velTilt: 0,
            lookX: 0,
            lookY: 0,
            happyTimer: 0,
            blushBoost: 0,
            bowColor: '#ff3b8d',
            bowInner: '#ffb3d9',
            tapCount: 0,
            walkCycle: 0,
            facing: 1,
            wanderTimer: 1.5,
            wanderX: 0,
            wanderY: 0
        };

        const BOW_THEMES = [
            { bow: '#ff3b8d', inner: '#ffb3d9' },
            { bow: '#a855f7', inner: '#e9d5ff' },
            { bow: '#f43f5e', inner: '#fecdd3' },
            { bow: '#ec4899', inner: '#fbcfe8' },
            { bow: '#fb7185', inner: '#ffe4e6' },
            { bow: '#d946ef', inner: '#f5d0fe' }
        ];

        function resize() {
            dpr = Math.min(window.devicePixelRatio || 1, 2);
            width = window.innerWidth;
            height = window.innerHeight;
            canvas.width = width * dpr;
            canvas.height = height * dpr;
            ctx.setTransform(dpr, 0, 0, dpr, 0, 0);

            kitty.baseX = width * 0.5;
            kitty.baseY = height * 0.62;
            if (kitty.x === 0 && kitty.y === 0) {
                kitty.x = kitty.baseX;
                kitty.y = kitty.baseY;
                kitty.wanderX = kitty.baseX;
                kitty.wanderY = kitty.baseY;
            } else {
                kitty.x = Math.max(80, Math.min(width - 80, kitty.x));
                kitty.y = Math.max(height * 0.42, Math.min(height * 0.78, kitty.y));
                kitty.wanderX = kitty.x;
                kitty.wanderY = kitty.y;
            }

            initBackground();
        }

        function initBackground() {
            stars.length = 0;
            const starCount = Math.floor((width * height) / 11000);
            for (let i = 0; i < starCount; i++) {
                stars.push({
                    x: Math.random() * width,
                    y: Math.random() * height * 0.85,
                    r: 1.2 + Math.random() * 2.6,
                    phase: Math.random() * Math.PI * 2,
                    speed: 1.2 + Math.random() * 2.2,
                    isFourPoint: Math.random() < 0.35,
                    color: Math.random() < 0.5 ? '#fff5fa' : (Math.random() < 0.5 ? '#fde68a' : '#f5d0fe')
                });
            }

            clouds.length = 0;
            const cloudCount = width < 640 ? 5 : 8;
            for (let i = 0; i < cloudCount; i++) {
                clouds.push({
                    x: (i / cloudCount) * (width + 260) - 130,
                    y: height * (0.14 + (i % 3) * 0.16 + Math.random() * 0.06),
                    scale: (width < 640 ? 0.65 : 0.95) + Math.random() * 0.45,
                    speed: 8 + Math.random() * 10,
                    alpha: 0.45 + Math.random() * 0.35
                });
            }

            ambientPetals.length = 0;
            const petalCount = width < 640 ? 16 : 28;
            for (let i = 0; i < petalCount; i++) {
                ambientPetals.push({
                    x: Math.random() * width,
                    y: Math.random() * height,
                    vx: -10 - Math.random() * 15,
                    vy: 14 + Math.random() * 18,
                    size: 4 + Math.random() * 4.5,
                    rot: Math.random() * Math.PI * 2,
                    rotSpeed: (Math.random() - 0.5) * 2.5,
                    swayPhase: Math.random() * Math.PI * 2,
                    color: Math.random() < 0.5 ? 'rgba(255, 182, 217, 0.7)' : 'rgba(233, 213, 255, 0.7)'
                });
            }
        }

        function spawnCelebration(x, y, count = 18) {
            const palette = ['#ff4da6', '#ff85d0', '#fde68a', '#e879f9', '#ffffff', '#fb7185'];
            for (let i = 0; i < count; i++) {
                const angle = (Math.PI * 2 * i) / count + (Math.random() - 0.5) * 0.35;
                const speed = 55 + Math.random() * 135;
                floatingHearts.push({
                    x,
                    y,
                    vx: Math.cos(angle) * speed,
                    vy: Math.sin(angle) * speed - 35,
                    size: 7 + Math.random() * 9,
                    alpha: 1,
                    decay: 0.55 + Math.random() * 0.35,
                    rotation: (Math.random() - 0.5) * 0.8,
                    rotSpeed: (Math.random() - 0.5) * 3,
                    isHeart: Math.random() < 0.68,
                    color: palette[i % palette.length]
                });
            }
        }

        function spawnSpeechBubble(text) {
            // Mantener solo 1 burbuja corta y visible para no saturar la pantalla
            speechBubbles.length = 0;

            const isMobile = width < 640;
            const offsetX = (Math.random() - 0.5) * (isMobile ? 16 : 36);

            speechBubbles.push({
                text,
                x: kitty.x + offsetX,
                y: kitty.y - (isMobile ? 124 : 152),
                targetOffsetY: 0,
                currentOffsetY: 14,
                scale: 0.25,
                alpha: 1,
                age: 0,
                maxAge: 2.8
            });
        }

        function triggerKittyInteraction(tapX, tapY) {
            // Rebote elástico kawaii (squash & stretch)
            kitty.scaleX = 1.24;
            kitty.scaleY = 0.78;
            kitty.velTilt += (tapX < kitty.x ? -1 : 1) * 3.8;
            kitty.happyTimer = 2.6;
            kitty.blushBoost = 1.0;
            kitty.tapCount++;

            // Cambiar de moño cada 3 toques para darle variedad
            if (kitty.tapCount % 3 === 0) {
                const theme = BOW_THEMES[(kitty.tapCount / 3) % BOW_THEMES.length];
                kitty.bowColor = theme.bow;
                kitty.bowInner = theme.inner;
            }

            const msg = CUTE_MESSAGES[messageIndex % CUTE_MESSAGES.length];
            messageIndex++;
            spawnSpeechBubble(msg);
            spawnCelebration(tapX || kitty.x, tapY || kitty.y - 30, 22);
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

        function drawFourPointStar(x, y, size, color, alpha) {
            ctx.save();
            ctx.translate(x, y);
            ctx.globalAlpha = Math.max(0, alpha);
            ctx.fillStyle = color;
            ctx.beginPath();
            for (let i = 0; i < 4; i++) {
                const ang = (i * Math.PI) / 2;
                ctx.lineTo(Math.cos(ang) * size, Math.sin(ang) * size);
                ctx.lineTo(Math.cos(ang + Math.PI / 4) * (size * 0.28), Math.sin(ang + Math.PI / 4) * (size * 0.28));
            }
            ctx.closePath();
            ctx.fill();
            ctx.restore();
        }

        function drawBackground(time) {
            // Cielo mágico pastel crepuscular (morado suave -> rosa algodón -> durazno luminoso)
            const skyGrad = ctx.createLinearGradient(0, 0, 0, height);
            skyGrad.addColorStop(0.0, '#2b124c');
            skyGrad.addColorStop(0.32, '#5e2b7a');
            skyGrad.addColorStop(0.65, '#c46aa0');
            skyGrad.addColorStop(0.88, '#f8a5c2');
            skyGrad.addColorStop(1.0, '#ffe3ec');
            ctx.fillStyle = skyGrad;
            ctx.fillRect(0, 0, width, height);

            // Arcoíris pastel translúcido en el horizonte
            const rbX = width * 0.5;
            const rbY = height * 0.78;
            const baseR = Math.max(width, height) * 0.52;
            const rbColors = [
                'rgba(255, 133, 192, 0.16)',
                'rgba(253, 186, 116, 0.15)',
                'rgba(254, 240, 138, 0.16)',
                'rgba(167, 243, 208, 0.14)',
                'rgba(196, 181, 253, 0.16)'
            ];
            for (let i = 0; i < rbColors.length; i++) {
                const arcRadius = baseR - i * 16;
                if (arcRadius <= 0) continue;
                ctx.save();
                ctx.strokeStyle = rbColors[i];
                ctx.lineWidth = 18;
                ctx.beginPath();
                ctx.arc(rbX, rbY, arcRadius, Math.PI, 0, false);
                ctx.stroke();
                ctx.restore();
            }

            // Estrellas y destellos titilantes
            for (const s of stars) {
                const twinkle = 0.25 + 0.75 * (0.5 + 0.5 * Math.sin(time * s.speed + s.phase));
                if (s.isFourPoint) {
                    drawFourPointStar(s.x, s.y, s.r * 2.3, s.color, twinkle * 0.85);
                } else {
                    ctx.save();
                    ctx.globalAlpha = twinkle * 0.85;
                    ctx.fillStyle = s.color;
                    ctx.beginPath();
                    ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.restore();
                }
            }

            // Nubes esponjosas rosa pastel
            for (const c of clouds) {
                ctx.save();
                ctx.translate(c.x, c.y);
                ctx.scale(c.scale, c.scale);
                ctx.globalAlpha = c.alpha;

                const cGrad = ctx.createLinearGradient(0, -35, 0, 28);
                cGrad.addColorStop(0, '#fff5fa');
                cGrad.addColorStop(1, '#fbcfe8');
                ctx.fillStyle = cGrad;

                ctx.beginPath();
                ctx.arc(0, 0, 38, 0, Math.PI * 2);
                ctx.arc(34, -8, 32, 0, Math.PI * 2);
                ctx.arc(-34, -6, 30, 0, Math.PI * 2);
                ctx.arc(64, 4, 24, 0, Math.PI * 2);
                ctx.arc(-62, 5, 22, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            }

            // Colina suave de nubes/prado pastel en la parte inferior donde descansa Kitty
            const groundY = height * 0.75;
            const groundGrad = ctx.createLinearGradient(0, groundY - 30, 0, height);
            groundGrad.addColorStop(0, '#fbcfe8');
            groundGrad.addColorStop(0.5, '#f472b6');
            groundGrad.addColorStop(1, '#be185d');

            ctx.save();
            ctx.fillStyle = groundGrad;
            ctx.beginPath();
            ctx.moveTo(0, height);
            for (let x = 0; x <= width; x += 20) {
                const wave = Math.sin((x / width) * Math.PI) * 28 + Math.sin(x * 0.015 + time * 0.8) * 5;
                ctx.lineTo(x, groundY - wave);
            }
            ctx.lineTo(width, height);
            ctx.closePath();
            ctx.fill();

            // Borde luminoso en la colina
            ctx.strokeStyle = 'rgba(255, 255, 255, 0.65)';
            ctx.lineWidth = 3;
            ctx.beginPath();
            for (let x = 0; x <= width; x += 20) {
                const wave = Math.sin((x / width) * Math.PI) * 28 + Math.sin(x * 0.015 + time * 0.8) * 5;
                if (x === 0) ctx.moveTo(x, groundY - wave);
                else ctx.lineTo(x, groundY - wave);
            }
            ctx.stroke();
            ctx.restore();
        }

        function drawInteractiveKitty(time) {
            const isMobile = width < 640;
            const baseSize = isMobile ? 74 : 96;
            const walkBob = Math.abs(Math.sin(kitty.walkCycle)) * 7;
            const idleBob = Math.sin(time * 2.6) * 4;
            const bobY = walkBob + idleBob;

            ctx.save();
            ctx.translate(kitty.x, kitty.y + bobY);

            // Sombra suave debajo de Kitty
            ctx.save();
            ctx.translate(0, baseSize * 1.48 - bobY * 0.5);
            ctx.fillStyle = 'rgba(131, 24, 67, 0.26)';
            ctx.beginPath();
            ctx.ellipse(0, 0, baseSize * 0.85 * kitty.scaleX, baseSize * 0.2, 0, 0, Math.PI * 2);
            ctx.fill();
            ctx.restore();

            // Aura mágica detrás de Kitty
            const auraRadius = baseSize * (2.1 + Math.sin(time * 3) * 0.12);
            const auraGrad = ctx.createRadialGradient(0, 0, baseSize * 0.3, 0, 0, auraRadius);
            auraGrad.addColorStop(0, 'rgba(255, 255, 255, 0.55)');
            auraGrad.addColorStop(0.5, 'rgba(255, 133, 208, 0.28)');
            auraGrad.addColorStop(1, 'rgba(255, 133, 208, 0)');
            ctx.fillStyle = auraGrad;
            ctx.beginPath();
            ctx.arc(0, 0, auraRadius, 0, Math.PI * 2);
            ctx.fill();

            ctx.rotate(kitty.tilt + Math.sin(kitty.walkCycle) * 0.045);
            ctx.scale(kitty.scaleX, kitty.scaleY);

            // 1. Cuerpecito tierno con overol rosa pastel
            const bodyY = baseSize * 0.86;
            ctx.save();
            ctx.translate(0, bodyY);

            // Bracitos que balancean al caminar o saludan cuando está feliz
            const stepSwing = Math.sin(kitty.walkCycle) * 0.32;
            const waveAngle = kitty.happyTimer > 0
                ? Math.sin(time * 16) * 0.45 - 0.5
                : Math.sin(time * 2.5) * 0.08 + stepSwing;

            // Brazo izquierdo
            ctx.save();
            ctx.translate(-baseSize * 0.52, -baseSize * 0.12);
            ctx.rotate(waveAngle);
            ctx.fillStyle = '#ffffff';
            ctx.strokeStyle = '#581c46';
            ctx.lineWidth = 3.2;
            ctx.beginPath();
            ctx.ellipse(-baseSize * 0.14, 0, baseSize * 0.22, baseSize * 0.15, -0.3, 0, Math.PI * 2);
            ctx.fill();
            ctx.stroke();
            ctx.restore();

            // Brazo derecho
            ctx.save();
            ctx.translate(baseSize * 0.52, -baseSize * 0.12);
            ctx.rotate(-waveAngle);
            ctx.fillStyle = '#ffffff';
            ctx.strokeStyle = '#581c46';
            ctx.lineWidth = 3.2;
            ctx.beginPath();
            ctx.ellipse(baseSize * 0.14, 0, baseSize * 0.22, baseSize * 0.15, 0.3, 0, Math.PI * 2);
            ctx.fill();
            ctx.stroke();
            ctx.restore();

            // Patitas blancas que alternan al caminar
            const leftFootY = baseSize * 0.46 - Math.max(0, Math.sin(kitty.walkCycle)) * baseSize * 0.11;
            const rightFootY = baseSize * 0.46 - Math.max(0, -Math.sin(kitty.walkCycle)) * baseSize * 0.11;
            const leftFootX = -baseSize * 0.26 + Math.sin(kitty.walkCycle) * baseSize * 0.07;
            const rightFootX = baseSize * 0.26 - Math.sin(kitty.walkCycle) * baseSize * 0.07;

            ctx.fillStyle = '#ffffff';
            ctx.strokeStyle = '#581c46';
            ctx.lineWidth = 3.2;
            ctx.beginPath();
            ctx.ellipse(leftFootX, leftFootY, baseSize * 0.2, baseSize * 0.13, 0, 0, Math.PI * 2);
            ctx.ellipse(rightFootX, rightFootY, baseSize * 0.2, baseSize * 0.13, 0, 0, Math.PI * 2);
            ctx.fill();
            ctx.stroke();

            // Vestidito / overol rosa
            const dressGrad = ctx.createLinearGradient(0, -baseSize * 0.3, 0, baseSize * 0.42);
            dressGrad.addColorStop(0, kitty.bowInner);
            dressGrad.addColorStop(1, kitty.bowColor);
            ctx.fillStyle = dressGrad;
            ctx.beginPath();
            ctx.moveTo(-baseSize * 0.42, -baseSize * 0.22);
            ctx.lineTo(baseSize * 0.42, -baseSize * 0.22);
            ctx.lineTo(baseSize * 0.54, baseSize * 0.38);
            ctx.quadraticCurveTo(0, baseSize * 0.48, -baseSize * 0.54, baseSize * 0.38);
            ctx.closePath();
            ctx.fill();
            ctx.stroke();

            // Botoncitos amarillos del overol
            ctx.fillStyle = '#facc15';
            ctx.beginPath();
            ctx.arc(-baseSize * 0.22, -baseSize * 0.04, baseSize * 0.06, 0, Math.PI * 2);
            ctx.arc(baseSize * 0.22, -baseSize * 0.04, baseSize * 0.06, 0, Math.PI * 2);
            ctx.fill();

            ctx.restore();

            // 2. Cabeza grande de Hello Kitty (ligero paralaje mirando al cursor/dedo)
            ctx.save();
            ctx.translate(kitty.lookX * 0.45, kitty.lookY * 0.45);

            const rx = baseSize * 1.15;
            const ry = baseSize * 0.90;

            ctx.fillStyle = '#ffffff';
            ctx.strokeStyle = '#581c46';
            ctx.lineWidth = 3.6;
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

            // Interior rosado suave en oreja izquierda
            ctx.fillStyle = '#ffd6ec';
            ctx.beginPath();
            ctx.moveTo(-rx * 0.76, -ry * 0.52);
            ctx.lineTo(-rx * 0.74, -ry * 1.04);
            ctx.lineTo(-rx * 0.42, -ry * 0.84);
            ctx.closePath();
            ctx.fill();

            // Rasgos faciales con seguimiento sutil
            ctx.save();
            ctx.translate(kitty.lookX * 0.55, kitty.lookY * 0.55);

            // Mejillas sonrojadas que brillan más al tocarla
            const blushAlpha = 0.52 + kitty.blushBoost * 0.35;
            ctx.fillStyle = `rgba(255, 105, 180, ${blushAlpha})`;
            ctx.beginPath();
            ctx.ellipse(-rx * 0.56, ry * 0.22, rx * 0.16, ry * 0.11, 0, 0, Math.PI * 2);
            ctx.ellipse(rx * 0.56, ry * 0.22, rx * 0.16, ry * 0.11, 0, 0, Math.PI * 2);
            ctx.fill();

            // Ojitos: cambian a arcos felices "^ ^" cuando la tocas
            if (kitty.happyTimer > 0) {
                ctx.strokeStyle = '#24091b';
                ctx.lineWidth = 4;
                ctx.lineCap = 'round';
                ctx.beginPath();
                ctx.arc(-rx * 0.44, ry * 0.08, rx * 0.11, Math.PI * 1.1, Math.PI * 1.9);
                ctx.stroke();
                ctx.beginPath();
                ctx.arc(rx * 0.44, ry * 0.08, rx * 0.11, Math.PI * 1.1, Math.PI * 1.9);
                ctx.stroke();
            } else {
                ctx.fillStyle = '#24091b';
                ctx.beginPath();
                ctx.ellipse(-rx * 0.44, ry * 0.05, rx * 0.072, ry * 0.125, 0, 0, Math.PI * 2);
                ctx.ellipse(rx * 0.44, ry * 0.05, rx * 0.072, ry * 0.125, 0, 0, Math.PI * 2);
                ctx.fill();

                ctx.fillStyle = '#ffffff';
                ctx.beginPath();
                ctx.arc(-rx * 0.46, 0, baseSize * 0.032, 0, Math.PI * 2);
                ctx.arc(rx * 0.42, 0, baseSize * 0.032, 0, Math.PI * 2);
                ctx.fill();
            }

            // Naricita dorada
            ctx.fillStyle = '#facc15';
            ctx.strokeStyle = '#d97706';
            ctx.lineWidth = 2.2;
            ctx.beginPath();
            ctx.ellipse(0, ry * 0.23, rx * 0.105, ry * 0.085, 0, 0, Math.PI * 2);
            ctx.fill();
            ctx.stroke();

            // Boquita tierna cuando está feliz
            if (kitty.happyTimer > 0) {
                ctx.fillStyle = '#fb7185';
                ctx.beginPath();
                ctx.arc(0, ry * 0.37, rx * 0.085, 0, Math.PI, false);
                ctx.fill();
            }

            // Tres bigotes a cada lado
            ctx.strokeStyle = '#3b0d26';
            ctx.lineWidth = 3.2;
            ctx.lineCap = 'round';
            const whiskerY = [-ry * 0.05, ry * 0.14, ry * 0.33];
            const whiskerTilt = [-0.14, 0, 0.14];
            for (let i = 0; i < 3; i++) {
                ctx.beginPath();
                ctx.moveTo(-rx * 0.78, whiskerY[i]);
                ctx.lineTo(-rx * 1.14, whiskerY[i] + whiskerTilt[i] * baseSize);
                ctx.stroke();

                ctx.beginPath();
                ctx.moveTo(rx * 0.78, whiskerY[i]);
                ctx.lineTo(rx * 1.14, whiskerY[i] + whiskerTilt[i] * baseSize);
                ctx.stroke();
            }
            ctx.restore();

            // Moño grande en su oreja derecha
            ctx.save();
            ctx.translate(rx * 0.56, -ry * 0.76);
            ctx.rotate(0.42 + Math.sin(time * 3.2) * 0.04);

            const bowW = baseSize * 0.38;
            const bowH = baseSize * 0.30;

            ctx.fillStyle = kitty.bowColor;
            ctx.strokeStyle = '#4c0528';
            ctx.lineWidth = 3.2;

            ctx.beginPath();
            ctx.moveTo(-bowW * 0.15, 0);
            ctx.bezierCurveTo(-bowW * 1.15, -bowH * 1.25, -bowW * 1.35, bowH * 1.25, -bowW * 0.15, 0);
            ctx.fill();
            ctx.stroke();

            ctx.beginPath();
            ctx.moveTo(bowW * 0.15, 0);
            ctx.bezierCurveTo(bowW * 1.15, -bowH * 1.25, bowW * 1.35, bowH * 1.25, bowW * 0.15, 0);
            ctx.fill();
            ctx.stroke();

            ctx.fillStyle = kitty.bowInner;
            ctx.beginPath();
            ctx.arc(0, 0, baseSize * 0.16, 0, Math.PI * 2);
            ctx.fill();
            ctx.stroke();
            ctx.restore();

            ctx.restore();
            ctx.restore();
        }

        function drawSpeechBubbles() {
            const isMobile = width < 640;
            const fontSize = isMobile ? 16 : 20;
            ctx.font = `700 ${fontSize}px 'Fredoka', 'Quicksand', sans-serif`;
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';

            for (const b of speechBubbles) {
                if (b.alpha <= 0.02 || b.scale <= 0.05) continue;

                const drawX = Math.max(135, Math.min(width - 135, b.x));
                const drawY = Math.max(55, b.y + b.currentOffsetY);

                const textMetrics = ctx.measureText(b.text);
                const padX = isMobile ? 20 : 26;
                const boxW = Math.min(width - 32, textMetrics.width + padX * 2);
                const boxH = isMobile ? 46 : 52;
                const radius = boxH / 2;

                ctx.save();
                ctx.translate(drawX, drawY);
                ctx.scale(b.scale, b.scale);
                ctx.globalAlpha = Math.max(0, b.alpha);

                // Sombra rosada luminosa
                ctx.shadowBlur = 22;
                ctx.shadowColor = 'rgba(255, 77, 166, 0.55)';
                ctx.shadowOffsetY = 6;

                // Fondo de burbuja blanco cristalino
                ctx.fillStyle = 'rgba(255, 248, 252, 0.96)';
                ctx.beginPath();
                ctx.roundRect(-boxW / 2, -boxH / 2, boxW, boxH, radius);
                ctx.fill();

                ctx.shadowBlur = 0;
                ctx.shadowOffsetY = 0;
                ctx.strokeStyle = '#ff69b4';
                ctx.lineWidth = 2.8;
                ctx.stroke();

                // Piquito de diálogo apuntando hacia Kitty
                ctx.fillStyle = 'rgba(255, 248, 252, 0.96)';
                ctx.beginPath();
                ctx.moveTo(-9, boxH / 2 - 2);
                ctx.lineTo(0, boxH / 2 + 11);
                ctx.lineTo(9, boxH / 2 - 2);
                ctx.closePath();
                ctx.fill();

                ctx.strokeStyle = '#ff69b4';
                ctx.lineWidth = 2.6;
                ctx.beginPath();
                ctx.moveTo(-9, boxH / 2);
                ctx.lineTo(0, boxH / 2 + 11);
                ctx.lineTo(9, boxH / 2);
                ctx.stroke();

                // Texto de la burbuja
                ctx.fillStyle = '#6b1647';
                ctx.fillText(b.text, 0, 1);

                ctx.restore();
            }
        }

        function update(dt, time) {
            // Mover nubes suavemente
            for (const c of clouds) {
                c.x += c.speed * dt;
                if (c.x > width + 140) {
                    c.x = -140;
                }
            }

            // Mover pétalos ambientales
            for (const p of ambientPetals) {
                p.x += (p.vx + Math.sin(time * 1.4 + p.swayPhase) * 14) * dt;
                p.y += p.vy * dt;
                p.rot += p.rotSpeed * dt;
                if (p.y > height + 20) {
                    p.y = -20;
                    p.x = Math.random() * width;
                }
                if (p.x < -30) {
                    p.x = width + 20;
                }
            }

            // Física elástica de Kitty (caminata hacia el toque/paseo suave + rebote)
            const targetLookX = pointer.active ? Math.max(-14, Math.min(14, (pointer.x - kitty.x) * 0.04)) : 0;
            const targetLookY = pointer.active ? Math.max(-10, Math.min(10, (pointer.y - kitty.y) * 0.04)) : 0;
            kitty.lookX += (targetLookX - kitty.lookX) * Math.min(1, dt * 8);
            kitty.lookY += (targetLookY - kitty.lookY) * Math.min(1, dt * 8);

            // Paseo tranquilo cuando no hay toque activo
            kitty.wanderTimer -= dt;
            if (kitty.wanderTimer <= 0) {
                kitty.wanderTimer = 2.4 + Math.random() * 2.2;
                const marginX = Math.min(140, width * 0.22);
                kitty.wanderX = marginX + Math.random() * Math.max(80, width - marginX * 2);
                kitty.wanderY = height * (0.52 + Math.random() * 0.18);
            }

            const goalX = pointer.active
                ? Math.max(70, Math.min(width - 70, pointer.x))
                : kitty.wanderX;
            const goalY = pointer.active
                ? Math.max(height * 0.38, Math.min(height * 0.80, pointer.y))
                : kitty.wanderY;

            const dxGoal = goalX - kitty.x;
            const dyGoal = goalY - kitty.y;
            const distGoal = Math.hypot(dxGoal, dyGoal);

            if (distGoal > 6) {
                const walkSpeed = pointer.active ? 145 : 68;
                const step = Math.min(distGoal, walkSpeed * dt);
                kitty.x += (dxGoal / distGoal) * step;
                kitty.y += (dyGoal / distGoal) * step;
                kitty.walkCycle += dt * (pointer.active ? 14 : 8.5);
                if (Math.abs(dxGoal) > 4) {
                    kitty.facing = dxGoal >= 0 ? 1 : -1;
                }
            } else {
                // Caminata cortita en su lugar para mantener movimiento vivo
                kitty.walkCycle += dt * 2.2;
            }

            // Resorte para scaleX y scaleY
            const stiffness = 140;
            const damping = 10;
            kitty.velScaleX += (1 - kitty.scaleX) * stiffness * dt;
            kitty.velScaleX *= Math.exp(-damping * dt);
            kitty.scaleX += kitty.velScaleX * dt;

            kitty.velScaleY += (1 - kitty.scaleY) * stiffness * dt;
            kitty.velScaleY *= Math.exp(-damping * dt);
            kitty.scaleY += kitty.velScaleY * dt;

            // Resorte para inclinación
            kitty.velTilt += -kitty.tilt * 90 * dt;
            kitty.velTilt *= Math.exp(-8 * dt);
            kitty.tilt += kitty.velTilt * dt;

            if (kitty.happyTimer > 0) kitty.happyTimer = Math.max(0, kitty.happyTimer - dt);
            if (kitty.blushBoost > 0) kitty.blushBoost = Math.max(0, kitty.blushBoost - dt * 0.45);

            // Actualizar burbujas de diálogo
            for (let i = speechBubbles.length - 1; i >= 0; i--) {
                const b = speechBubbles[i];
                b.age += dt;
                b.scale += (1 - b.scale) * Math.min(1, dt * 12);
                b.currentOffsetY += (b.targetOffsetY - b.currentOffsetY) * Math.min(1, dt * 8);
                b.x = kitty.x;

                if (b.age > b.maxAge - 0.8) {
                    b.alpha -= dt * 1.3;
                }
                if (b.age >= b.maxAge || b.alpha <= 0) {
                    speechBubbles.splice(i, 1);
                }
            }

            // Actualizar corazones y chispas
            for (let i = floatingHearts.length - 1; i >= 0; i--) {
                const h = floatingHearts[i];
                h.x += h.vx * dt;
                h.y += h.vy * dt;
                h.vy -= 24 * dt;
                h.vx *= Math.pow(0.18, dt);
                h.rotation += h.rotSpeed * dt;
                h.alpha -= h.decay * dt;
                if (h.alpha <= 0) {
                    floatingHearts.splice(i, 1);
                }
            }

            for (let i = sparkles.length - 1; i >= 0; i--) {
                const sp = sparkles[i];
                sp.y -= 18 * dt;
                sp.alpha -= 1.35 * dt;
                if (sp.alpha <= 0) sparkles.splice(i, 1);
            }
        }

        function render(time) {
            drawBackground(time);

            // Pétalos flotantes de fondo
            for (const p of ambientPetals) {
                ctx.save();
                ctx.translate(p.x, p.y);
                ctx.rotate(p.rot);
                ctx.fillStyle = p.color;
                ctx.beginPath();
                ctx.ellipse(0, 0, p.size, p.size * 0.58, 0, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            }

            drawInteractiveKitty(time);
            drawSpeechBubbles();

            for (const h of floatingHearts) {
                if (h.isHeart) {
                    drawHeart(h.x, h.y, h.size, h.color, h.alpha, h.rotation);
                } else {
                    drawFourPointStar(h.x, h.y, h.size * 0.65, h.color, h.alpha);
                }
            }

            for (const sp of sparkles) {
                drawFourPointStar(sp.x, sp.y, sp.size, sp.color, sp.alpha);
            }
        }

        let lastSparkleTime = 0;
        canvas.addEventListener('pointermove', (e) => {
            pointer.x = e.clientX;
            pointer.y = e.clientY;
            pointer.active = true;

            const now = performance.now();
            if (now - lastSparkleTime > 45) {
                lastSparkleTime = now;
                sparkles.push({
                    x: e.clientX + (Math.random() - 0.5) * 12,
                    y: e.clientY + (Math.random() - 0.5) * 12,
                    size: 4 + Math.random() * 4,
                    alpha: 0.9,
                    color: Math.random() < 0.5 ? '#fff5fa' : '#fde68a'
                });
            }
        });

        canvas.addEventListener('pointerdown', (e) => {
            pointer.x = e.clientX;
            pointer.y = e.clientY;
            pointer.active = true;
            triggerKittyInteraction(e.clientX, e.clientY);
        });

        canvas.addEventListener('pointerleave', () => {
            pointer.active = false;
        });

        window.addEventListener('resize', resize);

        let lastTime = performance.now();
        function loop(now) {
            requestAnimationFrame(loop);
            const dt = Math.min(0.05, (now - lastTime) / 1000);
            lastTime = now;
            const time = now / 1000;

            update(dt, time);
            render(time);
        }

        window.onload = function () {
            resize();
            requestAnimationFrame(loop);
        };
    </script>
</body>
</html>
