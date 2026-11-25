<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Para la más pro ✨</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@600;800&family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">

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
            background: radial-gradient(circle at 50% 45%, #1f0618 0%, #12030e 55%, #090208 100%);
            font-family: 'Plus Jakarta Sans', sans-serif;
            color: #fff5fa;
        }

        #scene-container {
            position: fixed;
            inset: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
            cursor: grab;
        }

        #scene-container:active {
            cursor: grabbing;
        }

        #labels-layer {
            position: fixed;
            inset: 0;
            z-index: 5;
            pointer-events: none;
            overflow: hidden;
        }

        .planet-label {
            position: absolute;
            top: 0;
            left: 0;
            transform: translate3d(-50%, -100%, 0) scale(0.7);
            opacity: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            will-change: transform, opacity;
            transition: opacity 0.45s ease;
        }

        .planet-connector {
            width: 1px;
            height: 14px;
            background: linear-gradient(to bottom, rgba(255, 133, 208, 0.85), rgba(255, 133, 208, 0.05));
            margin-top: 3px;
        }

        .quality-badge {
            padding: 5px 14px;
            border-radius: 999px;
            background: rgba(31, 6, 24, 0.68);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 105, 180, 0.55);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.65), 0 0 18px rgba(255, 59, 154, 0.42);
            color: #ffe4f3;
            font-size: 0.78rem;
            font-weight: 600;
            letter-spacing: 0.02em;
            white-space: nowrap;
            text-shadow: 0 1px 8px rgba(255, 105, 180, 0.75);
            transition: border-color 0.3s ease, box-shadow 0.3s ease, transform 0.3s ease;
        }

        .planet-label.active-focus .quality-badge {
            border-color: rgba(253, 230, 138, 0.9);
            box-shadow: 0 0 26px rgba(255, 59, 154, 0.78), 0 0 14px rgba(253, 230, 138, 0.55);
            transform: scale(1.08);
        }

        /* Mensaje final cinematográfico */
        #climax-overlay {
            position: fixed;
            inset: 0;
            z-index: 10;
            pointer-events: none;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 1.5rem;
            opacity: 0;
            transform: scale(0.92);
            transition: opacity 2.2s cubic-bezier(0.16, 1, 0.3, 1), transform 2.2s cubic-bezier(0.16, 1, 0.3, 1);
        }

        #climax-overlay.show {
            opacity: 1;
            transform: scale(1);
        }

        .climax-title {
            font-family: 'Cinzel', serif;
            font-weight: 800;
            text-align: center;
            line-height: 1.15;
            background: linear-gradient(135deg, #ffffff 0%, #ffd6ec 30%, #ff7ac6 65%, #fde68a 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            filter: drop-shadow(0 0 28px rgba(255, 59, 154, 0.85)) drop-shadow(0 4px 18px rgba(15, 2, 12, 0.95));
            animation: gentleHover 5s ease-in-out infinite;
        }

        @keyframes gentleHover {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-7px); }
        }

        /* Botón discreto en esquina */
        #replay-btn {
            position: fixed;
            bottom: 1.25rem;
            right: 1.25rem;
            z-index: 20;
            padding: 0.45rem 0.95rem;
            border-radius: 999px;
            background: rgba(31, 6, 24, 0.55);
            backdrop-filter: blur(8px);
            -webkit-backdrop-filter: blur(8px);
            border: 1px solid rgba(255, 133, 208, 0.32);
            color: rgba(255, 228, 243, 0.82);
            font-size: 0.75rem;
            font-weight: 500;
            letter-spacing: 0.06em;
            cursor: pointer;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.8s ease, background 0.25s ease, color 0.25s ease, border-color 0.25s ease;
        }

        #replay-btn.visible {
            opacity: 1;
            pointer-events: auto;
        }

        #replay-btn:hover {
            background: rgba(157, 23, 77, 0.65);
            border-color: rgba(255, 154, 213, 0.7);
            color: #ffffff;
        }
    </style>
