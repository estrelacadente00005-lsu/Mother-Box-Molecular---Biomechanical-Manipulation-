
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Caixa Materna - Manipulação Molecular e Biomecânica</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        mother: {
                            bg: '#03070d',
                            panel: 'rgba(7, 16, 27, 0.88)',
                            cyan: '#00f3ff',
                            green: '#00ff88',
                            purple: '#b026ff',
                            amber: '#ffaa00',
                            red: '#ff2a55',
                            border: 'rgba(0, 243, 255, 0.25)'
                        }
                    },
                    fontFamily: {
                        mono: ['Consolas', 'Courier New', 'monospace'],
                        sans: ['Inter', 'Segoe UI', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        ::-webkit-scrollbar { width: 5px; height: 5px; }
        ::-webkit-scrollbar-track { background: rgba(3, 7, 13, 0.9); }
        ::-webkit-scrollbar-thumb { background: rgba(0, 243, 255, 0.3); border-radius: 3px; }
        ::-webkit-scrollbar-thumb:hover { background: rgba(0, 243, 255, 0.7); }

        .neon-text-cyan { text-shadow: 0 0 10px rgba(0, 243, 255, 0.7); }
        .neon-text-green { text-shadow: 0 0 10px rgba(0, 255, 136, 0.7); }
        .neon-text-purple { text-shadow: 0 0 10px rgba(176, 38, 255, 0.7); }
        
        .glass-panel {
            background: rgba(7, 16, 27, 0.85);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(0, 243, 255, 0.22);
        }
        canvas { touch-action: none; }
    </style>
</head>
<body class="bg-[#03070d] text-gray-200 font-sans h-screen flex flex-col overflow-hidden select-none">

    <!-- Header Navigation Bar -->
    <header class="h-14 border-b border-mother-border bg-mother-bg flex items-center justify-between px-4 z-20 shrink-0">
        <div class="flex items-center gap-3">
            <div id="status-beacon" class="w-3.5 h-3.5 rounded-full bg-mother-cyan animate-pulse shadow-[0_0_10px_#00f3ff]"></div>
            <h1 class="font-bold text-base sm:text-lg tracking-wider text-mother-cyan neon-text-cyan uppercase">
                CAIXA MATERNA <span class="text-xs text-mother-green font-mono font-normal">| SROS / SAMS INTEGRATION v9.1</span>
            </h1>
        </div>
        <div class="flex items-center gap-3 text-xs font-mono">
            <div class="hidden sm:flex items-center gap-2 bg-black/60 px-3 py-1 rounded border border-mother-border">
                <span class="text-gray-400">ENERGIA CONSUMIDA:</span>
                <span id="hdr-energy" class="text-mother-amber font-bold">14.2 PW</span>
            </div>
            <div class="hidden md:flex items-center gap-2 bg-black/60 px-3 py-1 rounded border border-mother-border">
                <span class="text-gray-400">FLUXO SAMS:</span>
                <span id="hdr-sams" class="text-mother-green font-bold">98.4% ISO</span>
            </div>
            <button id="btn-info-modal" class="bg-mother-cyan/20 hover:bg-mother-cyan/40 text-mother-cyan border border-mother-cyan px-3 py-1 rounded transition text-xs font-bold">
                Visão Geral da Tecnologia
            </button>
        </div>
    </header>

    <!-- Main Workspace Grid -->
    <div class="flex-1 grid grid-cols-1 lg:grid-cols-12 overflow-hidden relative">

        <!-- Left Controls Sidebar -->
        <aside class="lg:col-span-3 glass-panel p-4 flex flex-col gap-3 overflow-y-auto z-10 max-h-[42vh] lg:max-h-full border-r border-mother-border">
            
            <!-- Mode Switching Navigation -->
            <div class="space-y-1.5">
                <label class="text-[10px] font-mono uppercase text-gray-400 tracking-wider">Modos de Operação Subatômica</label>
                <div class="grid grid-cols-1 gap-1.5 font-mono text-xs">
                    <button id="btn-mode-bio" onclick="setMode('bio')" class="w-full text-left p-2.5 rounded border border-mother-green bg-mother-green/20 text-mother-green font-bold transition flex items-center justify-between">
                        <span>🧪 1. Bio-Cura e Reorganização</span>
                        <span class="text-[10px] bg-mother-green/30 px-1.5 py-0.5 rounded">SAMS</span>
                    </button>
                    <button id="btn-mode-trans" onclick="setMode('trans')" class="w-full text-left p-2.5 rounded border border-mother-border bg-black/40 text-gray-400 hover:text-mother-cyan transition flex items-center justify-between">
                        <span>⚛️ 2. Transmutação Química</span>
                        <span class="text-[10px] bg-mother-cyan/20 px-1.5 py-0.5 rounded">QUARKS</span>
                    </button>
                    <button id="btn-mode-shield" onclick="setMode('shield')" class="w-full text-left p-2.5 rounded border border-mother-border bg-black/40 text-gray-400 hover:text-mother-purple transition flex items-center justify-between">
                        <span>🛡️ 3. Escudo Dobra Alcubierre</span>
                        <span class="text-[10px] bg-mother-purple/20 px-1.5 py-0.5 rounded">T_μν</span>
                    </button>
                </div>
            </div>

            <!-- Contextual Trigger Button -->
            <div id="dynamic-trigger-box">
                <button id="btn-primary-action" onclick="executePrimaryAction()" class="w-full bg-gradient-to-r from-mother-green via-emerald-500 to-teal-600 hover:from-emerald-400 hover:to-teal-400 text-black font-black text-xs p-3 rounded shadow-lg shadow-mother-green/20 transition transform active:scale-95 flex flex-col items-center justify-center gap-0.5 border border-emerald-300 tracking-wider uppercase">
                    <span id="btn-action-title">🧬 ACTIVAR BIO-CURA INSTANTÂNEA</span>
                    <span id="btn-action-sub" class="text-[9.5px] font-mono font-semibold text-black/80">REORGANIZAR CÉLULAS E REPARAR DNA</span>
                </button>
            </div>

            <!-- Mode Interactive Transmutation Selector (Only for Mode 2) -->
            <div id="transmutation-selector-panel" class="hidden space-y-2 bg-black/50 p-2.5 rounded border border-mother-cyan/40 font-mono text-xs">
                <span class="text-mother-cyan font-bold block text-[11px] uppercase">Alvo de Transmutação Atômica:</span>
                <select id="select-element-target" onchange="updateTransmutationTarget()" class="w-full bg-black border border-mother-cyan text-mother-cyan text-xs p-1.5 rounded focus:outline-none">
                    <option value="fe_to_ti">Ferro (Fe-56) ➔ Treliça de Carbono Titanium (Ti-C)</option>
                    <option value="pb_to_au">Chumbo (Pb-208) ➔ Ouro Puro (Au-197)</option>
                    <option value="si_to_graphene">Sílica (SiO2) ➔ Grafeno Monocristalino</option>
                    <option value="organic_to_diamond">Massa Orgânica ➔ Estrutura de Diamante Sintético</option>
                </select>
            </div>

            <!-- Interactive Hardware & Physics Sliders -->
            <div class="space-y-3 bg-black/50 p-3 rounded border border-mother-border/60 font-mono text-xs">
                <h3 class="uppercase text-mother-cyan tracking-wider text-[11px] font-bold flex items-center justify-between">
                    <span>⚙ Ajustes Módulo SROS / SAMS</span>
                    <span class="text-mother-green text-[10px]">ATIVO</span>
                </h3>

                <div>
                    <div class="flex justify-between mb-1 text-[11px]">
                        <span class="text-gray-400">Potência Lasers SROS:</span>
                        <span id="val-sros-power" class="text-mother-cyan">480 THz</span>
                    </div>
                    <input type="range" id="slider-sros-power" min="100" max="1000" value="480" class="w-full accent-mother-cyan bg-gray-800 rounded h-1.5 cursor-pointer">
                </div>

                <div>
                    <div class="flex justify-between mb-1 text-[11px]">
                        <span class="text-gray-400">Injeção Bio-Fluido SAMS:</span>
                        <span id="val-sams-rate" class="text-mother-green">2.45 L/min</span>
                    </div>
                    <input type="range" id="slider-sams-rate" min="10" max="100" value="45" class="w-full accent-mother-green bg-gray-800 rounded h-1.5 cursor-pointer">
                </div>

                <div>
                    <div class="flex justify-between mb-1 text-[11px]">
                        <span class="text-gray-400">Curvatura Gravitacional Alcubierre:</span>
                        <span id="val-alcubierre-curvature" class="text-mother-purple">-0.88 T_μν</span>
                    </div>
                    <input type="range" id="slider-alcubierre" min="10" max="100" value="88" class="w-full accent-mother-purple bg-gray-800 rounded h-1.5 cursor-pointer">
                </div>
            </div>

            <!-- System Logs Console -->
            <div class="flex-1 flex flex-col min-h-[110px] bg-black/90 rounded border border-mother-border/70 p-2 font-mono text-[11px] overflow-hidden">
                <div class="text-gray-500 border-b border-gray-800 pb-1 mb-1 flex justify-between">
                    <span>> REGISTRO DE EVENTOS ATÔMICOS</span>
                    <span id="log-clock" class="text-mother-cyan">00:00:00</span>
                </div>
                <div id="log-console" class="flex-1 overflow-y-auto space-y-1 text-gray-300 pr-1">
                    <p class="text-mother-cyan">[SISTEMA] Caixa Materna vinculada ao bio-hospedeiro.</p>
                    <p class="text-mother-green">[SAMS] Matriz de bio-fluidos estabilizada em estado homeostático.</p>
                </div>
            </div>

        </aside>

        <!-- Center Canvas Interactive Visualization Deck -->
        <main class="lg:col-span-6 relative flex flex-col items-center justify-center bg-black overflow-hidden">
            
            <!-- Canvas -->
            <canvas id="motherBoxCanvas" class="w-full h-full cursor-crosshair"></canvas>

            <!-- Mode Indicator Top Overlay HUD -->
            <div class="absolute top-4 left-4 pointer-events-none font-mono text-xs space-y-1 bg-black/70 p-3 rounded border border-mother-border/50 backdrop-blur-md z-10">
                <div class="text-mother-cyan font-bold flex items-center gap-2">
                    <span id="hud-status-dot" class="inline-block w-2.5 h-2.5 rounded-full bg-mother-green animate-ping"></span>
                    <span id="hud-mode-title">MODO: BIO-CURA & REORGANIZAÇÃO</span>
                </div>
                <div class="text-gray-400 text-[11px]">Clique/Arraste na tela para direcionar os Lasers SROS</div>
                <div class="text-mother-green text-[11px]">SAMS Viscosidade: <span id="hud-viscosity" class="text-white">1.22 Pa·s</span></div>
            </div>

            <!-- Dynamic Shockwave Pulse Overlay Effect -->
            <div id="hud-action-banner" class="hidden absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 pointer-events-none text-center z-20">
                <h2 id="hud-action-text" class="text-3xl sm:text-5xl font-black text-transparent bg-clip-text bg-gradient-to-r from-mother-cyan via-mother-green to-mother-purple neon-text-cyan tracking-wider uppercase animate-bounce">
                    REORGANIZANDO ESTRUTURA CELULAR
                </h2>
            </div>

        </main>

        <!-- Right Scientific Telemetry & Hard Sci-Fi Overview -->
        <aside class="lg:col-span-3 glass-panel p-4 flex flex-col gap-4 overflow-y-auto z-10 max-h-[42vh] lg:max-h-full border-l border-mother-border">
            
            <h2 class="text-xs font-mono uppercase text-mother-cyan tracking-wider flex items-center gap-1 border-b border-mother-border/40 pb-2 font-bold">
                <span>📊</span> Telemetria Métrica & Subatômica
            </h2>

            <!-- Metric Readout Cards -->
            <div class="grid grid-cols-2 gap-2 text-xs font-mono">
                <div class="bg-black/60 p-2.5 rounded border border-mother-cyan/30">
                    <span class="text-gray-400 block text-[10px]">DENSIDADE DE QUARKS</span>
                    <span id="tele-quark-density" class="text-mother-cyan text-sm font-bold">10³⁴ / cm³</span>
                </div>
                <div class="bg-black/60 p-2.5 rounded border border-mother-green/30">
                    <span class="text-gray-400 block text-[10px]">FREQ. LASER SROS</span>
                    <span id="tele-sros-freq" class="text-mother-green text-sm font-bold">480 THz</span>
                </div>
                <div class="bg-black/60 p-2.5 rounded border border-mother-purple/30">
                    <span class="text-gray-400 block text-[10px]">CURVATURA $T_{\mu\nu}$</span>
                    <span id="tele-alcubierre-curv" class="text-mother-purple text-sm font-bold">-0.88 Exp.</span>
                </div>
                <div class="bg-black/60 p-2.5 rounded border border-mother-amber/30">
                    <span class="text-gray-400 block text-[10px]">CONSUMO DE ENERGIA</span>
                    <span id="tele-energy-draw" class="text-mother-amber text-sm font-bold">14.2 PW</span>
                </div>
            </div>

            <!-- Molecular Real-Time Integrity Bars -->
            <div class="bg-black/70 p-3 rounded border border-mother-border/50 space-y-2.5 font-mono text-xs">
                <div>
                    <div class="flex justify-between text-[11px] mb-1">
                        <span class="text-mother-green">INTEGRIDADE TISULAR DO HOSPEDEIRO</span>
                        <span id="tele-bio-health" class="text-mother-green">100%</span>
                    </div>
                    <div class="w-full bg-gray-900 h-2 rounded overflow-hidden p-0.5 border border-gray-800">
                        <div id="bar-bio-health" class="bg-gradient-to-r from-emerald-500 to-mother-green h-full w-[100%] transition-all duration-300"></div>
                    </div>
                </div>

                <div>
                    <div class="flex justify-between text-[11px] mb-1">
                        <span class="text-mother-cyan">ESTABILIDADE DAS LIGAÇÕES DE GLÚONS</span>
                        <span id="tele-gluon-stability" class="text-mother-cyan">99.98%</span>
                    </div>
                    <div class="w-full bg-gray-900 h-2 rounded overflow-hidden p-0.5 border border-gray-800">
                        <div id="bar-gluon-stability" class="bg-gradient-to-r from-blue-500 to-mother-cyan h-full w-[99%] transition-all duration-300"></div>
                    </div>
                </div>

                <div>
                    <div class="flex justify-between text-[11px] mb-1">
                        <span class="text-mother-purple">REPULSÃO ESPACIAL DA MICRO-DOBRA</span>
                        <span id="tele-shield-repulsion" class="text-mother-purple">0.0 GW</span>
                    </div>
                    <div class="w-full bg-gray-900 h-2 rounded overflow-hidden p-0.5 border border-gray-800">
                        <div id="bar-shield-repulsion" class="bg-gradient-to-r from-purple-600 to-mother-purple h-full w-[15%] transition-all duration-300"></div>
                    </div>
                </div>
            </div>

            <!-- Hard Sci-Fi Mode Documentation -->
            <div class="space-y-2 font-mono text-xs">
                <h3 class="text-mother-cyan font-bold text-[11px] uppercase border-b border-mother-cyan/20 pb-1">
                    🔬 Fundamentos Científicos do Módulo
                </h3>
                <div id="mode-doc-text" class="bg-black/40 p-2.5 rounded border border-gray-800 space-y-2 text-gray-300 text-[11px] leading-relaxed">
                    <p>
                        <strong class="text-mother-green">Sistema SROS / SAMS:</strong> Os Lasers de Micro-focalização SROS reescrevem sequências de aminoácidos e DNA em nano-segundos, enquanto o bio-fluido SAMS fornece matrizes protéicas puras para reconstituição celular imediata.
                    </p>
                </div>
            </div>

            <!-- Canvas Legend -->
            <div class="bg-black/60 p-3 rounded border border-mother-border/30 space-y-1 text-[11px] font-mono">
                <div class="text-gray-400 mb-1 font-bold">Legenda de Componentes Visuais:</div>
                <div class="flex items-center gap-2">
                    <span class="w-3 h-3 rounded-full bg-mother-green inline-block"></span>
                    <span class="text-gray-300">Bio-Fluido SAMS & Células Reparadas</span>
                </div>
                <div class="flex items-center gap-2">
                    <span class="w-3 h-3 rounded-full bg-mother-cyan inline-block"></span>
                    <span class="text-gray-300">Lasers SROS & Quarks Up/Down</span>
                </div>
                <div class="flex items-center gap-2">
                    <span class="w-3 h-3 rounded-full bg-mother-purple inline-block"></span>
                    <span class="text-gray-300">Frente de Onda Gravitacional Alcubierre</span>
                </div>
            </div>

        </aside>

    </div>

    <!-- Tech Overview Modal -->
    <div id="info-modal" class="hidden fixed inset-0 bg-black/85 backdrop-blur-md z-50 flex items-center justify-center p-4">
        <div class="glass-panel max-w-2xl w-full p-6 rounded-lg border border-mother-cyan space-y-4 max-h-[85vh] overflow-y-auto font-mono">
            <div class="flex justify-between items-center border-b border-mother-cyan/30 pb-3">
                <h2 class="text-base sm:text-lg text-mother-cyan font-bold uppercase">Caixa Materna: Manipulação Molecular & Biomecânica</h2>
                <button id="close-modal-x" class="text-gray-400 hover:text-mother-red text-xl font-bold">&times;</button>
            </div>
            
            <div class="space-y-3 text-xs text-gray-300 leading-relaxed">
                <h3 class="text-mother-green text-sm font-bold">Arquitetura de Engenharia Quântica & Biológica</h3>
                <p>
                    Sob a perspectiva Hard Sci-Fi, a Caixa Materna utiliza campos métrico-dimensionais de alta precisão combinados com dois subsistemas avançados:
                </p>

                <div class="bg-black/60 p-3 rounded border border-mother-border space-y-1.5">
                    <h4 class="text-mother-green font-bold">1. Bio-Cura e Reorganização Biomecânica (SROS & SAMS)</h4>
                    <p>
                        Usando o **SROS** (Sistema Radiativo de Micro-focalização Subatômica) e o bio-fluido auto-montável **SAMS** (Substrato Sintético de Aminoácidos), a Caixa reorganiza lesões biológicas em tempo real. Danos a tecidos vasculares, neurais e cromossômicos são corrigidos por meio da reorientação iônica atômica.
                    </p>
                </div>

                <div class="bg-black/60 p-3 rounded border border-mother-border space-y-1.5">
                    <h4 class="text-mother-cyan font-bold">2. Transmutação de Elementos (Sintonização de Quarks)</h4>
                    <p>
                        A alteração da matéria não ocorre por magia, mas mudando o sabor dos Quarks (*Up* para *Down* ou vice-versa) através da força fraca controlada e do ajuste de gluões no núcleo atômico, convertendo qualquer elemento básico em ligas ultradensas ou carbono monocristalino.
                    </p>
                </div>

                <div class="bg-black/60 p-3 rounded border border-mother-border space-y-1.5">
                    <h4 class="text-mother-purple font-bold">3. Escudo Repulsivo de Micro-Dobra Alcubierre</h4>
                    <p>
                        Ao estressar o tensor de energia-momento ($T_{\mu\nu}$), a caixa gera um anel de densidade de energia negativa ao redor do usuário. Projéteis cinéticos e feixes de energia incidentes são defletidos pela curvatura do espaço, sem depender de barreiras físicas rígidas.
                    </p>
                </div>
            </div>

            <div class="text-right pt-2 border-t border-mother-cyan/30">
                <button id="close-modal-btn" class="bg-mother-cyan/20 hover:bg-mother-cyan/40 text-mother-cyan border border-mother-cyan px-4 py-2 rounded text-xs font-bold transition">
                    ENTENDIDO - RETORNAR AO SISTEMA
                </button>
            </div>
        </div>
    </div>

    <!-- Main JavaScript Logic Engine -->
    <script>
        // --- System State & Configuration ---
        const state = {
            mode: 'bio',                // 'bio', 'trans', 'shield'
            srosPower: 480,             // THz
            samsRate: 0.45,             // Normalized rate
            alcubierreCurvature: 0.88,  // Relative T_mu_nu
            transmutTarget: 'fe_to_ti',
            
            // Dynamics & Physics
            bioHealth: 82.0,            // Target damaged cellular tissue to heal
            gluonStability: 99.98,
            shieldRepulsion: 0.0,
            energyPW: 14.2,
            
            // Pointer Target
            pointer: { x: 0, y: 0, isPressed: false },
            
            time: 0,
            pulseWave: 0
        };

        // UI Text Maps for Documentation & Action Button
        const modeDocs = {
            bio: `<p><strong class="text-mother-green">Bio-Cura SROS / SAMS:</strong> Os Lasers SROS rastreiam sequências genéticas e bio-tecidos danificados. O bio-fluido SAMS penetra no nível celular, restaurando membranas lipídicas e reconectando axônios neurais através de estímulo eletroquímico instantâneo.</p>`,
            trans: `<p><strong class="text-mother-cyan">Transmutação Subatômica:</strong> Interação controlada com os campos de Higgs e glúons. O sistema realiza 'Quark-Flip' em prótons e nêutrons, modificando o número atômico (Z) da matéria para transmutar elementos sintéticos em tempo real.</p>`,
            shield: `<p><strong class="text-mother-purple">Micro-Dobra Alcubierre Repulsiva:</strong> Criação de uma bolha métrica local com densidade de energia negativa. Impactos cinéticos e lasers sofrem desvio geodésico gravitacional antes de atingir o bio-hospedeiro.</p>`
        };

        const actionButtons = {
            bio: {
                title: "🧬 ATIVAR BIO-CURA INSTANTÂNEA",
                sub: "REORGANIZAR CÉLULAS E REPARAR DNA",
                bg: "from-mother-green via-emerald-500 to-teal-600"
            },
            trans: {
                title: "⚛️ EXECUTA TRANSMUTAÇÃO ATÔMICA",
                sub: "REARRANJAR QUARKS E LIGAÇÕES QUÍMICAS",
                bg: "from-mother-cyan via-blue-600 to-indigo-600"
            },
            shield: {
                title: "🛡️ PULSAR ESCUDO DE REPULSÃO",
                sub: "GERAR DEFORMAÇÃO MÉTRICA ALCUBIERRE",
                bg: "from-mother-purple via-purple-600 to-pink-600"
            }
        };

        // Canvas Setup
        const canvas = document.getElementById('motherBoxCanvas');
        const ctx = canvas.getContext('2d');
        let width = 0;
        let height = 0;

        // Particle System Arrays
        let cells = [];
        let dnaHelices = [];
        let atoms = [];
        let shieldProjectiles = [];
        let laserBeams = [];

        // Web Audio Context Synthesizer
        let audioCtx = null;

        function initAudio() {
            if (!audioCtx) {
                const AudioContext = window.AudioContext || window.webkitAudioContext;
                audioCtx = new AudioContext();
            }
        }

        function playSciFiSound(type) {
            try {
                initAudio();
                if (!audioCtx) return;
                const now = audioCtx.currentTime;

                if (type === 'laser') {
                    const osc = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(800, now);
                    osc.frequency.exponentialRampToValueAtTime(120, now + 0.25);
                    gain.gain.setValueAtTime(0.3, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.25);
                    osc.connect(gain);
                    gain.connect(audioCtx.destination);
                    osc.start(now);
                    osc.stop(now + 0.25);
                } else if (type === 'transmute') {
                    const osc = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(300, now);
                    osc.frequency.linearRampToValueAtTime(1200, now + 0.5);
                    gain.gain.setValueAtTime(0.4, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.55);
                    osc.connect(gain);
                    gain.connect(audioCtx.destination);
                    osc.start(now);
                    osc.stop(now + 0.55);
                } else if (type === 'shield') {
                    const osc = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(150, now);
                    osc.frequency.exponentialRampToValueAtTime(40, now + 0.8);
                    gain.gain.setValueAtTime(0.6, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.8);
                    osc.connect(gain);
                    gain.connect(audioCtx.destination);
                    osc.start(now);
                    osc.stop(now + 0.8);
                }
            } catch (e) {
                console.log("Audio syntax supported on interaction.");
            }
        }

        // --- Initialize Simulation Entities ---
        function initModeEntities() {
            // Mode 1: Biological Cells & DNA strands
            cells = [];
            for (let i = 0; i < 45; i++) {
                cells.push({
                    x: Math.random() * (width || 600),
                    y: Math.random() * (height || 500),
                    radius: Math.random() * 14 + 10,
                    damaged: Math.random() < 0.4,
                    repairProgress: 0,
                    vx: (Math.random() - 0.5) * 0.8,
                    vy: (Math.random() - 0.5) * 0.8
                });
            }

            dnaHelices = [];
            for (let i = 0; i < 30; i++) {
                dnaHelices.push({
                    y: (i / 30) * (height || 500),
                    phase: i * 0.4
                });
            }

            // Mode 2: Atoms, Nucleus & Quarks
            atoms = [];
            for (let i = 0; i < 18; i++) {
                atoms.push({
                    x: (Math.random() * 0.7 + 0.15) * (width || 600),
                    y: (Math.random() * 0.7 + 0.15) * (height || 500),
                    type: 'Fe', // Initial state
                    quarks: [
                        { type: 'up', angle: Math.random() * Math.PI * 2 },
                        { type: 'up', angle: Math.random() * Math.PI * 2 },
                        { type: 'down', angle: Math.random() * Math.PI * 2 }
                    ],
                    electrons: 6,
                    transmuting: false
                });
            }

            // Mode 3: Alcubierre Defense Projectiles
            shieldProjectiles = [];
            for (let i = 0; i < 12; i++) {
                spawnProjectile();
            }
        }

        function spawnProjectile() {
            const angle = Math.random() * Math.PI * 2;
            const dist = Math.max(width, height) * 0.6;
            shieldProjectiles.push({
                x: width / 2 + Math.cos(angle) * dist,
                y: height / 2 + Math.sin(angle) * dist,
                vx: -Math.cos(angle) * (Math.random() * 3 + 2),
                vy: -Math.sin(angle) * (Math.random() * 3 + 2),
                deflected: false,
                radius: 4 + Math.random() * 3
            });
        }

        function handleResize() {
            const container = canvas.parentElement;
            width = container.clientWidth;
            height = container.clientHeight;
            canvas.width = width;
            canvas.height = height;

            state.pointer.x = width / 2;
            state.pointer.y = height / 2;

            initModeEntities();
        }

        window.addEventListener('resize', handleResize);

        // --- Main Animation Render Loop ---
        function draw() {
            state.time += 0.025;

            // Clear Background
            ctx.fillStyle = '#03070d';
            ctx.fillRect(0, 0, width, height);

            // Render Mode-specific 2D Visualization
            if (state.mode === 'bio') {
                renderBioHealingMode();
            } else if (state.mode === 'trans') {
                renderTransmutationMode();
            } else if (state.mode === 'shield') {
                renderAlcubierreShieldMode();
            }

            // Laser Pointer Beam overlay
            renderSROSMicroLasers();

            // Render Dynamic UI HUD Updates
            updateDynamicTelemetry();

            requestAnimationFrame(draw);
        }

        // ==========================================
        // MODE 1: Bio-Cura e Reorganização Biomecânica
        // ==========================================
        function renderBioHealingMode() {
            // Draw SAMS Bio-fluid Substrate Flow Background
            ctx.lineWidth = 1;
            ctx.strokeStyle = 'rgba(0, 255, 136, 0.08)';
            const gridStep = 40;
            for (let x = 0; x < width; x += gridStep) {
                ctx.beginPath();
                for (let y = 0; y < height; y += 20) {
                    const waveX = x + Math.sin(y * 0.02 + state.time) * (15 * state.samsRate);
                    if (y === 0) ctx.moveTo(waveX, y);
                    else ctx.lineTo(waveX, y);
                }
                ctx.stroke();
            }

            // Draw DNA Double Helix Strand on Right Side
            const dnaX = width * 0.85;
            ctx.lineWidth = 2;
            for (let i = 0; i < dnaHelices.length; i++) {
                const strand = dnaHelices[i];
                const y = (strand.y + state.time * 20) % height;
                const offset = Math.sin(state.time * 2 + strand.phase) * 35;

                // Base Pair Connector
                ctx.strokeStyle = 'rgba(0, 243, 255, 0.3)';
                ctx.beginPath();
                ctx.moveTo(dnaX - offset, y);
                ctx.lineTo(dnaX + offset, y);
                ctx.stroke();

                // Nucleotides
                ctx.fillStyle = '#00ff88';
                ctx.beginPath();
                ctx.arc(dnaX - offset, y, 4, 0, Math.PI * 2);
                ctx.fill();

                ctx.fillStyle = '#00f3ff';
                ctx.beginPath();
                ctx.arc(dnaX + offset, y, 4, 0, Math.PI * 2);
                ctx.fill();
            }

            // Render Biological Cells
            let damagedCount = 0;

            cells.forEach((cell) => {
                cell.x += cell.vx;
                cell.y += cell.vy;

                // Bounce from bounds
                if (cell.x < 30 || cell.x > width - 30) cell.vx *= -1;
                if (cell.y < 30 || cell.y > height - 30) cell.vy *= -1;

                // Laser Targeting Interaction
                const distToPointer = Math.hypot(cell.x - state.pointer.x, cell.y - state.pointer.y);
                if (cell.damaged && (distToPointer < 100 || state.pulseWave > 0)) {
                    cell.repairProgress += 0.02 * (state.srosPower / 300);
                    if (cell.repairProgress >= 1.0) {
                        cell.damaged = false;
                        cell.repairProgress = 1.0;
                    }
                }

                if (cell.damaged) damagedCount++;

                // Cell Body Rendering
                ctx.save();
                ctx.translate(cell.x, cell.y);

                if (cell.damaged) {
                    ctx.strokeStyle = `rgba(255, 42, 85, ${0.7 - cell.repairProgress * 0.5})`;
                    ctx.fillStyle = `rgba(255, 42, 85, ${0.2 - cell.repairProgress * 0.15})`;
                } else {
                    ctx.strokeStyle = 'rgba(0, 255, 136, 0.8)';
                    ctx.fillStyle = 'rgba(0, 255, 136, 0.15)';
                }

                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.arc(0, 0, cell.radius, 0, Math.PI * 2);
                ctx.fill();
                ctx.stroke();

                // Nucleus
                ctx.fillStyle = cell.damaged ? '#ff2a55' : '#00f3ff';
                ctx.beginPath();
                ctx.arc(0, 0, cell.radius * 0.35, 0, Math.PI * 2);
                ctx.fill();

                // SAMS Bio-Fluid Repair Rings
                if (cell.repairProgress > 0 && cell.repairProgress < 1.0) {
                    ctx.strokeStyle = '#00ff88';
                    ctx.beginPath();
                    ctx.arc(0, 0, cell.radius * (1 + cell.repairProgress * 0.5), 0, Math.PI * 2);
                    ctx.stroke();
                }

                ctx.restore();
            });

            // Update Tissue Health Percentage
            state.bioHealth = Math.min(100, Math.max(0, ((cells.length - damagedCount) / cells.length) * 100));
        }

        // ==========================================
        // MODE 2: Transmutação de Elementos (Quarks)
        // ==========================================
        function renderTransmutationMode() {
            // Draw Subatomic Grid lattice
            ctx.strokeStyle = 'rgba(0, 243, 255, 0.1)';
            ctx.lineWidth = 1;
            for (let x = 0; x < width; x += 50) {
                for (let y = 0; y < height; y += 50) {
                    ctx.strokeRect(x, y, 50, 50);
                }
            }

            // Render Atomic Structures
            atoms.forEach((atom) => {
                ctx.save();
                ctx.translate(atom.x, atom.y);

                const isTargeted = Math.hypot(atom.x - state.pointer.x, atom.y - state.pointer.y) < 110;

                if (isTargeted || atom.transmuting) {
                    atom.transmuting = true;
                    // Quark Flip logic
                    atom.quarks.forEach(q => q.angle += 0.08);
                } else {
                    atom.quarks.forEach(q => q.angle += 0.02);
                }

                // Electron Shell Orbits
                ctx.strokeStyle = atom.type === 'Ti' ? 'rgba(0, 243, 255, 0.6)' : 'rgba(255, 170, 0, 0.5)';
                ctx.lineWidth = 1.5;
                
                for (let e = 1; e <= 2; e++) {
                    const r = 30 * e;
                    ctx.beginPath();
                    ctx.arc(0, 0, r, 0, Math.PI * 2);
                    ctx.stroke();

                    // Electrons
                    const eAngle = state.time * (3 / e);
                    const ex = Math.cos(eAngle) * r;
                    const ey = Math.sin(eAngle) * r;
                    ctx.fillStyle = '#00f3ff';
                    ctx.beginPath();
                    ctx.arc(ex, ey, 3, 0, Math.PI * 2);
                    ctx.fill();
                }

                // Atomic Nucleus (Gluton Bond Cluster)
                ctx.fillStyle = atom.type === 'Ti' ? '#00f3ff' : '#ffaa00';
                ctx.shadowBlur = 12;
                ctx.shadowColor = ctx.fillStyle;
                ctx.beginPath();
                ctx.arc(0, 0, 14, 0, Math.PI * 2);
                ctx.fill();
                ctx.shadowBlur = 0;

                // Quarks Inside Nucleus (Up / Down)
                atom.quarks.forEach((q, idx) => {
                    const qx = Math.cos(q.angle + (idx * Math.PI * 2 / 3)) * 7;
                    const qy = Math.sin(q.angle + (idx * Math.PI * 2 / 3)) * 7;
                    ctx.fillStyle = idx % 2 === 0 ? '#00ff88' : '#b026ff';
                    ctx.beginPath();
                    ctx.arc(qx, qy, 2.5, 0, Math.PI * 2);
                    ctx.fill();
                });

                // Element Label
                ctx.fillStyle = '#ffffff';
                ctx.font = 'bold 10px Consolas';
                ctx.textAlign = 'center';
                ctx.fillText(atom.type, 0, 25);

                ctx.restore();
            });
        }

        // ==========================================
        // MODE 3: Escudo de Força Alcubierre (Repulsão)
        // ==========================================
        function renderAlcubierreShieldMode() {
            const centerX = width / 2;
            const centerY = height / 2;

            // Core Bio-host Indicator
            ctx.fillStyle = '#00f3ff';
            ctx.shadowBlur = 20;
            ctx.shadowColor = '#00f3ff';
            ctx.beginPath();
            ctx.arc(centerX, centerY, 18, 0, Math.PI * 2);
            ctx.fill();
            ctx.shadowBlur = 0;

            // Alcubierre Metric Distortion Field Rings
            const baseRadius = 110 * (state.alcubierreCurvature / 0.88);
            const ringCount = 5;

            for (let i = 0; i < ringCount; i++) {
                const r = baseRadius + (i * 18) + Math.sin(state.time * 4 + i) * 6;
                const alpha = 0.8 - (i * 0.15);

                ctx.strokeStyle = `rgba(176, 38, 255, ${alpha})`;
                ctx.lineWidth = 2.5;
                ctx.beginPath();
                ctx.arc(centerX, centerY, r, 0, Math.PI * 2);
                ctx.stroke();
            }

            // Shockwave Pulse Wave Expansion
            if (state.pulseWave > 0) {
                state.pulseWave += 12;
                ctx.strokeStyle = `rgba(0, 243, 255, ${Math.max(0, 1 - state.pulseWave / 400)})`;
                ctx.lineWidth = 6;
                ctx.beginPath();
                ctx.arc(centerX, centerY, baseRadius + state.pulseWave, 0, Math.PI * 2);
                ctx.stroke();

                if (state.pulseWave > 400) state.pulseWave = 0;
            }

            // Incoming Projectiles Deflection Physics
            shieldProjectiles.forEach((p, idx) => {
                p.x += p.vx;
                p.y += p.vy;

                const distToCore = Math.hypot(p.x - centerX, p.y - centerY);

                // Deflection boundary
                if (distToCore < baseRadius + 20 && !p.deflected) {
                    p.deflected = true;
                    // Reverse vector with repulsion spatial warp
                    const angle = Math.atan2(p.y - centerY, p.x - centerX);
                    p.vx = Math.cos(angle) * 8;
                    p.vy = Math.sin(angle) * 8;
                    state.shieldRepulsion = (parseFloat(state.shieldRepulsion) + 12.4).toFixed(1);
                    playSciFiSound('shield');
                }

                // Reset projectile if off-screen
                if (p.x < 0 || p.x > width || p.y < 0 || p.y > height) {
                    shieldProjectiles.splice(idx, 1);
                    spawnProjectile();
                }

                // Draw Projectile
                ctx.fillStyle = p.deflected ? '#00f3ff' : '#ff2a55';
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
                ctx.fill();
            });
        }

        // Render SROS Targeting Lasers
        function renderSROSMicroLasers() {
            if (!state.pointer.isPressed && state.pulseWave === 0) return;

            const targetX = state.pointer.x;
            const targetY = state.pointer.y;

            // Emit Lasers from Top Corners (SROS Emitters)
            const emitters = [
                { x: 30, y: 30 },
                { x: width - 30, y: 30 }
            ];

            emitters.forEach(e => {
                ctx.strokeStyle = state.mode === 'bio' ? '#00ff88' : (state.mode === 'trans' ? '#00f3ff' : '#b026ff');
                ctx.lineWidth = 2;
                ctx.shadowBlur = 10;
                ctx.shadowColor = ctx.strokeStyle;

                ctx.beginPath();
                ctx.moveTo(e.x, e.y);
                ctx.lineTo(targetX, targetY);
                ctx.stroke();

                ctx.shadowBlur = 0;
            });
        }

        // --- Mode Switcher ---
        function setMode(modeKey) {
            state.mode = modeKey;

            // UI Button highlights
            const btns = { bio: 'btn-mode-bio', trans: 'btn-mode-trans', shield: 'btn-mode-shield' };
            Object.keys(btns).forEach(k => {
                const el = document.getElementById(btns[k]);
                if (k === modeKey) {
                    const activeColor = k === 'bio' ? 'border-mother-green bg-mother-green/20 text-mother-green' : (k === 'trans' ? 'border-mother-cyan bg-mother-cyan/20 text-mother-cyan' : 'border-mother-purple bg-mother-purple/20 text-mother-purple');
                    el.className = `w-full text-left p-2.5 rounded border font-bold transition flex items-center justify-between ${activeColor}`;
                } else {
                    el.className = 'w-full text-left p-2.5 rounded border border-mother-border bg-black/40 text-gray-400 hover:text-white transition flex items-center justify-between';
                }
            });

            // Action button updates
            const act = actionButtons[modeKey];
            document.getElementById('btn-action-title').innerText = act.title;
            document.getElementById('btn-action-sub').innerText = act.sub;
            document.getElementById('btn-primary-action').className = `w-full bg-gradient-to-r ${act.bg} text-black font-black text-xs p-3 rounded shadow-lg transition transform active:scale-95 flex flex-col items-center justify-center gap-0.5 border border-white/40 uppercase tracking-wider`;

            // Toggle Transmutation Target dropdown
            const selector = document.getElementById('transmutation-selector-panel');
            if (modeKey === 'trans') selector.classList.remove('hidden');
            else selector.classList.add('hidden');

            // Documentation panel update
            document.getElementById('mode-doc-text').innerHTML = modeDocs[modeKey];

            // HUD Title update
            const hudTitles = {
                bio: "MODO: BIO-CURA & REORGANIZAÇÃO",
                trans: "MODO: TRANSMUTAÇÃO QUÍMICA & QUARKS",
                shield: "MODO: ESCUDO DE DOBRA ALCUBIERRE"
            };
            document.getElementById('hud-mode-title').innerText = hudTitles[modeKey];

            addLog(`[MODO] Módulo alterado para: ${hudTitles[modeKey]}`);
        }

        // --- Execute Primary Action Trigger ---
        function executePrimaryAction() {
            state.pulseWave = 1;

            const banner = document.getElementById('hud-action-banner');
            const text = document.getElementById('hud-action-text');

            if (state.mode === 'bio') {
                playSciFiSound('laser');
                text.innerText = "BIO-CURA REORGANIZANDO CÉLULAS!";
                cells.forEach(c => {
                    c.damaged = false;
                    c.repairProgress = 1.0;
                });
                addLog('🧬 [BIO-CURA] SROS lasers regeneraram 100% dos tecidos danificados.', 'green');
            } else if (state.mode === 'trans') {
                playSciFiSound('transmute');
                text.innerText = "TRANSMUTAÇÃO QUÂNTICA EXECUTADA!";
                const targetName = state.transmutTarget === 'fe_to_ti' ? 'Ti' : 'Au';
                atoms.forEach(a => a.type = targetName);
                addLog(`⚛️ [QUARKS] Quarks rearranjados com sucesso para elemento: ${targetName}`, 'cyan');
            } else if (state.mode === 'shield') {
                playSciFiSound('shield');
                text.innerText = "ESCUDO ALCUBIERRE PULSADO!";
                shieldProjectiles.forEach(p => p.deflected = true);
                addLog('🛡️ [DOBRA] Micro-dobra gravitacional expandida. Projéteis repulcionados!', 'purple');
            }

            banner.classList.remove('hidden');
            setTimeout(() => { banner.classList.add('hidden'); }, 1600);
        }

        function updateTransmutationTarget() {
            state.transmutTarget = document.getElementById('select-element-target').value;
            addLog(`[ALVO] Transmutação ajustada para: ${state.transmutTarget.toUpperCase()}`);
        }

        // --- Event Listeners & Interaction Drag ---
        function initInteractions() {
            const getCoords = (e) => {
                const rect = canvas.getBoundingClientRect();
                const clientX = e.touches ? e.touches[0].clientX : e.clientX;
                const clientY = e.touches ? e.touches[0].clientY : e.clientY;
                return {
                    x: clientX - rect.left,
                    y: clientY - rect.top
                };
            };

            const start = (e) => {
                state.pointer.isPressed = true;
                const c = getCoords(e);
                state.pointer.x = c.x;
                state.pointer.y = c.y;
            };

            const move = (e) => {
                if (!state.pointer.isPressed) return;
                const c = getCoords(e);
                state.pointer.x = c.x;
                state.pointer.y = c.y;
            };

            const stop = () => { state.pointer.isPressed = false; };

            canvas.addEventListener('mousedown', start);
            canvas.addEventListener('mousemove', move);
            window.addEventListener('mouseup', stop);

            canvas.addEventListener('touchstart', start);
            canvas.addEventListener('touchmove', move);
            window.addEventListener('touchend', stop);

            // Sliders
            document.getElementById('slider-sros-power').addEventListener('input', (e) => {
                state.srosPower = parseInt(e.target.value);
                document.getElementById('val-sros-power').innerText = `${state.srosPower} THz`;
                document.getElementById('tele-sros-freq').innerText = `${state.srosPower} THz`;
            });

            document.getElementById('slider-sams-rate').addEventListener('input', (e) => {
                state.samsRate = e.target.value / 100;
                const lmin = (state.samsRate * 5.4).toFixed(2);
                document.getElementById('val-sams-rate').innerText = `${lmin} L/min`;
            });

            document.getElementById('slider-alcubierre').addEventListener('input', (e) => {
                state.alcubierreCurvature = (e.target.value / 100).toFixed(2);
                document.getElementById('val-alcubierre-curvature').innerText = `-${state.alcubierreCurvature} T_μν`;
                document.getElementById('tele-alcubierre-curv').innerText = `-${state.alcubierreCurvature} Exp.`;
            });

            // Modal Controls
            document.getElementById('btn-info-modal').addEventListener('click', () => {
                document.getElementById('info-modal').classList.remove('hidden');
            });
            const closeModal = () => document.getElementById('info-modal').classList.add('hidden');
            document.getElementById('close-modal-x').addEventListener('click', closeModal);
            document.getElementById('close-modal-btn').addEventListener('click', closeModal);
        }

        // Dynamic Telemetry HUD Refresher
        function updateDynamicTelemetry() {
            const now = new Date();
            document.getElementById('log-clock').innerText = now.toTimeString().split(' ')[0];

            // Tissue Health Update
            const healthVal = state.bioHealth.toFixed(1);
            document.getElementById('tele-bio-health').innerText = `${healthVal}%`;
            document.getElementById('bar-bio-health').style.width = `${healthVal}%`;

            // Shield Repulsion Meter Decay
            if (state.shieldRepulsion > 0) {
                state.shieldRepulsion = Math.max(0, state.shieldRepulsion - 0.2).toFixed(1);
            }
            document.getElementById('tele-shield-repulsion').innerText = `${state.shieldRepulsion} GW`;
            document.getElementById('bar-shield-repulsion').style.width = `${Math.min(100, state.shieldRepulsion)}%`;

            // Viscosity calculation
            const viscosity = (1.10 + state.samsRate * 0.4).toFixed(2);
            document.getElementById('hud-viscosity').innerText = `${viscosity} Pa·s`;

            // Energy Draw dynamics
            state.energyPW = (12.0 + (state.srosPower / 200) + parseFloat(state.alcubierreCurvature)).toFixed(1);
            document.getElementById('hdr-energy').innerText = `${state.energyPW} PW`;
            document.getElementById('tele-energy-draw').innerText = `${state.energyPW} PW`;
        }

        function addLog(msg, color = 'cyan') {
            const consoleBox = document.getElementById('log-console');
            const p = document.createElement('p');

            if (color === 'green') p.className = 'text-mother-green font-bold';
            else if (color === 'purple') p.className = 'text-mother-purple font-bold';
            else if (color === 'pink') p.className = 'text-mother-red font-bold';
            else p.className = 'text-mother-cyan';

            p.innerText = `${msg}`;
            consoleBox.appendChild(p);
            consoleBox.scrollTop = consoleBox.scrollHeight;
        }

        // Initialize Engine on Window Load
        window.onload = () => {
            handleResize();
            initInteractions();
            setMode('bio');
            draw();
            addLog('[INICIALIZAÇÃO] Módulo Biomecânico e Transmutador ativado.');
        };
    </script>
</body>
</html>
