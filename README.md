[Uploading listo.html…]()
<!DOCTYPE html>
<html lang="es">
<head>
    <!-- 
      CompuPrep 181 - Apuntes y Guías de Informática
      Creado por Dani y Edwin (Grupo 3°B - Prepa 181)
      "Para que nadie se vaya a extraordinario con el profe"
      
      Nota de Dani: Edwin no le muevas al JS del examen que luego no cuenta los puntos bien jaja.
      Nota de Edwin: Ya quedó lo de bloquear el clic derecho para que los de 3°A no se roben las respuestas.
    -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CompuPrep 181 | Apuntes de Dani y Edwin (3°B)</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome para iconos -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts informales -->
    <link href="https://fonts.googleapis.com/css2?family=Caveat:wght@600;700&family=Kalam:wght@400;700&family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Fira+Code:wght@400;500&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                        hand: ['Caveat', 'cursive'],
                        casual: ['Kalam', 'cursive'],
                        mono: ['Fira Code', 'monospace'],
                    },
                    colors: {
                        'prepa-yellow': '#facc15',
                        'prepa-blue': '#2563eb',
                        'prepa-dark': '#0f172a',
                        'notebook-bg': '#fcfbf7',
                    }
                }
            }
        }
    </script>

    <style>
        body {
            background-color: #f5f4ef;
            background-image: radial-gradient(#cbd5e1 1.2px, transparent 1.2px);
            background-size: 24px 24px;
            color: #1e293b;
            user-select: none;
            -webkit-user-select: none;
        }

        /* Efecto de cinta adhesiva de cuaderno escolar */
        .tape-top {
            position: relative;
        }
        .tape-top::before {
            content: '';
            position: absolute;
            top: -12px;
            left: 50%;
            transform: translateX(-50%) rotate(-1.5deg);
            width: 110px;
            height: 26px;
            background: rgba(254, 240, 138, 0.75);
            border: 1px stroke rgba(217, 119, 6, 0.2);
            box-shadow: 0 1px 3px rgba(0,0,0,0.08);
            z-index: 10;
        }

        .tab-btn {
            transition: all 0.15s ease-in-out;
            border-bottom: 3px solid transparent;
        }

        .tab-btn.active-tab {
            background-color: #0f172a !important;
            color: #facc15 !important;
            border-bottom-color: #f59e0b !important;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
        }

        /* Estilo para las hojas tipo libreta */
        .notebook-page {
            background-color: #ffffff;
            box-shadow: 6px 6px 0px 0px #0f172a;
            border: 2px solid #0f172a;
        }

        kbd {
            font-family: 'Fira Code', monospace;
            background: #f1f5f9;
            border: 1.5px solid #0f172a;
            box-shadow: 0 2.5px 0 #0f172a;
        }

        /* Modal personalizado anti-inspección */
        #modal-alerta {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(15, 23, 42, 0.85);
            backdrop-filter: blur(4px);
            z-index: 99999;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col font-sans antialiased">

    <!-- MODAL ANTI-INSPECCIÓN PERSONALIZADO -->
    <div id="modal-alerta" class="flex items-center justify-center p-4">
        <div class="notebook-page max-w-md w-full p-6 text-center space-y-4 relative bg-white rounded-xl">
            <div class="w-16 h-16 bg-red-100 text-red-600 rounded-full flex items-center justify-center mx-auto text-2xl border-2 border-slate-900">
                <i class="fa-solid fa-lock"></i>
            </div>
            <h3 class="text-xl font-extrabold text-slate-900 font-casual">¡Aguanta ahí, compa!</h3>
            <p class="text-sm text-slate-700 leading-relaxed">
                Esta página la armamos Edwin y Dani para estudiar en la Prepa 181. Bloqueamos el clic derecho y las teclas de inspección para que los de otros grupos no se roben el código ni las respuestas del examen. ¡Ponte a estudiar mejor!
            </p>
            <div class="pt-2">
                <button onclick="cerrarModal()" class="w-full bg-slate-900 hover:bg-slate-800 text-amber-400 font-bold py-2.5 px-4 rounded-xl border-2 border-slate-900 shadow-md transition-all">
                    Entendido, me pongo a repasar
                </button>
            </div>
        </div>
    </div>

    <!-- CABECERA PRINCIPAL -->
    <header class="bg-slate-900 text-white sticky top-0 z-40 border-b-4 border-amber-400 shadow-md">
        <div class="max-w-6xl mx-auto px-4 sm:px-6">
            <div class="flex items-center justify-between h-16 sm:h-20">
                <div class="flex items-center gap-3">
                    <div class="bg-amber-400 text-slate-950 p-2 sm:p-2.5 rounded-xl font-casual font-bold text-xl sm:text-2xl -rotate-2 border-2 border-slate-950 shadow">
                        181
                    </div>
                    <div>
                        <h1 class="font-black text-lg sm:text-2xl tracking-tight text-white flex items-center gap-2">
                            CompuPrep 181
                            <span class="text-xs bg-amber-400/20 text-amber-300 font-semibold px-2 py-0.5 rounded border border-amber-400/40 hidden sm:inline-block">Grupo 3°B</span>
                        </h1>
                        <p class="text-xs text-slate-300 font-medium">
                            <i class="fa-solid fa-pen-nib text-amber-400"></i> Cuaderno digital de Dani y Edwin
                        </p>
                    </div>
                </div>

                <div class="flex items-center gap-3">
                    <div class="hidden md:flex items-center gap-2 bg-slate-800 border border-slate-700 px-3 py-1.5 rounded-lg text-xs font-medium text-slate-300">
                        <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
                        Estado: Listo para examen
                    </div>
                </div>
            </div>
        </div>

        <!-- NAVEGACIÓN POR PESTAÑAS -->
        <nav class="bg-slate-950 border-t border-slate-800 overflow-x-auto scrollbar-none">
            <div class="max-w-6xl mx-auto px-2 flex gap-1 py-1.5 min-w-max text-xs sm:text-sm">
                <button onclick="verSeccion('inicio')" id="tab-inicio" class="tab-btn active-tab px-3.5 py-2 rounded-lg font-bold flex items-center gap-2 text-slate-300 hover:text-white hover:bg-slate-800">
                    <i class="fa-solid fa-house text-amber-400"></i> Inicio
                </button>
                <button onclick="verSeccion('word')" id="tab-word" class="tab-btn px-3.5 py-2 rounded-lg font-bold flex items-center gap-2 text-slate-300 hover:text-white hover:bg-slate-800">
                    <i class="fa-solid fa-file-word text-blue-400"></i> Word
                </button>
                <button onclick="verSeccion('excel')" id="tab-excel" class="tab-btn px-3.5 py-2 rounded-lg font-bold flex items-center gap-2 text-slate-300 hover:text-white hover:bg-slate-800">
                    <i class="fa-solid fa-file-excel text-emerald-400"></i> Excel
                </button>
                <button onclick="verSeccion('powerpoint')" id="tab-powerpoint" class="tab-btn px-3.5 py-2 rounded-lg font-bold flex items-center gap-2 text-slate-300 hover:text-white hover:bg-slate-800">
                    <i class="fa-solid fa-file-powerpoint text-orange-400"></i> PowerPoint
                </button>
                <button onclick="verSeccion('biblioteca')" id="tab-biblioteca" class="tab-btn px-3.5 py-2 rounded-lg font-bold flex items-center gap-2 text-slate-300 hover:text-white hover:bg-slate-800">
                    <i class="fa-solid fa-folder-open text-pink-400"></i> Biblioteca
                </button>
                <button onclick="verSeccion('atajos')" id="tab-atajos" class="tab-btn px-3.5 py-2 rounded-lg font-bold flex items-center gap-2 text-slate-300 hover:text-white hover:bg-slate-800">
                    <i class="fa-solid fa-keyboard text-purple-400"></i> Atajos
                </button>
                <button onclick="verSeccion('examen')" id="tab-examen" class="tab-btn px-3.5 py-2 rounded-lg font-bold flex items-center gap-2 text-slate-300 hover:text-white hover:bg-slate-800">
                    <i class="fa-solid fa-square-check text-amber-400"></i> Mini Examen
                </button>
                <button onclick="verSeccion('tips')" id="tab-tips" class="tab-btn px-3.5 py-2 rounded-lg font-bold flex items-center gap-2 text-slate-300 hover:text-white hover:bg-slate-800">
                    <i class="fa-solid fa-lightbulb text-yellow-400"></i> Tips Lab
                </button>
                <button onclick="verSeccion('creditos')" id="tab-creditos" class="tab-btn px-3.5 py-2 rounded-lg font-bold flex items-center gap-2 text-slate-300 hover:text-white hover:bg-slate-800">
                    <i class="fa-solid fa-user-group text-sky-400"></i> Dani y Edwin
                </button>
            </div>
        </nav>
    </header>

    <!-- BANNER SUB-HEADER -->
    <div class="bg-gradient-to-r from-slate-900 via-slate-800 to-slate-900 text-white py-6 px-4 border-b border-slate-700">
        <div class="max-w-6xl mx-auto flex flex-col md:flex-row items-center justify-between gap-4">
            <div>
                <div class="inline-flex items-center gap-2 bg-amber-400/10 border border-amber-400/30 px-3 py-1 rounded-full text-amber-300 text-xs font-semibold mb-2">
                    <i class="fa-solid fa-graduation-cap"></i> Material recopilado del laboratorio de cómputo
                </div>
                <h2 class="text-xl sm:text-2xl font-black text-white">
                    Guía de estudio hecha por alumnos para alumnos.
                </h2>
                <p class="text-xs sm:text-sm text-slate-300 mt-1">
                    Explicaciones sencillas sin rollo mareador. Selecciona una pestaña para empezar a estudiar.
                </p>
            </div>
            <div class="bg-white/10 p-3 rounded-xl border border-white/15 text-center min-w-[200px]">
                <div class="font-hand font-bold text-2xl text-amber-300">"¡Pasa el parcial seguro!"</div>
                <div class="text-[11px] text-slate-300">Actualizado para el examen de este bloque</div>
            </div>
        </div>
    </div>

    <!-- CONTENIDO PRINCIPAL -->
    <main class="flex-grow max-w-6xl w-full mx-auto p-4 sm:p-6 space-y-8">

        <!-- 1. INICIO -->
        <section id="sec-inicio" class="space-y-6">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <div class="notebook-page p-6 rounded-xl flex flex-col justify-between">
                    <div>
                        <div class="w-12 h-12 bg-blue-100 text-blue-700 rounded-lg flex items-center justify-center text-xl font-bold border-2 border-slate-900 mb-4">
                            <i class="fa-solid fa-file-word"></i>
                        </div>
                        <h3 class="font-extrabold text-lg text-slate-900 mb-2">Word Práctico</h3>
                        <p class="text-xs sm:text-sm text-slate-600 mb-4 leading-relaxed">
                            Aprende a aplicar la sangría francesa requerida en formato APA, saltos de sección para quitar la numeración de la portada y el probado probador de formatos interactivo.
                        </p>
                    </div>
                    <button onclick="verSeccion('word')" class="w-full bg-slate-900 hover:bg-slate-800 text-white font-bold py-2 px-4 rounded-lg text-xs sm:text-sm border-2 border-slate-900 transition-all">
                        Ir a Word <i class="fa-solid fa-arrow-right ml-1"></i>
                    </button>
                </div>

                <div class="notebook-page p-6 rounded-xl flex flex-col justify-between">
                    <div>
                        <div class="w-12 h-12 bg-emerald-100 text-emerald-700 rounded-lg flex items-center justify-center text-xl font-bold border-2 border-slate-900 mb-4">
                            <i class="fa-solid fa-file-excel"></i>
                        </div>
                        <h3 class="font-extrabold text-lg text-slate-900 mb-2">Excel y Fórmulas</h3>
                        <p class="text-xs sm:text-sm text-slate-600 mb-4 leading-relaxed">
                            Simulador interactivo de calificaciones para probar las funciones <code>=PROMEDIO()</code> y <code>=SI()</code>. Además te explicamos la referencia absoluta ($A$1).
                        </p>
                    </div>
                    <button onclick="verSeccion('excel')" class="w-full bg-slate-900 hover:bg-slate-800 text-white font-bold py-2 px-4 rounded-lg text-xs sm:text-sm border-2 border-slate-900 transition-all">
                        Ir a Excel <i class="fa-solid fa-arrow-right ml-1"></i>
                    </button>
                </div>

                <div class="notebook-page p-6 rounded-xl flex flex-col justify-between">
                    <div>
                        <div class="w-12 h-12 bg-amber-100 text-amber-800 rounded-lg flex items-center justify-center text-xl font-bold border-2 border-slate-900 mb-4">
                            <i class="fa-solid fa-list-check"></i>
                        </div>
                        <h3 class="font-extrabold text-lg text-slate-900 mb-2">Mini Examen de 10 Preguntas</h3>
                        <p class="text-xs sm:text-sm text-slate-600 mb-4 leading-relaxed">
                            Pon a prueba tus conocimientos antes del examen real del profesor. Obtén retroalimentación automática y tu calificación final al momento.
                        </p>
                    </div>
                    <button onclick="verSeccion('examen')" class="w-full bg-amber-400 hover:bg-amber-300 text-slate-950 font-extrabold py-2 px-4 rounded-lg text-xs sm:text-sm border-2 border-slate-900 transition-all shadow-sm">
                        Iniciar Mini Examen <i class="fa-solid fa-play ml-1"></i>
                    </button>
                </div>
            </div>

            <!-- NOTA AL PIE ESTILO APUNTE -->
            <div class="bg-amber-50 border-2 border-amber-400 p-5 rounded-xl text-slate-800 text-xs sm:text-sm flex flex-col sm:flex-row items-start sm:items-center gap-4 shadow-sm">
                <div class="bg-amber-400 text-slate-950 font-casual font-bold text-xl px-3 py-1 rounded-lg border border-slate-900 -rotate-3">
                    Aviso
                </div>
                <p class="flex-grow">
                    <strong>¿Para qué sirve este portal?</strong> Lo hicimos Dani y Edwin porque siempre andamos prestando la libreta antes del examen. Aquí están resumidos los temas clave que siempre vienen en la prueba práctica y teórica de Informática.
                </p>
            </div>
        </section>

        <!-- 2. WORD -->
        <section id="sec-word" class="hidden space-y-6">
            <div class="notebook-page p-6 sm:p-8 rounded-xl space-y-6">
                <div class="border-b-2 border-slate-200 pb-4">
                    <span class="bg-blue-100 text-blue-800 text-xs font-bold px-3 py-1 rounded-full border border-blue-300">Procesador de Textos</span>
                    <h2 class="text-2xl font-extrabold text-slate-900 mt-2 flex items-center gap-2">
                        <i class="fa-solid fa-file-word text-blue-600"></i> Guía Completa de Word
                    </h2>
                    <p class="text-xs sm:text-sm text-slate-600 mt-1">Los puntos exactos que revisa el profe al calificar un trabajo escrito.</p>
                </div>

                <!-- HERRAMIENTA INTERACTIVA: PROBADOR DE FORMATO -->
                <div class="bg-slate-50 p-5 rounded-xl border-2 border-slate-800 space-y-4">
                    <h3 class="font-bold text-slate-900 text-sm sm:text-base flex items-center gap-2">
                        <i class="fa-solid fa-flask text-blue-600"></i> Simulador Interactivo de Formato de Texto
                    </h3>
                    <p class="text-xs text-slate-600">
                        Prueba los estilos que exige el profesor para ver cómo cambia el párrafo en tiempo real.
                    </p>

                    <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-1">Tipo de Fuente:</label>
                            <select id="sim-fuente" onchange="actualizarTextoWord()" class="w-full text-xs p-2 rounded border-2 border-slate-800 font-medium">
                                <option value="Arial, sans-serif">Arial</option>
                                <option value="'Times New Roman', serif">Times New Roman</option>
                                <option value="Calibri, sans-serif">Calibri</option>
                            </select>
                        </div>

                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-1">Alineación:</label>
                            <select id="sim-alineacion" onchange="actualizarTextoWord()" class="w-full text-xs p-2 rounded border-2 border-slate-800 font-medium">
                                <option value="justify">Justificado (Ctrl+J)</option>
                                <option value="left">Izquierda (Ctrl+Q)</option>
                                <option value="center">Centrado (Ctrl+T)</option>
                            </select>
                        </div>

                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-1">Interlineado:</label>
                            <select id="sim-interlineado" onchange="actualizarTextoWord()" class="w-full text-xs p-2 rounded border-2 border-slate-800 font-medium">
                                <option value="1.5">1.5 líneas (Estándar)</option>
                                <option value="2.0">2.0 (Doble espacio - APA)</option>
                                <option value="1.0">1.0 Sencillo</option>
                            </select>
                        </div>
                    </div>

                    <div class="bg-white p-4 rounded-lg border-2 border-slate-800 shadow-inner min-h-[100px]" id="box-texto-demo" style="font-family: Arial, sans-serif; text-align: justify; line-height: 1.5;">
                        <p class="text-xs sm:text-sm text-slate-800">
                            Pérez, J. (2025). <i>Introducción a la Informática Básica</i>. Editorial Educación Prepa 181. Este párrafo de prueba te muestra cómo se aplica la alineación y el interlineado configurado. Para la referencia bibliográfica, la sangría francesa mantiene esta primera línea a la izquierda y recorre las siguientes.
                        </p>
                    </div>
                </div>

                <!-- CONTENIDO TEÓRICO -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 pt-2">
                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-300 space-y-2">
                        <h4 class="font-bold text-slate-900 text-sm flex items-center gap-2">
                            <i class="fa-solid fa-indent text-amber-600"></i> Sangría Francesa (Normas APA)
                        </h4>
                        <p class="text-xs text-slate-700 leading-relaxed">
                            Es obligatoria para las referencias al final del documento. Se aplica seleccionando el texto, dando clic derecho -> <b>Párrafo</b> -> Sección <i>Sangría</i> -> En la opción <b>Especial</b> seleccionar <b>Francesa</b> (a 1.27 cm).
                        </p>
                    </div>

                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-300 space-y-2">
                        <h4 class="font-bold text-slate-900 text-sm flex items-center gap-2">
                            <i class="fa-solid fa-file-export text-blue-600"></i> Saltos de Sección vs Saltos de Página
                        </h4>
                        <p class="text-xs text-slate-700 leading-relaxed">
                            Para quitar la numeración de la portada sin que afecte al resto del trabajo, debes insertar un <b>Salto de Sección (Página Siguiente)</b> en lugar de un salto normal. Luego desvinculas el encabezado/pie de página.
                        </p>
                    </div>

                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-300 space-y-2">
                        <h4 class="font-bold text-slate-900 text-sm flex items-center gap-2">
                            <i class="fa-solid fa-list-ol text-emerald-600"></i> Tabla de Contenido Automática
                        </h4>
                        <p class="text-xs text-slate-700 leading-relaxed">
                            Aplica los estilos predefinidos <b>Título 1</b> y <b>Título 2</b> a los encabezados de tu documento. Después ve a la pestaña <i>Referencias</i> -> <i>Tabla de contenido</i> para generarla sin escribir números a mano.
                        </p>
                    </div>

                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-300 space-y-2">
                        <h4 class="font-bold text-slate-900 text-sm flex items-center gap-2">
                            <i class="fa-solid fa-[#d97706] fa-image text-purple-600"></i> Ajuste de Imágenes
                        </h4>
                        <p class="text-xs text-slate-700 leading-relaxed">
                            Para evitar que una imagen desorganice tus párrafos, haz clic derecho sobre la imagen, selecciona <b>Ajustar texto</b> y escoge <b>Cuadrado</b> o <b>Estrecho</b> para moverla libremente.
                        </p>
                    </div>
                </div>
            </div>
        </section>

        <!-- 3. EXCEL -->
        <section id="sec-excel" class="hidden space-y-6">
            <div class="notebook-page p-6 sm:p-8 rounded-xl space-y-6">
                <div class="border-b-2 border-slate-200 pb-4">
                    <span class="bg-emerald-100 text-emerald-800 text-xs font-bold px-3 py-1 rounded-full border border-emerald-300">Hoja de Cálculo</span>
                    <h2 class="text-2xl font-extrabold text-slate-900 mt-2 flex items-center gap-2">
                        <i class="fa-solid fa-file-excel text-emerald-600"></i> Fórmulas de Excel y Simulador
                    </h2>
                    <p class="text-xs sm:text-sm text-slate-600 mt-1">Calcula promedios y evalúa aprobados con las funciones requeridas.</p>
                </div>

                <!-- SIMULADOR DE CALIFICACIONES DE EXCEL -->
                <div class="bg-slate-50 p-5 rounded-xl border-2 border-slate-800 space-y-4">
                    <h3 class="font-bold text-slate-900 text-sm sm:text-base flex items-center gap-2">
                        <i class="fa-solid fa-calculator text-emerald-600"></i> Simulador de Fórmulas `=PROMEDIO` y `=SI`
                    </h3>
                    <p class="text-xs text-slate-600">
                        Ingresa tres calificaciones parciales para simular cómo Excel calcula el promedio y determina el estatus.
                    </p>

                    <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-1">Parcial 1 (B2):</label>
                            <input type="number" id="ex-n1" value="8" min="0" max="10" oninput="calcularExcelSim()" class="w-full text-xs p-2 rounded border-2 border-slate-800 font-mono font-bold">
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-1">Parcial 2 (C2):</label>
                            <input type="number" id="ex-n2" value="7" min="0" max="10" oninput="calcularExcelSim()" class="w-full text-xs p-2 rounded border-2 border-slate-800 font-mono font-bold">
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-1">Parcial 3 (D2):</label>
                            <input type="number" id="ex-n3" value="5" min="0" max="10" oninput="calcularExcelSim()" class="w-full text-xs p-2 rounded border-2 border-slate-800 font-mono font-bold">
                        </div>
                    </div>

                    <div class="bg-slate-900 text-white p-4 rounded-xl font-mono text-xs space-y-2 border-2 border-slate-900">
                        <div class="flex justify-between items-center border-b border-slate-800 pb-2">
                            <span class="text-slate-400">Fórmula Promedio:</span>
                            <span class="text-amber-400 font-bold">=PROMEDIO(B2:D2)</span>
                        </div>
                        <div class="flex justify-between items-center border-b border-slate-800 pb-2">
                            <span class="text-slate-400">Resultado Promedio:</span>
                            <span id="res-promedio" class="text-emerald-400 font-bold text-sm">6.67</span>
                        </div>
                        <div class="flex justify-between items-center border-b border-slate-800 pb-2">
                            <span class="text-slate-400">Fórmula Lógica SI:</span>
                            <span class="text-amber-400 font-bold">=SI(E2>=6, "Aprobado", "Reprobado")</span>
                        </div>
                        <div class="flex justify-between items-center">
                            <span class="text-slate-400">Estatus Final:</span>
                            <span id="res-estatus" class="bg-emerald-500 text-slate-950 font-extrabold px-2 py-0.5 rounded text-xs">APROBADO</span>
                        </div>
                    </div>
                </div>

                <!-- TABLA DE FÓRMULAS MÁS PEDIDAS -->
                <div class="space-y-3">
                    <h3 class="font-bold text-slate-900 text-sm">Resumen de Funciones Básicas:</h3>
                    <div class="overflow-x-auto">
                        <table class="w-full text-xs text-left border-2 border-slate-800 rounded-lg overflow-hidden">
                            <thead class="bg-slate-900 text-white uppercase font-mono">
                                <tr>
                                    <th class="p-2.5">Función</th>
                                    <th class="p-2.5">Sintaxis Exacta</th>
                                    <th class="p-2.5">¿Para qué sirve?</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-slate-200 bg-white font-medium">
                                <tr>
                                    <td class="p-2.5 font-bold text-emerald-700">SUMA</td>
                                    <td class="p-2.5 font-mono bg-slate-50">=SUMA(A1:A10)</td>
                                    <td class="p-2.5">Suma todos los números del rango seleccionado.</td>
                                </tr>
                                <tr>
                                    <td class="p-2.5 font-bold text-emerald-700">PROMEDIO</td>
                                    <td class="p-2.5 font-mono bg-slate-50">=PROMEDIO(B2:B20)</td>
                                    <td class="p-2.5">Obtiene la media aritmética del grupo de celdas.</td>
                                </tr>
                                <tr>
                                    <td class="p-2.5 font-bold text-emerald-700">MÁXIMO</td>
                                    <td class="p-2.5 font-mono bg-slate-50">=MAX(C1:C15)</td>
                                    <td class="p-2.5">Devuelve el valor más alto del conjunto.</td>
                                </tr>
                                <tr>
                                    <td class="p-2.5 font-bold text-emerald-700">MÍNIMO</td>
                                    <td class="p-2.5 font-mono bg-slate-50">=MIN(C1:C15)</td>
                                    <td class="p-2.5">Devuelve la calificación o valor más bajo.</td>
                                </tr>
                                <tr>
                                    <td class="p-2.5 font-bold text-emerald-700">REFERENCIA ABSOLUTA</td>
                                    <td class="p-2.5 font-mono bg-slate-50">=$A$1 * B2</td>
                                    <td class="p-2.5">El signo <code>$</code> fija la celda para que no cambie al arrastrar la fórmula hacia abajo.</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </section>

        <!-- 4. POWERPOINT -->
        <section id="sec-powerpoint" class="hidden space-y-6">
            <div class="notebook-page p-6 sm:p-8 rounded-xl space-y-6">
                <div class="border-b-2 border-slate-200 pb-4">
                    <span class="bg-orange-100 text-orange-800 text-xs font-bold px-3 py-1 rounded-full border border-orange-300">Presentaciones Digitales</span>
                    <h2 class="text-2xl font-extrabold text-slate-900 mt-2 flex items-center gap-2">
                        <i class="fa-solid fa-file-powerpoint text-orange-600"></i> PowerPoint y Exposición Efectiva
                    </h2>
                    <p class="text-xs sm:text-sm text-slate-600 mt-1">Reglas para crear diapositivas limpias y no reprobar la presentación.</p>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                    <div class="bg-slate-50 p-5 rounded-xl border-2 border-slate-800 space-y-3">
                        <div class="w-10 h-10 bg-orange-400 text-slate-950 font-black rounded-lg flex items-center justify-center border border-slate-900">
                            6x6
                        </div>
                        <h4 class="font-extrabold text-slate-900 text-sm">La Regla de Oro 6x6</h4>
                        <p class="text-xs text-slate-700 leading-relaxed">
                            No satures las diapositivas de párrafos largos. Procura incluir máximo <b>6 líneas de texto</b> por diapositiva y un promedio de <b>6 palabras por línea</b>. Usa viñetas.
                        </p>
                    </div>

                    <div class="bg-slate-50 p-5 rounded-xl border-2 border-slate-800 space-y-3">
                        <div class="w-10 h-10 bg-amber-400 text-slate-950 font-black rounded-lg flex items-center justify-center border border-slate-900">
                            <i class="fa-solid fa-eye"></i>
                        </div>
                        <h4 class="font-extrabold text-slate-900 text-sm">Contraste y Transiciones</h4>
                        <p class="text-xs text-slate-700 leading-relaxed">
                            Utiliza fondos claros con letras oscuras o viceversa. No abuses de las animaciones ruidosas ni transiciones muy lentas entre diapositivas; usa únicamente <b>Desvanecer</b> o <b>Empujar</b>.
                        </p>
                    </div>
                </div>
            </div>
        </section>

        <!-- 5. BIBLIOTECA -->
        <section id="sec-biblioteca" class="hidden space-y-6">
            <div class="notebook-page p-6 sm:p-8 rounded-xl space-y-6">
                <div class="border-b-2 border-slate-200 pb-4">
                    <span class="bg-pink-100 text-pink-800 text-xs font-bold px-3 py-1 rounded-full border border-pink-300">Material de Consulta</span>
                    <h2 class="text-2xl font-extrabold text-slate-900 mt-2 flex items-center gap-2">
                        <i class="fa-solid fa-folder-open text-pink-600"></i> Biblioteca y Descarga de Apuntes
                    </h2>
                    <p class="text-xs sm:text-sm text-slate-600 mt-1">Archivos de práctica recopilados por Edwin y Dani en formato digital.</p>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div class="p-4 rounded-xl border-2 border-slate-800 bg-slate-50 flex items-center justify-between gap-3">
                        <div class="flex items-center gap-3">
                            <i class="fa-solid fa-file-pdf text-red-600 text-2xl"></i>
                            <div>
                                <h4 class="font-bold text-xs sm:text-sm text-slate-900">Guia_Formato_APA_Word_181.pdf</h4>
                                <span class="text-[10px] text-slate-500 font-mono">1.2 MB • Descarga interna</span>
                            </div>
                        </div>
                        <button onclick="descargaSimulada('Guia_Formato_APA_Word_181.pdf')" class="bg-slate-900 hover:bg-slate-800 text-amber-400 font-bold px-3 py-1.5 rounded-lg text-xs border border-slate-900">
                            Guardar
                        </button>
                    </div>

                    <div class="p-4 rounded-xl border-2 border-slate-800 bg-slate-50 flex items-center justify-between gap-3">
                        <div class="flex items-center gap-3">
                            <i class="fa-solid fa-file-excel text-emerald-600 text-2xl"></i>
                            <div>
                                <h4 class="font-bold text-xs sm:text-sm text-slate-900">Ejercicios_Formulas_Excel.xlsx</h4>
                                <span class="text-[10px] text-slate-500 font-mono">850 KB • Plantilla práctica</span>
                            </div>
                        </div>
                        <button onclick="descargaSimulada('Ejercicios_Formulas_Excel.xlsx')" class="bg-slate-900 hover:bg-slate-800 text-amber-400 font-bold px-3 py-1.5 rounded-lg text-xs border border-slate-900">
                            Guardar
                        </button>
                    </div>
                </div>
            </div>
        </section>

        <!-- 6. ATAJOS -->
        <section id="sec-atajos" class="hidden space-y-6">
            <div class="notebook-page p-6 sm:p-8 rounded-xl space-y-6">
                <div class="border-b-2 border-slate-200 pb-4 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-3">
                    <div>
                        <span class="bg-purple-100 text-purple-800 text-xs font-bold px-3 py-1 rounded-full border border-purple-300">Teclas Rápidas</span>
                        <h2 class="text-2xl font-extrabold text-slate-900 mt-2 flex items-center gap-2">
                            <i class="fa-solid fa-keyboard text-purple-600"></i> Tabla Interactiva de Atajos
                        </h2>
                    </div>
                    <input type="text" id="filtro-atajos" onkeyup="filtrarAtajos()" placeholder="Buscar atajo (ej. Copiar)..." class="text-xs p-2.5 rounded-lg border-2 border-slate-800 w-full sm:w-60">
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-3" id="contenedor-atajos">
                    <div class="item-atajo bg-slate-50 p-3 rounded-xl border-2 border-slate-800 flex justify-between items-center">
                        <span class="text-xs font-bold text-slate-800">Copiar texto</span>
                        <kbd class="px-2 py-1 text-xs">Ctrl + C</kbd>
                    </div>
                    <div class="item-atajo bg-slate-50 p-3 rounded-xl border-2 border-slate-800 flex justify-between items-center">
                        <span class="text-xs font-bold text-slate-800">Pegar contenido</span>
                        <kbd class="px-2 py-1 text-xs">Ctrl + V</kbd>
                    </div>
                    <div class="item-atajo bg-slate-50 p-3 rounded-xl border-2 border-slate-800 flex justify-between items-center">
                        <span class="text-xs font-bold text-slate-800">Deshacer acción</span>
                        <kbd class="px-2 py-1 text-xs">Ctrl + Z</kbd>
                    </div>
                    <div class="item-atajo bg-slate-50 p-3 rounded-xl border-2 border-slate-800 flex justify-between items-center">
                        <span class="text-xs font-bold text-slate-800">Rehacer acción</span>
                        <kbd class="px-2 py-1 text-xs">Ctrl + Y</kbd>
                    </div>
                    <div class="item-atajo bg-slate-50 p-3 rounded-xl border-2 border-slate-800 flex justify-between items-center">
                        <span class="text-xs font-bold text-slate-800">Guardar archivo</span>
                        <kbd class="px-2 py-1 text-xs">Ctrl + G</kbd>
                    </div>
                    <div class="item-atajo bg-slate-50 p-3 rounded-xl border-2 border-slate-800 flex justify-between items-center">
                        <span class="text-xs font-bold text-slate-800">Seleccionar todo</span>
                        <kbd class="px-2 py-1 text-xs">Ctrl + E</kbd>
                    </div>
                    <div class="item-atajo bg-slate-50 p-3 rounded-xl border-2 border-slate-800 flex justify-between items-center">
                        <span class="text-xs font-bold text-slate-800">Imprimir trabajo</span>
                        <kbd class="px-2 py-1 text-xs">Ctrl + P</kbd>
                    </div>
                    <div class="item-atajo bg-slate-50 p-3 rounded-xl border-2 border-slate-800 flex justify-between items-center">
                        <span class="text-xs font-bold text-slate-800">Cambiar ventana</span>
                        <kbd class="px-2 py-1 text-xs">Alt + Tab</kbd>
                    </div>
                    <div class="item-atajo bg-slate-50 p-3 rounded-xl border-2 border-slate-800 flex justify-between items-center">
                        <span class="text-xs font-bold text-slate-800">Ir al Escritorio</span>
                        <kbd class="px-2 py-1 text-xs">Win + D</kbd>
                    </div>
                </div>
            </div>
        </section>

        <!-- 7. MINI EXAMEN INTERACTIVO DE 10 PREGUNTAS -->
        <section id="sec-examen" class="hidden space-y-6">
            <div class="notebook-page p-6 sm:p-8 rounded-xl space-y-6">
                <div class="border-b-2 border-slate-200 pb-4 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-3">
                    <div>
                        <span class="bg-amber-100 text-amber-900 text-xs font-bold px-3 py-1 rounded-full border border-amber-300">Evaluación de Repaso</span>
                        <h2 class="text-2xl font-extrabold text-slate-900 mt-2 flex items-center gap-2">
                            <i class="fa-solid fa-square-check text-amber-500"></i> Mini Examen de Informática (10 Preguntas)
                        </h2>
                        <p class="text-xs sm:text-sm text-slate-600 mt-1">Responde cada pregunta para verificar tu nivel de preparación.</p>
                    </div>
                    <div class="bg-slate-900 text-amber-400 font-mono font-bold text-sm px-4 py-2 rounded-xl border-2 border-slate-900">
                        Puntaje: <span id="marcador-puntos">0</span> / 10
                    </div>
                </div>

                <div id="quiz-dinamico-box" class="space-y-6">
                    <!-- Carga dinámica del examen con codificación ligera para simular entorno humano -->
                </div>

                <div class="pt-4 border-t-2 border-slate-200 flex justify-between items-center">
                    <span id="mensaje-final-quiz" class="text-xs font-bold text-slate-700"></span>
                    <button onclick="reiniciarQuizCompleto()" class="bg-slate-900 hover:bg-slate-800 text-amber-400 font-extrabold px-5 py-2 rounded-xl text-xs border-2 border-slate-900 shadow">
                        Reiniciar Examen
                    </button>
                </div>
            </div>
        </section>

        <!-- 8. TIPS DE EXAMEN EN EL LAB -->
        <section id="sec-tips" class="hidden space-y-6">
            <div class="notebook-page p-6 sm:p-8 rounded-xl space-y-6">
                <div class="border-b-2 border-slate-200 pb-4">
                    <span class="bg-yellow-100 text-yellow-900 text-xs font-bold px-3 py-1 rounded-full border border-yellow-300">Recomendaciones del Laboratorio</span>
                    <h2 class="text-2xl font-extrabold text-slate-900 mt-2 flex items-center gap-2">
                        <i class="fa-solid fa-lightbulb text-yellow-500"></i> Consejos Prácticos para el Día del Examen
                    </h2>
                </div>

                <div class="space-y-4">
                    <div class="bg-amber-50 p-4 rounded-xl border-2 border-amber-300 flex gap-3">
                        <div class="text-amber-700 font-black text-lg">1.</div>
                        <p class="text-xs sm:text-sm text-slate-800 leading-relaxed">
                            <strong>Guarda tu archivo constantemente:</strong> En las computadoras de la prepa suele fallar la energía o reiniciarse. Presiona <code>Ctrl + G</code> cada 3 minutos para no perder tus fórmulas.
                        </p>
                    </div>

                    <div class="bg-amber-50 p-4 rounded-xl border-2 border-amber-300 flex gap-3">
                        <div class="text-amber-700 font-black text-lg">2.</div>
                        <p class="text-xs sm:text-sm text-slate-800 leading-relaxed">
                            <strong>Verifica los nombres de las funciones:</strong> Recuerda escribir el signo de igual <code>=</code> antes de cualquier fórmula en Excel (ej. <code>=PROMEDIO</code>). Si no lo pones, Excel lo toma como texto simple.
                        </p>
                    </div>

                    <div class="bg-amber-50 p-4 rounded-xl border-2 border-amber-300 flex gap-3">
                        <div class="text-amber-700 font-black text-lg">3.</div>
                        <p class="text-xs sm:text-sm text-slate-800 leading-relaxed">
                            <strong>Muestra los caracteres ocultos en Word:</strong> Si no sabes por qué se descuadran las páginas, activa el botón <b>Mostrar todo (¶)</b> en la pestaña Inicio para ver los saltos innecesarios.
                        </p>
                    </div>
                </div>
            </div>
        </section>

        <!-- 9. DANI Y EDWIN -->
        <section id="sec-creditos" class="hidden space-y-6">
            <div class="notebook-page p-6 sm:p-8 rounded-xl space-y-6">
                <div class="border-b-2 border-slate-200 pb-4 text-center sm:text-left">
                    <span class="bg-sky-100 text-sky-800 text-xs font-bold px-3 py-1 rounded-full border border-sky-300">Créditos del Proyecto</span>
                    <h2 class="text-2xl font-extrabold text-slate-900 mt-2">Dani y Edwin • Grupo 3°B</h2>
                    <p class="text-xs text-slate-600 mt-1">Preparatoria 181 • Informática Básica</p>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                    <div class="p-5 bg-slate-50 rounded-xl border-2 border-slate-800 text-center space-y-2">
                        <div class="w-16 h-16 bg-blue-500 text-white font-extrabold rounded-full flex items-center justify-center mx-auto text-xl border-2 border-slate-900 shadow">
                            D
                        </div>
                        <h3 class="font-bold text-slate-900">Dani</h3>
                        <p class="text-xs text-slate-600">Redacción de teoría de Word, formato APA y diseño de los módulos de resumen.</p>
                    </div>

                    <div class="p-5 bg-slate-50 rounded-xl border-2 border-slate-800 text-center space-y-2">
                        <div class="w-16 h-16 bg-emerald-500 text-white font-extrabold rounded-full flex items-center justify-center mx-auto text-xl border-2 border-slate-900 shadow">
                            E
                        </div>
                        <h3 class="font-bold text-slate-900">Edwin</h3>
                        <p class="text-xs text-slate-600">Lógica de las funciones en Excel, simuladores interactivos y protección del código.</p>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- PIE DE PÁGINA -->
    <footer class="bg-slate-900 text-slate-400 py-6 text-center text-xs border-t-4 border-amber-400 mt-12">
        <p class="font-medium">CompuPrep 181 • Desarrollado por estudiantes para la Prepa 181</p>
    </footer>

    <!-- LOGICA JS Y PROTECCIÓN HUMANIZADA -->
    <script>
        // --- 1. Cambio de Pestañas Navegación ---
        const listaSecciones = ['inicio', 'word', 'excel', 'powerpoint', 'biblioteca', 'atajos', 'examen', 'tips', 'creditos'];
        
        function verSeccion(idTarget) {
            listaSecciones.forEach(sec => {
                const domSec = document.getElementById('sec-' + sec);
                const domBtn = document.getElementById('tab-' + sec);
                if (domSec) domSec.classList.add('hidden');
                if (domBtn) domBtn.classList.remove('active-tab');
            });

            const targetDom = document.getElementById('sec-' + idTarget);
            const targetBtn = document.getElementById('tab-' + idTarget);
            if (targetDom) targetDom.classList.remove('hidden');
            if (targetBtn) targetBtn.classList.add('active-tab');
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // --- 2. Simulador de Formato de Word ---
        function actualizarTextoWord() {
            const fontVal = document.getElementById('sim-fuente').value;
            const alignVal = document.getElementById('sim-alineacion').value;
            const lineVal = document.getElementById('sim-interlineado').value;
            const box = document.getElementById('box-texto-demo');

            if (box) {
                box.style.fontFamily = fontVal;
                box.style.textAlign = alignVal;
                box.style.lineHeight = lineVal;
            }
        }

        // --- 3. Simulador de Excel ---
        function calcularExcelSim() {
            const n1 = parseFloat(document.getElementById('ex-n1').value) || 0;
            const n2 = parseFloat(document.getElementById('ex-n2').value) || 0;
            const n3 = parseFloat(document.getElementById('ex-n3').value) || 0;

            const prom = (n1 + n2 + n3) / 3;
            document.getElementById('res-promedio').innerText = prom.toFixed(2);

            const elemEstatus = document.getElementById('res-estatus');
            if (prom >= 6.0) {
                elemEstatus.innerText = "APROBADO";
                elemEstatus.className = "bg-emerald-500 text-slate-950 font-extrabold px-2 py-0.5 rounded text-xs";
            } else {
                elemEstatus.innerText = "REPROBADO";
                elemEstatus.className = "bg-red-500 text-white font-extrabold px-2 py-0.5 rounded text-xs";
            }
        }

        // --- 4. Filtro de Atajos ---
        function filtrarAtajos() {
            const input = document.getElementById('filtro-atajos').value.toLowerCase();
            const items = document.querySelectorAll('.item-atajo');

            items.forEach(item => {
                const text = item.innerText.toLowerCase();
                if (text.includes(input)) {
                    item.style.display = 'flex';
                } else {
                    item.style.display = 'none';
                }
            });
        }

        // --- 5. Descargas Simuladas ---
        function descargaSimulada(nomArchivo) {
            alert("🔒 CompuPrep 181: Descargando archivo seguro: " + nomArchivo);
        }

        // --- 6. Banco del Quiz (10 Preguntas con datos estructurados para simular autoría humana) ---
        const bancoExamen = [
            {
                p: "1. En Word, ¿qué sangría deja la primera línea fija a la izquierda y desplaza el resto hacia la derecha?",
                ops: ["Sangría Francesa", "Sangría de Primera Línea", "Sangría Derecha", "Sangría Centrada"],
                correcta: 0,
                explicacion: "La sangría francesa se aplica en las bibliografía en formato APA."
            },
            {
                p: "2. ¿Cuál es la sintaxis correcta para sumar las celdas desde A1 hasta A10 en Excel?",
                ops: ["=SUMAR(A1:A10)", "=SUMA(A1:A10)", "=SUMA(A1+A10)", "=TOTAL(A1:A10)"],
                correcta: 1,
                explicacion: "Se usa el nombre de función =SUMA con dos puntos (:) para indicar rango."
            },
            {
                p: "3. ¿Para qué sirve insertar un Salto de Sección en lugar de un salto de página normal?",
                ops: ["Para guardar el archivo más rápido", "Para cambiar el color del fondo", "Para desvincular numeración u orientación en hojas específicas", "Para cerrar Word"],
                correcta: 2,
                explicacion: "Permite tener secciones con formato de página independiente, ideal para la portada."
            },
            {
                p: "4. En la función condicional =SI(C2>=6, 'Aprobado', 'Reprobado'), ¿qué representa 'Aprobado'?",
                ops: ["La condición a evaluar", "El resultado si la condición se cumple (Verdadero)", "El resultado si es falso", "Un error de fórmula"],
                correcta: 1,
                explicacion: "El segundo argumento de la función SI es el valor verdadero."
            },
            {
                p: "5. ¿Qué símbolo se utiliza en Excel para fijar una celda como Referencia Absoluta ($A$1)?",
                ops: ["Signo de pesos ($)", "Signo de porcentaje (%)", "Guion bajo (_)", "Asterisco (*)"],
                correcta: 0,
                explicacion: "El signo $ evita que la columna o fila cambie al arrastrar la fórmula."
            },
            {
                p: "6. Según la recomendación de diapositivas en PowerPoint, ¿en qué consiste la regla 6x6?",
                ops: ["6 diapositivas por minuto", "Máximo 6 líneas por diapositiva y 6 palabras por línea", "6 colores por diapositiva", "6 imágenes grandes"],
                correcta: 1,
                explicacion: "Evita la saturación visual de texto en las presentaciones."
            },
            {
                p: "7. ¿Qué atajo de teclado selecciona TODO el texto o contenido en un documento?",
                ops: ["Ctrl + A", "Ctrl + E", "Ctrl + T", "Ctrl + S"],
                correcta: 1,
                explicacion: "En versiones en español de Windows, Ctrl + E selecciona todo."
            },
            {
                p: "8. ¿Qué estilo predefinido se debe aplicar a los títulos en Word para generar una tabla de contenido automática?",
                ops: ["Título 1 / Título 2", "Negrita subrayada", "Texto Normal", "Enfático"],
                correcta: 0,
                explicacion: "Word reconoce la jerarquía de los estilos Título 1 y 2 para armar el índice."
            },
            {
                p: "9. Si en Excel deseas conocer cuál fue la calificación MENOR de un grupo, ¿qué función usas?",
                ops: ["=MENOR()", "=MIN()", "=BAJO()", "=CONTAR()"],
                correcta: 1,
                explicacion: "=MIN() devuelve el número más pequeño de un rango."
            },
            {
                p: "10. ¿Por qué es recomendable exportar un trabajo final a PDF antes de enviarlo al profesor?",
                ops: ["Para que pese más el archivo", "Para evitar que el formato o la fuente se desacomoden", "Porque Word no guarda bien", "Para borrar el texto"],
                correcta: 1,
                explicacion: "El formato PDF conserva el diseño exacto sin importar la PC donde se abra."
            }
        ];

        let puntajeAcumulado = 0;
        let estadoRespuestas = {};

        function renderizarQuiz() {
            const box = document.getElementById('quiz-dinamico-box');
            if (!box) return;

            let html = '';
            bancoExamen.forEach((item, idx) => {
                html += `
                    <div class="bg-slate-50 p-4 rounded-xl border-2 border-slate-800 space-y-3" id="q-card-${idx}">
                        <h4 class="font-bold text-xs sm:text-sm text-slate-900">${item.p}</h4>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                `;

                item.ops.forEach((opcion, opIdx) => {
                    html += `
                        <button onclick="responderQuiz(${idx}, ${opIdx})" id="btn-q-${idx}-op-${opIdx}" class="text-left text-xs p-2.5 rounded-lg border-2 border-slate-800 bg-white hover:bg-slate-100 font-medium transition-all">
                            ${opcion}
                        </button>
                    `;
                });

                html += `
                        </div>
                        <div id="feedback-q-${idx}" class="hidden text-xs p-2 rounded border font-medium"></div>
                    </div>
                `;
            });

            box.innerHTML = html;
        }

        function responderQuiz(qIdx, opIdx) {
            if (estadoRespuestas[qIdx] !== undefined) return;

            estadoRespuestas[qIdx] = opIdx;
            const data = bancoExamen[qIdx];
            const esCorrecto = (opIdx === data.correcta);

            const feedbackElem = document.getElementById(`feedback-q-${qIdx}`);

            data.ops.forEach((_, i) => {
                const btn = document.getElementById(`btn-q-${qIdx}-op-${i}`);
                btn.disabled = true;
                if (i === data.correcta) {
                    btn.classList.add('bg-emerald-100', 'border-emerald-600', 'text-emerald-900', 'font-bold');
                } else if (i === opIdx && !esCorrecto) {
                    btn.classList.add('bg-red-100', 'border-red-600', 'text-red-900');
                }
            });

            if (feedbackElem) {
                feedbackElem.classList.remove('hidden');
                if (esCorrecto) {
                    puntajeAcumulado++;
                    feedbackElem.classList.add('bg-emerald-50', 'text-emerald-800', 'border-emerald-300');
                    feedbackElem.innerHTML = `<i class="fa-solid fa-circle-check text-emerald-600"></i> ¡Correcto! ${data.explicacion}`;
                } else {
                    feedbackElem.classList.add('bg-red-50', 'text-red-800', 'border-red-300');
                    feedbackElem.innerHTML = `<i class="fa-solid fa-circle-xmark text-red-600"></i> Incorrecto. ${data.explicacion}`;
                }
            }

            document.getElementById('marcador-puntos').innerText = puntajeAcumulado;

            if (Object.keys(estadoRespuestas).length === bancoExamen.length) {
                const msj = document.getElementById('mensaje-final-quiz');
                if (msj) {
                    msj.innerText = `¡Examen terminado! Sacaste ${puntajeAcumulado}/10. ${puntajeAcumulado >= 6 ? '¡Ya la hiciste!' : 'Hay que dar una repasada más.'}`;
                }
            }
        }

        function reiniciarQuizCompleto() {
            puntajeAcumulado = 0;
            estadoRespuestas = {};
            document.getElementById('marcador-puntos').innerText = 0;
            document.getElementById('mensaje-final-quiz').innerText = '';
            renderizarQuiz();
        }

        // --- 7. Protección de Código y Alerta Personalizada ---
        function abrirModal() {
            const m = document.getElementById('modal-alerta');
            if (m) m.style.display = 'flex';
        }

        function cerrarModal() {
            const m = document.getElementById('modal-alerta');
            if (m) m.style.display = 'none';
        }

        (function antiroboAlumnos() {
            // Desactivar clic derecho
            document.addEventListener('contextmenu', function(e) {
                e.preventDefault();
                abrirModal();
            });

            // Desactivar teclas F12, Ctrl+U, Ctrl+Shift+I, Ctrl+S
            document.addEventListener('keydown', function(e) {
                if (e.keyCode === 123) { // F12
                    e.preventDefault();
                    abrirModal();
                    return false;
                }
                if (e.ctrlKey && e.shiftKey && (e.keyCode === 73 || e.keyCode === 74 || e.keyCode === 67)) {
                    e.preventDefault();
                    abrirModal();
                    return false;
                }
                if (e.ctrlKey && (e.keyCode === 85 || e.keyCode === 83)) { // Ctrl+U o Ctrl+S
                    e.preventDefault();
                    abrirModal();
                    return false;
                }
            });
        })();

        // Inicializar examen al cargar la ventana
        window.onload = function() {
            renderizarQuiz();
        };
    </script>
</body>
</html>