</head>
<body>

    <div id="scene-container"></div>
    <div id="labels-layer"></div>

    <div id="climax-overlay">
        <h1 class="climax-title text-3xl sm:text-5xl md:text-6xl tracking-wide max-w-4xl">
            te quiero proo la mas pro
        </h1>
    </div>

    <button id="replay-btn" type="button">Repetir</button>

    <script>
        const PLANETS_DATA = [
            {
                quality: 'por nuub',
                orbitRadius: 15.5,
                size: 1.18,
                speed: 0.52,
                startAngle: 0.3,
                headTint: 0xffffff,
                bowColor: 0xff3b9a,
                bowCenter: 0xffb3da,
                atmosphere: 0xff85d0
            },
            {
                quality: 'por linda',
                orbitRadius: 22.5,
                size: 1.52,
                speed: 0.41,
                startAngle: 2.1,
                headTint: 0xfff5fa,
                bowColor: 0xff69b4,
                bowCenter: 0xffe4f3,
                atmosphere: 0xff9ad5
            },
            {
                quality: 'por ñeñeñe',
                orbitRadius: 30.5,
                size: 1.72,
                speed: 0.34,
                startAngle: 4.0,
                headTint: 0xffffff,
                bowColor: 0xe11d48,
                bowCenter: 0xfda4af,
                atmosphere: 0xff66b2,
                hasHeartMoon: true
            },
            {
                quality: 'por hacerme reír siempre',
                orbitRadius: 39,
                size: 1.42,
                speed: 0.28,
                startAngle: 1.1,
                headTint: 0xfff0f7,
                bowColor: 0xd946ef,
                bowCenter: 0xf5d0fe,
                atmosphere: 0xf472b6
            },
            {
                quality: 'por tu vibra única',
                orbitRadius: 51.5,
                size: 2.65,
                speed: 0.20,
                startAngle: 5.1,
                headTint: 0xffffff,
                bowColor: 0xc026d3,
                bowCenter: 0xf0abfc,
                atmosphere: 0xff9ad5,
                hasStarCrown: true
            },
            {
                quality: 'por ser tan auténtica',
                orbitRadius: 66.5,
                size: 2.35,
                speed: 0.15,
                startAngle: 3.0,
                headTint: 0xfff7fb,
                bowColor: 0xf43f5e,
                bowCenter: 0xfde68a,
                atmosphere: 0xfde68a,
                hasRings: true
            },
            {
                quality: 'por estar siempre ahí',
                orbitRadius: 80.5,
                size: 1.95,
                speed: 0.11,
                startAngle: 0.8,
                headTint: 0xfdf2f8,
                bowColor: 0xa855f7,
                bowCenter: 0xe9d5ff,
                atmosphere: 0xf0abfc
            },
            {
                quality: 'por brillar sin intentarlo',
                orbitRadius: 93.5,
                size: 1.9,
                speed: 0.085,
                startAngle: 4.6,
                headTint: 0xffffff,
                bowColor: 0xff1493,
                bowCenter: 0xfde68a,
                atmosphere: 0xff85d0
            }
        ];

        const container = document.getElementById('scene-container');
        const labelsLayer = document.getElementById('labels-layer');
        const climaxOverlay = document.getElementById('climax-overlay');
        const replayBtn = document.getElementById('replay-btn');

        const scene = new THREE.Scene();
        scene.fog = new THREE.FogExp2(0x12030e, 0.0021);

        const camera = new THREE.PerspectiveCamera(52, window.innerWidth / window.innerHeight, 0.1, 1200);
        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true, powerPreference: 'high-performance' });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        renderer.toneMapping = THREE.ACESFilmicToneMapping;
        renderer.toneMappingExposure = 1.18;
        container.appendChild(renderer.domElement);

        function createRadialGlowTexture(innerColor, midColor, outerColor) {
            const canvas = document.createElement('canvas');
            canvas.width = 256;
            canvas.height = 256;
            const ctx = canvas.getContext('2d');
            const grad = ctx.createRadialGradient(128, 128, 0, 128, 128, 128);
            grad.addColorStop(0, innerColor);
            grad.addColorStop(0.36, midColor);
            grad.addColorStop(1, outerColor);
            ctx.fillStyle = grad;
            ctx.fillRect(0, 0, 256, 256);
            return new THREE.CanvasTexture(canvas);
        }

        function createRingTexture() {
            const canvas = document.createElement('canvas');
            canvas.width = 512;
            canvas.height = 64;
            const ctx = canvas.getContext('2d');
            const grad = ctx.createLinearGradient(0, 0, 512, 0);
            grad.addColorStop(0.0, 'rgba(255, 154, 213, 0.0)');
            grad.addColorStop(0.18, 'rgba(255, 105, 180, 0.6)');
            grad.addColorStop(0.35, 'rgba(253, 230, 138, 0.88)');
            grad.addColorStop(0.48, 'rgba(31, 6, 24, 0.12)');
            grad.addColorStop(0.58, 'rgba(255, 228, 243, 0.85)');
            grad.addColorStop(0.82, 'rgba(255, 59, 154, 0.55)');
            grad.addColorStop(1.0, 'rgba(255, 154, 213, 0.0)');
            ctx.fillStyle = grad;
            ctx.fillRect(0, 0, 512, 64);
            return new THREE.CanvasTexture(canvas);
        }

        const glowSpriteTex = createRadialGlowTexture(
            'rgba(255, 255, 255, 1)',
            'rgba(255, 133, 208, 0.52)',
            'rgba(0, 0, 0, 0)'
        );

        const nebulaSpriteTex = createRadialGlowTexture(
            'rgba(255, 59, 154, 0.26)',
            'rgba(131, 24, 67, 0.12)',
            'rgba(0, 0, 0, 0)'
        );

        // Constructor de modelos 3D estilo Hello Kitty
        function createKitty3DModel(options = {}) {
            const {
                scale = 1,
                headColor = 0xffffff,
                bowColor = 0xff3b9a,
                bowCenterColor = 0xffb3da,
                isSun = false
            } = options;

            const kitty = new THREE.Group();

            const headMat = isSun
                ? new THREE.MeshBasicMaterial({ color: 0xfff8fc })
                : new THREE.MeshStandardMaterial({
                    color: headColor,
                    emissive: 0xff85d0,
                    emissiveIntensity: 0.14,
                    roughness: 0.38,
                    metalness: 0.05
                });

            const innerEarMat = isSun
                ? new THREE.MeshBasicMaterial({ color: 0xffb3da })
                : new THREE.MeshStandardMaterial({
                    color: 0xff9ad5,
                    emissive: 0xff69b4,
                    emissiveIntensity: 0.18,
                    roughness: 0.45
                });

            const bowMat = isSun
                ? new THREE.MeshBasicMaterial({ color: 0xfde68a })
                : new THREE.MeshStandardMaterial({
                    color: bowColor,
                    emissive: bowColor,
                    emissiveIntensity: 0.22,
                    roughness: 0.32,
                    metalness: 0.12
                });

            const bowCenterMat = isSun
                ? new THREE.MeshBasicMaterial({ color: 0xff69b4 })
                : new THREE.MeshStandardMaterial({
                    color: bowCenterColor,
                    emissive: bowCenterColor,
                    emissiveIntensity: 0.2,
                    roughness: 0.3
                });

            const eyeMat = new THREE.MeshStandardMaterial({
                color: 0x1a0512,
                roughness: 0.15,
                metalness: 0.2
            });

            const noseMat = new THREE.MeshStandardMaterial({
                color: 0xfacc15,
                emissive: 0xf59e0b,
                emissiveIntensity: 0.25,
                roughness: 0.3
            });

            const whiskerMat = new THREE.MeshStandardMaterial({
                color: 0x3b0724,
                roughness: 0.4
            });

            // 1. Cabeza elipsoidal suave
            const headGeo = new THREE.SphereGeometry(1, 32, 28);
            const headMesh = new THREE.Mesh(headGeo, headMat);
            headMesh.scale.set(1.24, 0.96, 0.92);
            kitty.add(headMesh);

            // 2. Dos orejitas redondeadas en la parte superior
            const earGeo = new THREE.ConeGeometry(0.36, 0.68, 24);
            const innerEarGeo = new THREE.ConeGeometry(0.2, 0.42, 20);

            const leftEar = new THREE.Mesh(earGeo, headMat);
            leftEar.position.set(-0.74, 0.86, 0.02);
            leftEar.rotation.z = 0.44;
            kitty.add(leftEar);

            const leftInnerEar = new THREE.Mesh(innerEarGeo, innerEarMat);
            leftInnerEar.position.set(-0.72, 0.84, 0.14);
            leftInnerEar.rotation.z = 0.44;
            kitty.add(leftInnerEar);

            const rightEar = new THREE.Mesh(earGeo, headMat);
            rightEar.position.set(0.74, 0.86, 0.02);
            rightEar.rotation.z = -0.44;
            kitty.add(rightEar);

            const rightInnerEar = new THREE.Mesh(innerEarGeo, innerEarMat);
            rightInnerEar.position.set(0.72, 0.84, 0.14);
            rightInnerEar.rotation.z = -0.44;
            kitty.add(rightInnerEar);

            // 3. Ojitos ovalados negros brillantes y reflejo
            const eyeGeo = new THREE.SphereGeometry(0.11, 16, 16);
            const shineGeo = new THREE.SphereGeometry(0.032, 10, 10);
            const shineMat = new THREE.MeshBasicMaterial({ color: 0xffffff });

            const leftEye = new THREE.Mesh(eyeGeo, eyeMat);
            leftEye.scale.set(0.85, 1.32, 0.55);
            leftEye.position.set(-0.46, -0.04, 0.87);
            kitty.add(leftEye);

            const leftShine = new THREE.Mesh(shineGeo, shineMat);
            leftShine.position.set(-0.48, 0.01, 0.93);
            kitty.add(leftShine);

            const rightEye = new THREE.Mesh(eyeGeo, eyeMat);
            rightEye.scale.set(0.85, 1.32, 0.55);
            rightEye.position.set(0.46, -0.04, 0.87);
            kitty.add(rightEye);

            const rightShine = new THREE.Mesh(shineGeo, shineMat);
            rightShine.position.set(0.44, 0.01, 0.93);
            kitty.add(rightShine);

            // 4. Naricita amarilla/dorada ovalada
            const noseMesh = new THREE.Mesh(new THREE.SphereGeometry(0.115, 16, 16), noseMat);
            noseMesh.scale.set(1.32, 0.86, 0.65);
            noseMesh.position.set(0, -0.18, 0.91);
            kitty.add(noseMesh);

            // 5. Mejillas sonrojadas rosa pastel
            const blushGeo = new THREE.SphereGeometry(0.13, 14, 14);
            const blushMat = new THREE.MeshBasicMaterial({
                color: 0xff85d0,
                transparent: true,
                opacity: 0.65
            });
            const leftBlush = new THREE.Mesh(blushGeo, blushMat);
            leftBlush.scale.set(1.35, 0.78, 0.35);
            leftBlush.position.set(-0.66, -0.22, 0.76);
            kitty.add(leftBlush);

            const rightBlush = new THREE.Mesh(blushGeo, blushMat);
            rightBlush.scale.set(1.35, 0.78, 0.35);
            rightBlush.position.set(0.66, -0.22, 0.76);
            kitty.add(rightBlush);

            // 6. Seis bigotes finos a los lados
            const whiskerGeo = new THREE.CylinderGeometry(0.018, 0.018, 0.42, 8);
            const whiskerAngles = [0.22, 0, -0.22];
            const whiskerY = [-0.04, -0.17, -0.30];

            for (let i = 0; i < 3; i++) {
                const lw = new THREE.Mesh(whiskerGeo, whiskerMat);
                lw.position.set(-1.12, whiskerY[i], 0.46);
                lw.rotation.z = Math.PI / 2 + whiskerAngles[i];
                lw.rotation.y = -0.35;
                kitty.add(lw);

                const rw = new THREE.Mesh(whiskerGeo, whiskerMat);
                rw.position.set(1.12, whiskerY[i], 0.46);
                rw.rotation.z = Math.PI / 2 - whiskerAngles[i];
                rw.rotation.y = 0.35;
                kitty.add(rw);
            }

            // 7. Moño 3D característico en la oreja izquierda (lado derecho al ver de frente)
            const bowGroup = new THREE.Group();
            bowGroup.position.set(0.68, 0.74, 0.42);
            bowGroup.rotation.set(-0.15, 0.25, -0.42);

            const wingGeo = new THREE.SphereGeometry(0.24, 18, 16);
            const leftWing = new THREE.Mesh(wingGeo, bowMat);
            leftWing.scale.set(1.25, 1.0, 0.62);
            leftWing.position.set(-0.22, 0, 0);
            bowGroup.add(leftWing);

            const rightWing = new THREE.Mesh(wingGeo, bowMat);
            rightWing.scale.set(1.25, 1.0, 0.62);
            rightWing.position.set(0.22, 0, 0);
            bowGroup.add(rightWing);

            const knot = new THREE.Mesh(new THREE.SphereGeometry(0.14, 16, 16), bowCenterMat);
            knot.scale.set(1.05, 1.05, 0.82);
            knot.position.set(0, 0, 0.05);
            bowGroup.add(knot);

            kitty.add(bowGroup);
            kitty.scale.setScalar(scale);
            return kitty;
        }

        // Constructor de corazón 3D pequeño para satélite
        function createHeartMesh(color = 0xff3b9a, size = 0.32) {
            const shape = new THREE.Shape();
            const x = 0, y = 0;
            shape.moveTo(x, y + 0.35);
            shape.bezierCurveTo(x, y + 0.35, x - 0.05, y, x - 0.35, y);
            shape.bezierCurveTo(x - 0.75, y, x - 0.75, y + 0.52, x - 0.75, y + 0.52);
            shape.bezierCurveTo(x - 0.75, y + 0.8, x - 0.45, y + 1.12, x, y + 1.38);
            shape.bezierCurveTo(x + 0.45, y + 1.12, x + 0.75, y + 0.8, x + 0.75, y + 0.52);
            shape.bezierCurveTo(x + 0.75, y + 0.52, x + 0.75, y, x + 0.35, y);
            shape.bezierCurveTo(x + 0.15, y, x, y + 0.35, x, y + 0.35);

            const geo = new THREE.ExtrudeGeometry(shape, {
                depth: 0.28,
                bevelEnabled: true,
                bevelSegments: 3,
                steps: 1,
                bevelSize: 0.08,
                bevelThickness: 0.08
            });
            geo.center();
            const mesh = new THREE.Mesh(
                geo,
                new THREE.MeshStandardMaterial({
                    color,
                    emissive: color,
                    emissiveIntensity: 0.35,
                    roughness: 0.25
                })
            );
            mesh.rotation.z = Math.PI;
            mesh.scale.setScalar(size);
            return mesh;
        }

        // Iluminación central cálida rosa-dorada
        const sunLight = new THREE.PointLight(0xffe4f3, 2.7, 340, 1.05);
        scene.add(sunLight);

        const hemiLight = new THREE.HemisphereLight(0xffb3da, 0x3b0724, 0.95);
        scene.add(hemiLight);

        const universeGroup = new THREE.Group();
        scene.add(universeGroup);

        // 1. Sol-Kitty Central 3D
        const sunGroup = new THREE.Group();
        universeGroup.add(sunGroup);

        const sunKittyModel = createKitty3DModel({
            scale: 3.4,
            isSun: true
        });
        sunGroup.add(sunKittyModel);

        const sunCoronaInner = new THREE.Sprite(
            new THREE.SpriteMaterial({
                map: glowSpriteTex,
                color: 0xfde68a,
                transparent: true,
                opacity: 0.9,
                blending: THREE.AdditiveBlending,
                depthWrite: false
            })
        );
        sunCoronaInner.scale.set(27, 27, 1);
        sunGroup.add(sunCoronaInner);

        const sunCoronaOuter = new THREE.Sprite(
            new THREE.SpriteMaterial({
                map: glowSpriteTex,
                color: 0xff3b9a,
                transparent: true,
                opacity: 0.85,
                blending: THREE.AdditiveBlending,
                depthWrite: false
            })
        );
        sunCoronaOuter.scale.set(60, 60, 1);
        sunGroup.add(sunCoronaOuter);

        // 2. Campo de estrellas rosas, blancas y doradas + nebulosas fucsia
        const starGeo = new THREE.BufferGeometry();
        const starCount = 2600;
        const starPos = new Float32Array(starCount * 3);
        const starColors = new Float32Array(starCount * 3);
        const colorChoices = [
            new THREE.Color(0xffffff),
            new THREE.Color(0xff9ad5),
            new THREE.Color(0xff3b9a),
            new THREE.Color(0xffd6ec),
            new THREE.Color(0xfde68a)
        ];

        for (let i = 0; i < starCount; i++) {
            const r = 140 + Math.random() * 380;
            const theta = Math.random() * Math.PI * 2;
            const phi = Math.acos(2 * Math.random() - 1);
            starPos[i * 3] = r * Math.sin(phi) * Math.cos(theta);
            starPos[i * 3 + 1] = r * Math.sin(phi) * Math.sin(theta);
            starPos[i * 3 + 2] = r * Math.cos(phi);

            const col = colorChoices[i % colorChoices.length];
            starColors[i * 3] = col.r;
            starColors[i * 3 + 1] = col.g;
            starColors[i * 3 + 2] = col.b;
        }
        starGeo.setAttribute('position', new THREE.BufferAttribute(starPos, 3));
        starGeo.setAttribute('color', new THREE.BufferAttribute(starColors, 3));

        const starField = new THREE.Points(
            starGeo,
            new THREE.PointsMaterial({
                size: 1.5,
                map: glowSpriteTex,
                vertexColors: true,
                transparent: true,
                opacity: 0.92,
                blending: THREE.AdditiveBlending,
                depthWrite: false
            })
        );
        scene.add(starField);

        const nebulaGroup = new THREE.Group();
        scene.add(nebulaGroup);
        const nebulaColors = [0xff3b9a, 0xd946ef, 0xff85d0, 0x9d174d, 0xf472b6];
        for (let i = 0; i < 16; i++) {
            const sprite = new THREE.Sprite(
                new THREE.SpriteMaterial({
                    map: nebulaSpriteTex,
                    color: nebulaColors[i % nebulaColors.length],
                    transparent: true,
                    opacity: 0.45,
                    blending: THREE.AdditiveBlending,
                    depthWrite: false
                })
            );
            const angle = (i / 16) * Math.PI * 2;
            const dist = 75 + Math.random() * 95;
            sprite.position.set(
                Math.cos(angle) * dist,
                (Math.random() - 0.5) * 45,
                Math.sin(angle) * dist
            );
            const s = 95 + Math.random() * 85;
            sprite.scale.set(s, s, 1);
            nebulaGroup.add(sprite);
        }

        // 3. Galaxia espiral rosa pastel, fucsia y dorada
        const galaxyCount = 4400;
        const galaxyGeo = new THREE.BufferGeometry();
        const galaxyPositions = new Float32Array(galaxyCount * 3);
        const galaxyBasePositions = new Float32Array(galaxyCount * 3);
        const galaxyColors = new Float32Array(galaxyCount * 3);

        const cInside = new THREE.Color(0xfde68a);
        const cMid = new THREE.Color(0xff9ad5);
        const cOutside = new THREE.Color(0xff3b9a);
        const branches = 4;

        for (let i = 0; i < galaxyCount; i++) {
            const radius = 12 + Math.pow(Math.random(), 1.35) * 125;
            const branchAngle = ((i % branches) / branches) * Math.PI * 2;
            const spinAngle = radius * 0.042;

            const randomX = Math.pow(Math.random(), 3) * (Math.random() < 0.5 ? 1 : -1) * (radius * 0.18);
            const randomY = Math.pow(Math.random(), 3) * (Math.random() < 0.5 ? 1 : -1) * (2.5 + radius * 0.05);
            const randomZ = Math.pow(Math.random(), 3) * (Math.random() < 0.5 ? 1 : -1) * (radius * 0.18);

            const bx = Math.cos(branchAngle + spinAngle) * radius + randomX;
            const by = randomY;
            const bz = Math.sin(branchAngle + spinAngle) * radius + randomZ;

            galaxyBasePositions[i * 3] = bx;
            galaxyBasePositions[i * 3 + 1] = by;
            galaxyBasePositions[i * 3 + 2] = bz;

            const mixedColor = cInside.clone();
            if (radius < 52) {
                mixedColor.lerp(cMid, radius / 52);
            } else {
                mixedColor.copy(cMid).lerp(cOutside, (radius - 52) / 85);
            }

            galaxyColors[i * 3] = mixedColor.r;
            galaxyColors[i * 3 + 1] = mixedColor.g;
            galaxyColors[i * 3 + 2] = mixedColor.b;
        }

        galaxyGeo.setAttribute('position', new THREE.BufferAttribute(galaxyPositions, 3));
        galaxyGeo.setAttribute('color', new THREE.BufferAttribute(galaxyColors, 3));

        const galaxyPoints = new THREE.Points(
            galaxyGeo,
            new THREE.PointsMaterial({
                size: 1.4,
                map: glowSpriteTex,
                vertexColors: true,
                transparent: true,
                opacity: 0.88,
                blending: THREE.AdditiveBlending,
                depthWrite: false
            })
        );
        universeGroup.add(galaxyPoints);

        // 4. Cinturón de destellos estelares rosa pastel entre órbitas
        const sparkleCount = 320;
        const sparkleDummy = new THREE.Object3D();
        const sparkleInstanced = new THREE.InstancedMesh(
            new THREE.OctahedronGeometry(0.24, 0),
            new THREE.MeshBasicMaterial({ color: 0xffb3da }),
            sparkleCount
        );
        const sparkleData = [];
        for (let i = 0; i < sparkleCount; i++) {
            sparkleData.push({
                radius: 43.5 + Math.random() * 3.8,
                angle: Math.random() * Math.PI * 2,
                yOffset: (Math.random() - 0.5) * 1.4,
                speed: 0.16 + Math.random() * 0.08,
                scale: 0.5 + Math.random() * 1.0,
                rotSpeed: Math.random() * 0.04
            });
        }
        universeGroup.add(sparkleInstanced);

        // 5. Creación de los 8 Planetas Kitty en 3D + Órbitas + Etiquetas
        const planetEntities = [];

        function buildPlanets() {
            planetEntities.forEach(p => universeGroup.remove(p.pivot));
            planetEntities.length = 0;
            labelsLayer.innerHTML = '';

            PLANETS_DATA.forEach((data, idx) => {
                const pivot = new THREE.Group();
                universeGroup.add(pivot);

                const orbitPoints = [];
                const segments = 128;
                for (let s = 0; s <= segments; s++) {
                    const theta = (s / segments) * Math.PI * 2;
                    orbitPoints.push(new THREE.Vector3(Math.cos(theta) * data.orbitRadius, 0, Math.sin(theta) * data.orbitRadius));
                }
                const orbitGeo = new THREE.BufferGeometry().setFromPoints(orbitPoints);
                const orbitMat = new THREE.LineBasicMaterial({
                    color: 0xff85d0,
                    transparent: true,
                    opacity: 0
                });
                const orbitLine = new THREE.Line(orbitGeo, orbitMat);
                pivot.add(orbitLine);

                const planetHolder = new THREE.Group();
                planetHolder.scale.set(0.001, 0.001, 0.001);
                pivot.add(planetHolder);

                // Modelo 3D de cabecita Kitty
                const kittyModel = createKitty3DModel({
                    scale: data.size,
                    headColor: data.headTint,
                    bowColor: data.bowColor,
                    bowCenterColor: data.bowCenter,
                    isSun: false
                });
                planetHolder.add(kittyModel);

                // Aura rosa luminosa alrededor del planeta Kitty
                const atmoSprite = new THREE.Sprite(
                    new THREE.SpriteMaterial({
                        map: glowSpriteTex,
                        color: data.atmosphere,
                        transparent: true,
                        opacity: 0.58,
                        blending: THREE.AdditiveBlending,
                        depthWrite: false
                    })
                );
                atmoSprite.scale.set(data.size * 4.2, data.size * 4.2, 1);
                planetHolder.add(atmoSprite);

                // Anillos brillantes estilo Saturno alrededor del gatito
                if (data.hasRings) {
                    const ringGeo = new THREE.RingGeometry(data.size * 1.65, data.size * 2.95, 64);
                    const ringMat = new THREE.MeshBasicMaterial({
                        map: createRingTexture(),
                        side: THREE.DoubleSide,
                        transparent: true,
                        opacity: 0.92,
                        depthWrite: false
                    });
                    const ringMesh = new THREE.Mesh(ringGeo, ringMat);
                    ringMesh.rotation.x = Math.PI / 2.35;
                    ringMesh.rotation.y = 0.2;
                    planetHolder.add(ringMesh);
                }

                // Mini satélite-corazón orbitando
                let heartMoon = null;
                if (data.hasHeartMoon) {
                    heartMoon = createHeartMesh(0xff3b9a, 0.42);
                    planetHolder.add(heartMoon);
                }

                // Corona de estrellitas orbitando
                let starCrownGroup = null;
                if (data.hasStarCrown) {
                    starCrownGroup = new THREE.Group();
                    const starMat = new THREE.MeshBasicMaterial({ color: 0xfde68a });
                    const starGeoMini = new THREE.OctahedronGeometry(0.24, 0);
                    for (let s = 0; s < 6; s++) {
                        const st = new THREE.Mesh(starGeoMini, starMat);
                        const ang = (s / 6) * Math.PI * 2;
                        st.position.set(Math.cos(ang) * (data.size * 1.65), data.size * 1.25, Math.sin(ang) * (data.size * 1.65));
                        starCrownGroup.add(st);
                    }
                    planetHolder.add(starCrownGroup);
                }

                // Etiqueta flotante con cualidad
                const labelEl = document.createElement('div');
                labelEl.className = 'planet-label';
                labelEl.innerHTML = `
                    <div class="quality-badge">${data.quality}</div>
                    <div class="planet-connector"></div>
                `;
                labelsLayer.appendChild(labelEl);

                planetEntities.push({
                    data,
                    index: idx,
                    pivot,
                    orbitLine,
                    orbitMat,
                    planetHolder,
                    kittyModel,
                    heartMoon,
                    starCrownGroup,
                    labelEl,
                    angle: data.startAngle,
                    spawnTime: 1.5 + idx * 1.55,
                    progress: 0
                });
            });
        }

        // 6. Ondas expansivas rosas y corazones/estrellas fugaces
        const pulseWaves = [];
        function spawnPulseWave(pos, maxScale = 45, color = 0xff85d0) {
            const sprite = new THREE.Sprite(
                new THREE.SpriteMaterial({
                    map: glowSpriteTex,
                    color,
                    transparent: true,
                    opacity: 0.92,
                    blending: THREE.AdditiveBlending,
                    depthWrite: false
                })
            );
            sprite.position.copy(pos);
            sprite.scale.set(2, 2, 1);
            universeGroup.add(sprite);
            pulseWaves.push({ sprite, age: 0, maxAge: 1.25, maxScale });
        }

        const shootingStars = [];
        function spawnShootingStar() {
            const group = new THREE.Group();
            const tailGeo = new THREE.BufferGeometry().setFromPoints([
                new THREE.Vector3(0, 0, 0),
                new THREE.Vector3(-12, 3, -6)
            ]);
            const tailMat = new THREE.LineBasicMaterial({
                color: 0xff9ad5,
                transparent: true,
                opacity: 0.95
            });
            group.add(new THREE.Line(tailGeo, tailMat));

            const headHeart = createHeartMesh(0xff3b9a, 0.45);
            group.add(headHeart);

            group.position.set(
                (Math.random() - 0.5) * 180,
                25 + Math.random() * 55,
                (Math.random() - 0.5) * 180
            );
            const vel = new THREE.Vector3(
                (Math.random() - 0.5) * 140,
                -25 - Math.random() * 25,
                (Math.random() - 0.5) * 140
            );
            scene.add(group);
            shootingStars.push({ group, tailMat, vel, life: 1.0 });
        }

        let simStartTime = performance.now();
        let hasStarted = false;
        let climaxShown = false;

        let userYaw = 0;
        let userPitch = 0;
        let userZoomOffset = 0;
        let isDragging = false;
        let prevPointer = { x: 0, y: 0 };
        let dragDistance = 0;
        let initialPinchDist = null;

        function startExperience() {
            simStartTime = performance.now();
            hasStarted = true;
            climaxShown = false;
            userYaw = 0;
            userPitch = 0;
            userZoomOffset = 0;

            climaxOverlay.classList.remove('show');
            replayBtn.classList.remove('visible');

            buildPlanets();
            spawnPulseWave(new THREE.Vector3(0, 0, 0), 80, 0xff69b4);
        }

        buildPlanets();
        setTimeout(() => {
            if (!hasStarted) startExperience();
        }, 1100);

        container.addEventListener('pointerdown', (e) => {
            isDragging = true;
            dragDistance = 0;
            prevPointer.x = e.clientX;
            prevPointer.y = e.clientY;
            if (!hasStarted) startExperience();
        });

        window.addEventListener('pointermove', (e) => {
            if (!isDragging || initialPinchDist !== null) return;
            const dx = e.clientX - prevPointer.x;
            const dy = e.clientY - prevPointer.y;
            dragDistance += Math.hypot(dx, dy);
            userYaw -= dx * 0.0055;
            userPitch = Math.max(-0.55, Math.min(0.65, userPitch + dy * 0.0045));
            prevPointer.x = e.clientX;
            prevPointer.y = e.clientY;
        });

        window.addEventListener('pointerup', (e) => {
            if (isDragging && dragDistance < 8 && initialPinchDist === null) {
                const raycaster = new THREE.Raycaster();
                const mouse = new THREE.Vector2(
                    (e.clientX / window.innerWidth) * 2 - 1,
                    -(e.clientY / window.innerHeight) * 2 + 1
                );
                raycaster.setFromCamera(mouse, camera);
                const targetPt = new THREE.Vector3();
                raycaster.ray.at(55, targetPt);
                spawnPulseWave(targetPt, 35, 0xff3b9a);
                spawnShootingStar();
            }
            isDragging = false;
        });

        // Soporte táctil de pellizco (pinch-to-zoom) en móviles
        container.addEventListener('touchstart', (e) => {
            if (e.touches.length === 2) {
                initialPinchDist = Math.hypot(
                    e.touches[0].clientX - e.touches[1].clientX,
                    e.touches[0].clientY - e.touches[1].clientY
                );
            }
        }, { passive: true });

        container.addEventListener('touchmove', (e) => {
            if (e.touches.length === 2 && initialPinchDist !== null) {
                const currentDist = Math.hypot(
                    e.touches[0].clientX - e.touches[1].clientX,
                    e.touches[0].clientY - e.touches[1].clientY
                );
                const delta = initialPinchDist - currentDist;
                userZoomOffset = Math.max(-45, Math.min(65, userZoomOffset + delta * 0.18));
                initialPinchDist = currentDist;
            }
        }, { passive: true });

        container.addEventListener('touchend', () => {
            initialPinchDist = null;
        }, { passive: true });

        container.addEventListener('wheel', (e) => {
            userZoomOffset = Math.max(-45, Math.min(65, userZoomOffset + e.deltaY * 0.05));
        }, { passive: true });

        replayBtn.addEventListener('click', (e) => {
            e.stopPropagation();
            startExperience();
        });

        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });

        const tempVec = new THREE.Vector3();
        let lastFrameTime = performance.now();

        function animate(now) {
            requestAnimationFrame(animate);

            const dt = Math.min(0.05, (now - lastFrameTime) / 1000);
            lastFrameTime = now;

            const elapsed = hasStarted ? (now - simStartTime) / 1000 : 0;

            // 1. Expansión de la galaxia rosa
            const galaxyExpansion = hasStarted
                ? Math.min(1, 1 - Math.pow(1 - Math.min(1, elapsed / 14.5), 2.4))
                : 0.04;

            const posAttr = galaxyGeo.attributes.position;
            for (let i = 0; i < galaxyCount; i++) {
                posAttr.array[i * 3] = galaxyBasePositions[i * 3] * galaxyExpansion;
                posAttr.array[i * 3 + 1] = galaxyBasePositions[i * 3 + 1] * galaxyExpansion;
                posAttr.array[i * 3 + 2] = galaxyBasePositions[i * 3 + 2] * galaxyExpansion;
            }
            posAttr.needsUpdate = true;
            galaxyPoints.rotation.y += dt * 0.045;

            // 2. Pulso y balanceo tierno del Sol-Kitty central
            const sunPulse = 1 + Math.sin(now * 0.0038) * 0.06;
            sunKittyModel.scale.setScalar(3.4 * sunPulse);
            sunKittyModel.rotation.y = Math.sin(now * 0.0012) * 0.35;
            sunKittyModel.rotation.z = Math.sin(now * 0.002) * 0.06;
            sunCoronaInner.scale.set(27 * sunPulse, 27 * sunPulse, 1);
            sunCoronaOuter.scale.set(60 * sunPulse, 60 * sunPulse, 1);

            // 3. Cinturón de destellos rosas
            const beltVisibility = Math.max(0, Math.min(1, (elapsed - 6.5) / 2.5));
            for (let i = 0; i < sparkleCount; i++) {
                const a = sparkleData[i];
                a.angle += a.speed * dt;
                const r = a.radius * (0.2 + 0.8 * beltVisibility);
                sparkleDummy.position.set(Math.cos(a.angle) * r, a.yOffset, Math.sin(a.angle) * r);
                sparkleDummy.rotation.x += a.rotSpeed;
                sparkleDummy.rotation.y += a.rotSpeed;
                const s = a.scale * beltVisibility;
                sparkleDummy.scale.set(s, s, s);
                sparkleDummy.updateMatrix();
                sparkleInstanced.setMatrixAt(i, sparkleDummy.matrix);
            }
            sparkleInstanced.instanceMatrix.needsUpdate = true;

            // 4. Actualización de los 8 Planetas Kitty y sus etiquetas
            const isMobile = window.innerWidth < 640;
            const mobileDistFactor = isMobile ? 1.28 : 1.0;

            planetEntities.forEach((p) => {
                p.angle += p.data.speed * dt;

                // Balanceo kawaii mirando suavemente hacia afuera/cámara mientras rota
                p.kittyModel.rotation.y = -p.angle + Math.PI / 2 + Math.sin(now * 0.0025 + p.index) * 0.28;
                p.kittyModel.rotation.z = Math.sin(now * 0.003 + p.index) * 0.07;

                if (hasStarted && elapsed >= p.spawnTime) {
                    const justSpawned = p.progress === 0;
                    p.progress = Math.min(1, p.progress + dt * 0.85);
                    const ease = 1 - Math.pow(1 - p.progress, 3);

                    const currentRadius = p.data.orbitRadius * (0.15 + 0.85 * ease);
                    const px = Math.cos(p.angle) * currentRadius;
                    const pz = Math.sin(p.angle) * currentRadius;

                    p.planetHolder.position.set(px, 0, pz);
                    p.planetHolder.scale.setScalar(Math.max(0.001, ease));
                    p.orbitMat.opacity = ease * 0.28;

                    if (justSpawned) {
                        spawnPulseWave(new THREE.Vector3(px, 0, pz), 20, 0xff85d0);
                    }

                    if (p.heartMoon) {
                        const mAngle = now * 0.0026;
                        p.heartMoon.position.set(Math.cos(mAngle) * 3.1, 0.35, Math.sin(mAngle) * 3.1);
                        p.heartMoon.rotation.y += dt * 2.2;
                    }

                    if (p.starCrownGroup) {
                        p.starCrownGroup.rotation.y += dt * 1.4;
                    }

                    p.planetHolder.getWorldPosition(tempVec);
                    tempVec.y += p.data.size * 1.45 + 1.25;
                    tempVec.project(camera);

                    if (tempVec.z < 1) {
                        const x = (tempVec.x * 0.5 + 0.5) * window.innerWidth;
                        const y = (-tempVec.y * 0.5 + 0.5) * window.innerHeight;
                        const depthScale = Math.max(0.72, Math.min(1.12, 1.15 - tempVec.z * 0.25));

                        p.labelEl.style.opacity = Math.min(0.96, ease * 1.3).toFixed(2);
                        p.labelEl.style.transform = `translate3d(${x.toFixed(1)}px, ${y.toFixed(1)}px, 0) translate(-50%, -100%) scale(${(ease * depthScale).toFixed(3)})`;
                        p.labelEl.style.zIndex = Math.round((1 - tempVec.z) * 100);

                        if (elapsed - p.spawnTime < 1.8) {
                            p.labelEl.classList.add('active-focus');
                        } else {
                            p.labelEl.classList.remove('active-focus');
                        }
                    } else {
                        p.labelEl.style.opacity = '0';
                    }
                } else {
                    p.planetHolder.scale.setScalar(0.001);
                    p.orbitMat.opacity = 0;
                    p.labelEl.style.opacity = '0';
                }
            });

            // 5. Viaje de cámara 3D
            const camT = hasStarted ? Math.min(1, elapsed / 15.5) : 0;
            const camEase = 1 - Math.pow(1 - camT, 2.5);

            const baseDist = (26 + camEase * 124) * mobileDistFactor + userZoomOffset;
            const autoOrbitAngle = elapsed * 0.11 + userYaw;
            const elevationAngle = 0.24 + camEase * 0.28 + userPitch;

            const targetCamX = Math.sin(autoOrbitAngle) * Math.cos(elevationAngle) * baseDist;
            const targetCamY = Math.sin(elevationAngle) * baseDist;
            const targetCamZ = Math.cos(autoOrbitAngle) * Math.cos(elevationAngle) * baseDist;

            camera.position.lerp(new THREE.Vector3(targetCamX, targetCamY, targetCamZ), 0.06);
            camera.lookAt(0, camEase * -4, 0);

            // 6. Clímax final
            if (hasStarted && elapsed >= 14.2 && !climaxShown) {
                climaxShown = true;
                climaxOverlay.classList.add('show');
                replayBtn.classList.add('visible');
                spawnPulseWave(new THREE.Vector3(0, 0, 0), 120, 0xff69b4);
                spawnShootingStar();
                setTimeout(spawnShootingStar, 450);
            }

            if (hasStarted && Math.random() < 0.013) {
                spawnShootingStar();
            }

            for (let i = pulseWaves.length - 1; i >= 0; i--) {
                const pw = pulseWaves[i];
                pw.age += dt;
                const ratio = pw.age / pw.maxAge;
                if (ratio >= 1) {
                    universeGroup.remove(pw.sprite);
                    pulseWaves.splice(i, 1);
                } else {
                    const s = 2 + ratio * pw.maxScale;
                    pw.sprite.scale.set(s, s, 1);
                    pw.sprite.material.opacity = (1 - ratio) * 0.78;
                }
            }

            for (let i = shootingStars.length - 1; i >= 0; i--) {
                const ss = shootingStars[i];
                ss.life -= dt * 1.1;
                ss.group.position.addScaledVector(ss.vel, dt);
                ss.tailMat.opacity = Math.max(0, ss.life);
                if (ss.life <= 0) {
                    scene.remove(ss.group);
                    shootingStars.splice(i, 1);
                }
            }

            starField.rotation.y += dt * 0.008;
            nebulaGroup.rotation.y -= dt * 0.012;

            renderer.render(scene, camera);
        }

        window.onload = function () {
            requestAnimationFrame(animate);
        };
    </script>
</body>
</html>
