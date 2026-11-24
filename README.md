[esme nuvv.html](https://github.com/user-attachments/files/35311128/esme.nuvv.html)
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>pa la mas nuvvv 💜</title>
    <!-- Tailwind CSS para diseño responsivo y utilidades -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Fuentes redondeadas y tiernas de Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@500;600;700&family=Quicksand:wght@600;700&display=swap" rel="stylesheet">
    
    <style>
        /* Configuración de fuentes y fondo base */
        * {
            box-sizing: border-box;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            font-family: 'Fredoka', 'Quicksand', sans-serif;
            /* Degradado nocturno en tonos morado profundo, violeta oscuro y índigo */
            background: radial-gradient(circle at 50% 35%, #2e104d 0%, #1a0730 55%, #0c0317 100%);
        }

        /* Canvas de fuegos artificiales en pantalla completa */
        #fireworksCanvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 10;
            pointer-events: auto;
        }

        /* Capa de gatitos flotantes en el fondo */
        #kittyLayer {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 5;
            pointer-events: none;
            overflow: hidden;
        }

        /* Animación de flotación suave para los gatitos SVG */
        .floating-kitty {
            position: absolute;
            will-change: transform;
            filter: drop-shadow(0 0 12px rgba(192, 132, 252, 0.4));
            animation: floatKitty var(--duration, 12s) ease-in-out infinite alternate;
        }

        @keyframes floatKitty {
            0% {
                transform: translate(0, 0) rotate(var(--rot-start, -8deg)) scale(var(--scale, 1));
            }
            50% {
                transform: translate(var(--tx-mid, 20px), var(--ty-mid, -30px)) rotate(var(--rot-mid, 5deg)) scale(calc(var(--scale, 1) * 1.04));
            }
            100% {
                transform: translate(var(--tx-end, -15px), var(--ty-end, -60px)) rotate(var(--rot-end, 10deg)) scale(var(--scale, 1));
            }
        }

        /* Contenedor central que permite hacer clic a través de él excepto en la tarjeta */
        .center-stage {
            position: fixed;
            inset: 0;
            z-index: 20;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 1.5rem;
            pointer-events: none;
        }

        /* Tarjeta principal con estilo Kawaii / Gamer */
        .kawaii-card {
            position: relative;
            pointer-events: auto;
            background: rgba(255, 240, 247, 0.92);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 4px solid #ff85c0;
            border-radius: 2.5rem;
            box-shadow: 
                0 0 35px rgba(255, 77, 166, 0.65),
                0 15px 35px rgba(0, 0, 0, 0.45),
                inset 0 0 20px rgba(255, 182, 217, 0.8);
        /* Tarjeta principal con estilo Kawaii en tonos morados/lila */
        .kawaii-card {
            position: relative;
            pointer-events: auto;
            background: rgba(243, 232, 255, 0.92);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 4px solid #c084fc;
            border-radius: 2.5rem;
            box-shadow: 
                0 0 35px rgba(168, 85, 247, 0.65),
                0 15px 35px rgba(0, 0, 0, 0.55),
                inset 0 0 20px rgba(216, 180, 254, 0.85);
            transition: transform 0.25s cubic-bezier(0.34, 1.56, 0.64, 1);
            animation: cardBreath 3.5s ease-in-out infinite;
            cursor: pointer;
        }

        .kawaii-card:hover {
            transform: scale(1.04) translateY(-4px);
        }

        .kawaii-card:active {
            transform: scale(0.96);
        }

        @keyframes cardBreath {
            0%, 100% {
                box-shadow: 
                    0 0 30px rgba(168, 85, 247, 0.6),
                    0 15px 35px rgba(0, 0, 0, 0.55),
                    inset 0 0 18px rgba(216, 180, 254, 0.75);
            }
            50% {
                box-shadow: 
                    0 0 55px rgba(192, 132, 252, 0.95),
                    0 0 85px rgba(233, 213, 255, 0.45),
                    0 18px 40px rgba(0, 0, 0, 0.6),
                    inset 0 0 25px rgba(216, 180, 254, 0.95);
            }
        }

        /* Orejitas de gatito superiores en la tarjeta */
        .ear-left, .ear-right {
            position: absolute;
            top: -32px;
            width: 58px;
            height: 54px;
            background: #f3e8ff;
            border: 4px solid #c084fc;
            z-index: -1;
        }

        .ear-left {
            left: 26px;
            border-radius: 65% 25% 10% 10%;
            transform: rotate(-16deg);
        }

        .ear-right {
            right: 26px;
            border-radius: 25% 65% 10% 10%;
            transform: rotate(16deg);
        }

        /* Interior lila de las orejitas */
        .ear-left::after, .ear-right::after {
            content: '';
            position: absolute;
            top: 9px;
            width: 28px;
            height: 26px;
            background: #d8b4fe;
            border-radius: 50% 50% 15% 15%;
        }

        .ear-left::after { left: 9px; }
        .ear-right::after { right: 9px; }

        /* Texto principal "pa la mas nuvvv" con brillo violeta neón */
        .neon-kawaii-text {
            background: linear-gradient(135deg, #7e22ce 0%, #a855f7 50%, #6b21a8 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            filter: drop-shadow(0 2px 10px rgba(168, 85, 247, 0.45));
            animation: textPulse 2.2s ease-in-out infinite;
        }

        @keyframes textPulse {
            0%, 100% {
                transform: scale(1);
                filter: drop-shadow(0 2px 8px rgba(168, 85, 247, 0.4));
            }
            50% {
                transform: scale(1.03);
                filter: drop-shadow(0 0 18px rgba(192, 132, 252, 0.85));
            }
        }

        /* Estrellitas parpadeantes en el fondo */
        .twinkle-star {
            position: absolute;
            border-radius: 50%;
            background: #f3e8ff;
            animation: twinkle var(--twinkle-dur, 3s) ease-in-out infinite;
        }

        @keyframes twinkle {
            0%, 100% { opacity: 0.2; transform: scale(0.8); }
            50% { opacity: 0.95; transform: scale(1.3); }
        }
    </style>
</head>
<body>

    <!-- Capa de fondo con estrellitas y gatitos estilo Hello Kitty flotando -->
    <div id="kittyLayer" aria-hidden="true"></div>

    <!-- Canvas interactivo para fuegos artificiales en tonos rosas, magenta, dorado y pastel -->
    <canvas id="fireworksCanvas"></canvas>

    <!-- Escenario central con la tarjeta principal -->
    <main class="center-stage">
        <div id="mainCard" class="kawaii-card px-8 py-8 sm:px-14 sm:py-11 max-w-[92vw] sm:max-w-xl text-center">
            <!-- Orejitas de gatito -->
            <div class="ear-left"></div>
            <div class="ear-right"></div>

            <!-- Moño morado clásico en la oreja derecha -->
            <div class="absolute -top-7 right-5 sm:right-6 w-16 h-12 pointer-events-none transform rotate-12 drop-shadow-md">
                <svg viewBox="0 0 100 70" class="w-full h-full">
                    <!-- Ala izquierda del moño -->
                    <path d="M45 35 C20 5, 2 15, 8 35 C2 55, 20 65, 45 35 Z" fill="#9333ea" stroke="#4c1d95" stroke-width="4"/>
                    <!-- Ala derecha del moño -->
                    <path d="M55 35 C80 5, 98 15, 92 35 C98 55, 80 65, 55 35 Z" fill="#9333ea" stroke="#4c1d95" stroke-width="4"/>
                    <!-- Pliegues internos -->
                    <path d="M36 35 C24 23, 18 28, 18 35 C18 42, 24 47, 36 35 Z" fill="#c084fc"/>
                    <path d="M64 35 C76 23, 82 28, 82 35 C82 42, 76 47, 64 35 Z" fill="#c084fc"/>
                    <!-- Centro del moño -->
                    <ellipse cx="50" cy="35" rx="12" ry="13" fill="#a855f7" stroke="#4c1d95" stroke-width="4"/>
                </svg>
            </div>

            <!-- Texto principal único solicitado en el centro -->
            <h1 class="neon-kawaii-text text-4xl sm:text-6xl font-bold tracking-tight leading-tight">
                pa la mas nuvvv
            </h1>
        </div>
    </main>

    <script>
        /**
         * 1. GENERACIÓN DE GATITOS ESTILO HELLO KITTY Y ESTRELLAS EN EL FONDO
         */
        const kittyLayer = document.getElementById('kittyLayer');

        // Plantilla SVG de gatito blanco tierno con moño morado/lila y bigotes
        function createKittySVG(bowColor = '#9333ea', innerBow = '#d8b4fe') {
            return `
                <svg viewBox="0 0 140 120" class="w-full h-full overflow-visible">
                    <!-- Oreja izquierda -->
                    <path d="M28 48 L22 14 L52 32 Z" fill="#ffffff" stroke="#d8b4fe" stroke-width="3.5" stroke-linejoin="round"/>
                    <!-- Oreja derecha -->
                    <path d="M112 48 L118 14 L88 32 Z" fill="#ffffff" stroke="#d8b4fe" stroke-width="3.5" stroke-linejoin="round"/>
                    
                    <!-- Cabeza ovalada -->
                    <ellipse cx="70" cy="68" rx="48" ry="38" fill="#ffffff" stroke="#d8b4fe" stroke-width="3.5"/>
                    
                    <!-- Mejillas sonrojadas en tono lila -->
                    <ellipse cx="42" cy="76" rx="7" ry="4.5" fill="#e9d5ff" opacity="0.9"/>
                    <ellipse cx="98" cy="76" rx="7" ry="4.5" fill="#e9d5ff" opacity="0.9"/>
                    
                    <!-- Ojitos oscuros ovalados -->
                    <ellipse cx="48" cy="67" rx="4.2" ry="5.8" fill="#240b36"/>
                    <ellipse cx="92" cy="67" rx="4.2" ry="5.8" fill="#240b36"/>
                    <!-- Brillo en los ojos -->
                    <circle cx="46.5" cy="65" r="1.5" fill="#ffffff"/>
                    <circle cx="90.5" cy="65" r="1.5" fill="#ffffff"/>
                    
                    <!-- Naricita amarilla -->
                    <ellipse cx="70" cy="74" rx="5.2" ry="3.6" fill="#ffd13b" stroke="#e09f00" stroke-width="1.5"/>
                    
                    <!-- Bigotes izquierdos -->
                    <line x1="12" y1="62" x2="29" y2="65" stroke="#4c1d95" stroke-width="2.8" stroke-linecap="round"/>
                    <line x1="10" y1="73" x2="28" y2="73" stroke="#4c1d95" stroke-width="2.8" stroke-linecap="round"/>
                    <line x1="14" y1="84" x2="30" y2="80" stroke="#4c1d95" stroke-width="2.8" stroke-linecap="round"/>
                    
                    <!-- Bigotes derechos -->
                    <line x1="128" y1="62" x2="111" y2="65" stroke="#4c1d95" stroke-width="2.8" stroke-linecap="round"/>
                    <line x1="130" y1="73" x2="112" y2="73" stroke="#4c1d95" stroke-width="2.8" stroke-linecap="round"/>
                    <line x1="126" y1="84" x2="110" y2="80" stroke="#4c1d95" stroke-width="2.8" stroke-linecap="round"/>
                    
                    <!-- Moño icónico en la oreja derecha -->
                    <g transform="translate(96, 26) rotate(22)">
                        <path d="M-4 0 C-20 -16, -30 -6, -24 4 C-30 14, -20 24, -4 8 Z" fill="${bowColor}" stroke="#3b0764" stroke-width="2.5"/>
                        <path d="M4 0 C20 -16, 30 -6, 24 4 C30 14, 20 24, 4 8 Z" fill="${bowColor}" stroke="#3b0764" stroke-width="2.5"/>
                        <circle cx="0" cy="4" r="7" fill="${innerBow}" stroke="#3b0764" stroke-width="2.5"/>
                    </g>
                </svg>
            `;
        }

        function populateBackground() {
            kittyLayer.innerHTML = '';
            const isMobile = window.innerWidth < 640;
            const kittyCount = isMobile ? 12 : 22;
            const starCount = isMobile ? 35 : 65;

            // Crear estrellitas titilantes
            for (let i = 0; i < starCount; i++) {
                const star = document.createElement('div');
                star.className = 'twinkle-star';
                const size = Math.random() * 3.5 + 1.5;
                star.style.width = `${size}px`;
                star.style.height = `${size}px`;
                star.style.left = `${Math.random() * 100}%`;
                star.style.top = `${Math.random() * 100}%`;
                star.style.setProperty('--twinkle-dur', `${Math.random() * 3 + 2}s`);
                star.style.animationDelay = `${Math.random() * 4}s`;
                kittyLayer.appendChild(star);
            }

            // Paleta de moños morados y lilas
            const bowPalettes = [
                ['#9333ea', '#d8b4fe'],
                ['#a855f7', '#e9d5ff'],
                ['#7e22ce', '#c084fc'],
                ['#c084fc', '#f3e8ff']
            ];

            // Distribuir gatitos en cuadrícula con desplazamiento aleatorio
            const cols = isMobile ? 3 : 5;
            const rows = Math.ceil(kittyCount / cols);

            for (let i = 0; i < kittyCount; i++) {
                const col = i % cols;
                const row = Math.floor(i / cols);

                const kitty = document.createElement('div');
                kitty.className = 'floating-kitty';

                const size = isMobile ? (Math.random() * 28 + 46) : (Math.random() * 38 + 56);
                kitty.style.width = `${size}px`;
                kitty.style.height = `${size * 0.85}px`;

                const cellW = 100 / cols;
                const cellH = 100 / rows;
                const left = col * cellW + Math.random() * (cellW * 0.65);
                const top = row * cellH + Math.random() * (cellH * 0.65);

                kitty.style.left = `${left}%`;
                kitty.style.top = `${top}%`;
                kitty.style.opacity = (Math.random() * 0.35 + 0.45).toFixed(2);

                // Variables CSS personalizadas para movimiento orgánico
                const duration = (Math.random() * 7 + 8).toFixed(1);
                kitty.style.setProperty('--duration', `${duration}s`);
                kitty.style.setProperty('--rot-start', `${(Math.random() * 24 - 12).toFixed(0)}deg`);
                kitty.style.setProperty('--rot-mid', `${(Math.random() * 24 - 12).toFixed(0)}deg`);
                kitty.style.setProperty('--rot-end', `${(Math.random() * 24 - 12).toFixed(0)}deg`);
                kitty.style.setProperty('--tx-mid', `${(Math.random() * 40 - 20).toFixed(0)}px`);
                kitty.style.setProperty('--ty-mid', `${(Math.random() * 40 - 20).toFixed(0)}px`);
                kitty.style.setProperty('--tx-end', `${(Math.random() * 50 - 25).toFixed(0)}px`);
                kitty.style.setProperty('--ty-end', `${(Math.random() * 50 - 25).toFixed(0)}px`);
                kitty.style.animationDelay = `-${(Math.random() * 10).toFixed(1)}s`;

                const palette = bowPalettes[i % bowPalettes.length];
                kitty.innerHTML = createKittySVG(palette[0], palette[1]);
                kittyLayer.appendChild(kitty);
            }
        }

        /**
         * 2. MOTOR DE FUEGOS ARTIFICIALES INTERACTIVOS EN CANVAS
         */
        const canvas = document.getElementById('fireworksCanvas');
        const ctx = canvas.getContext('2d');

        let width = canvas.width = window.innerWidth;
        let height = canvas.height = window.innerHeight;

        window.addEventListener('resize', () => {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
            populateBackground();
        });

        // Paleta de colores morados, violetas, lila, blanco y dorado
        const fireworkColors = [
            '#a855f7', // Violeta neón
            '#c084fc', // Lila brillante
            '#e9d5ff', // Lavanda claro
            '#9333ea', // Púrpura intenso
            '#ffd700', // Dorado chispa
            '#ffffff', // Blanco puro
            '#d946ef', // Fucsia violeta
            '#8b5cf6'  // Índigo violeta
        ];

        const rockets = [];
        const particles = [];

        class Rocket {
            constructor(startX, startY, targetX, targetY, forceHeart = false) {
                this.x = startX;
                this.y = startY;
                this.targetX = targetX;
                this.targetY = targetY;
                this.color = fireworkColors[Math.floor(Math.random() * fireworkColors.length)];
                this.forceHeart = forceHeart;
                
                // Calcular velocidad para alcanzar el objetivo suavemente
                const angle = Math.atan2(targetY - startY, targetX - startX);
                const distance = Math.hypot(targetX - startX, targetY - startY);
                const speed = Math.max(7, Math.min(14, distance * 0.025));
                
                this.vx = Math.cos(angle) * speed;
                this.vy = Math.sin(angle) * speed;
                this.trail = [];
                this.exploded = false;
            }

            update() {
                this.trail.push({ x: this.x, y: this.y });
                if (this.trail.length > 6) this.trail.shift();

                this.x += this.vx;
                this.y += this.vy;

                // Ligera desaceleración
                this.vx *= 0.985;
                this.vy *= 0.985;

                const distToTarget = Math.hypot(this.targetX - this.x, this.targetY - this.y);
                if (distToTarget < 18 || (Math.abs(this.vx) < 1.2 && Math.abs(this.vy) < 1.2)) {
                    this.explode();
                    this.exploded = true;
                }
            }

            draw() {
                ctx.save();
                for (let i = 0; i < this.trail.length; i++) {
                    const point = this.trail[i];
                    const alpha = (i + 1) / this.trail.length;
                    ctx.beginPath();
                    ctx.arc(point.x, point.y, 2.5 * alpha, 0, Math.PI * 2);
                    ctx.fillStyle = this.color;
                    ctx.globalAlpha = alpha * 0.8;
                    ctx.fill();
                }
                ctx.restore();
            }

            explode() {
                // 35% de probabilidad (o forzado al tocar) de explosión en forma de corazón
                const isHeart = this.forceHeart || Math.random() < 0.35;
                const secondaryColor = fireworkColors[Math.floor(Math.random() * fireworkColors.length)];

                if (isHeart) {
                    const count = 48;
                    const scale = Math.random() * 0.14 + 0.16;
                    for (let i = 0; i < count; i++) {
                        const t = (Math.PI * 2 * i) / count;
                        // Ecuación paramétrica de un corazón
                        const hx = 16 * Math.pow(Math.sin(t), 3);
                        const hy = -(13 * Math.cos(t) - 5 * Math.cos(2 * t) - 2 * Math.cos(3 * t) - Math.cos(4 * t));
                        
                        const color = i % 2 === 0 ? this.color : secondaryColor;
                        particles.push(new Particle(this.x, this.y, hx * scale, hy * scale, color, true));
                    }
                } else {
                    const count = 55;
                    for (let i = 0; i < count; i++) {
                        const angle = (Math.PI * 2 * i) / count + (Math.random() * 0.15);
                        const speed = Math.random() * 4.5 + 1.8;
                        const vx = Math.cos(angle) * speed;
                        const vy = Math.sin(angle) * speed;
                        const color = i % 3 === 0 ? '#ffd700' : (i % 2 === 0 ? this.color : secondaryColor);
                        particles.push(new Particle(this.x, this.y, vx, vy, color, false));
                    }
                }
            }
        }

        class Particle {
            constructor(x, y, vx, vy, color, isHeartPart) {
                this.x = x;
                this.y = y;
                this.vx = vx;
                this.vy = vy;
                this.color = color;
                this.alpha = 1;
                this.decay = Math.random() * 0.014 + 0.012;
                this.gravity = isHeartPart ? 0.025 : 0.055;
                this.friction = 0.96;
                this.size = Math.random() * 2.5 + 1.8;
                this.sparkle = Math.random() < 0.4;
            }

            update() {
                this.vx *= this.friction;
                this.vy *= this.friction;
                this.vy += this.gravity;
                this.x += this.vx;
                this.y += this.vy;
                this.alpha -= this.decay;
            }

            draw() {
                if (this.alpha <= 0) return;
                ctx.save();
                ctx.globalAlpha = Math.max(0, this.alpha);
                
                // Efecto de destello intermitente
                if (this.sparkle && Math.random() < 0.3) {
                    ctx.fillStyle = '#ffffff';
                } else {
                    ctx.fillStyle = this.color;
                }

                ctx.shadowBlur = 10;
                ctx.shadowColor = this.color;

                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            }
        }

        // Lanzar un cohete hacia coordenadas específicas
        function launchFirework(targetX, targetY, forceHeart = false) {
            const startX = width * (0.2 + Math.random() * 0.6);
            const startY = height + 10;
            rockets.push(new Rocket(startX, startY, targetX, targetY, forceHeart));
        }

        // Bucle principal de animación
        let lastAutoLaunch = 0;
        function animate(timestamp) {
            requestAnimationFrame(animate);

            // Limpiar con estela suave para dar efecto de brillo prolongado
            ctx.globalCompositeOperation = 'destination-out';
            ctx.fillStyle = 'rgba(0, 0, 0, 0.22)';
            ctx.fillRect(0, 0, width, height);
            ctx.globalCompositeOperation = 'lighter';

            // Lanzamiento automático periódico
            if (timestamp - lastAutoLaunch > 850) {
                const rx = width * (0.12 + Math.random() * 0.76);
                const ry = height * (0.12 + Math.random() * 0.55);
                launchFirework(rx, ry, false);
                lastAutoLaunch = timestamp;
            }

            // Actualizar y dibujar cohetes
            for (let i = rockets.length - 1; i >= 0; i--) {
                rockets[i].update();
                rockets[i].draw();
                if (rockets[i].exploded) {
                    rockets.splice(i, 1);
                }
            }

            // Actualizar y dibujar partículas
            for (let i = particles.length - 1; i >= 0; i--) {
                particles[i].update();
                particles[i].draw();
                if (particles[i].alpha <= 0) {
                    particles.splice(i, 1);
                }
            }
        }

        /**
         * 3. INTERACTIVIDAD AL HACER CLIC O TOCAR LA PANTALLA
         */
        function handleInteraction(clientX, clientY) {
            // Lanza un fuego artificial directo donde tocó el usuario
            launchFirework(clientX, clientY, true);
            // Lanza un segundo fuego artificial de acompañamiento cerca
            const offsetX = clientX + (Math.random() * 140 - 70);
            const offsetY = Math.max(60, clientY - (Math.random() * 80 + 20));
            setTimeout(() => launchFirework(offsetX, offsetY, false), 120);
        }

        canvas.addEventListener('pointerdown', (e) => {
            handleInteraction(e.clientX, e.clientY);
        });

        // Al tocar la tarjeta central también lanza una ráfaga celebratoria alrededor
        const mainCard = document.getElementById('mainCard');
        mainCard.addEventListener('click', (e) => {
            const rect = mainCard.getBoundingClientRect();
            const centerX = rect.left + rect.width / 2;
            const centerY = rect.top + rect.height / 2;

            launchFirework(centerX - 110, Math.max(80, centerY - 130), true);
            setTimeout(() => launchFirework(centerX + 110, Math.max(80, centerY - 130), true), 150);
            setTimeout(() => launchFirework(centerX, Math.max(60, centerY - 170), false), 300);
        });

        // Inicialización al cargar la página
        window.onload = function () {
            populateBackground();
            // Ráfaga inicial de bienvenida
            setTimeout(() => launchFirework(width * 0.28, height * 0.28, true), 200);
            setTimeout(() => launchFirework(width * 0.72, height * 0.28, true), 450);
            setTimeout(() => launchFirework(width * 0.5, height * 0.2, false), 700);
            requestAnimationFrame(animate);
        };
    </script>
</body>
</html>
