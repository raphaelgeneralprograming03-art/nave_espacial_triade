
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SISTEMA ESPACIAL QUANTICO // NAVE INTERPLANETÁRIA EXPLORER-1</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; user-select: none; }
        body, html { width: 100%; height: 100%; overflow: hidden; background-color: #030712; font-family: 'Segoe UI', 'Courier New', monospace; color: #38bdf8; }
        
        #canvas-container { width: 100%; height: 100%; position: absolute; top: 0; left: 0; z-index: 1; }
        canvas { display: block; width: 100%; height: 100%; }

        /* HUD PANEL (ESQUERDA) */
        .hud-panel {
            position: absolute; top: 15px; left: 15px; z-index: 10;
            background: rgba(10, 22, 40, 0.92); border: 1px solid #38bdf8;
            border-radius: 10px; padding: 15px; width: 380px; max-height: calc(100vh - 30px);
            overflow-y: auto; box-shadow: 0 0 30px rgba(56, 189, 248, 0.25);
            backdrop-filter: blur(12px);
        }

        .hud-header {
            font-size: 13px; font-weight: 900; letter-spacing: 1.5px;
            text-transform: uppercase; color: #38bdf8; margin-bottom: 12px;
            border-bottom: 2px solid #38bdf8; padding-bottom: 6px;
            display: flex; justify-content: space-between; align-items: center;
        }

        .badge-nasa {
            background: #ef4444; color: #ffffff; font-size: 9px; padding: 2px 7px; border-radius: 3px; font-weight: 900;
        }

        .section-title {
            font-size: 10.5px; font-weight: bold; color: #94a3b8; margin: 10px 0 6px 0; text-transform: uppercase; letter-spacing: 1px;
        }

        /* TRÍADE STATUS */
        .triad-box {
            background: rgba(56, 189, 248, 0.05); border: 1px solid rgba(56, 189, 248, 0.3);
            border-radius: 6px; padding: 8px; margin-bottom: 12px;
        }

        .triad-item {
            display: flex; justify-content: space-between; align-items: center;
            font-size: 10px; font-weight: bold; margin: 4px 0; color: #f8fafc;
        }

        .triad-status {
            padding: 2px 6px; border-radius: 3px; font-size: 9px; font-weight: bold;
        }
        .status-mhd { background: rgba(56, 189, 248, 0.2); color: #38bdf8; border: 1px solid #38bdf8; }
        .status-sams { background: rgba(168, 85, 247, 0.2); color: #c084fc; border: 1px solid #c084fc; }
        .status-sros { background: rgba(16, 185, 129, 0.2); color: #34d399; border: 1px solid #34d399; }

        .btn-mode {
            width: 100%; background: rgba(56, 189, 248, 0.06); border: 1px solid rgba(56, 189, 248, 0.3);
            color: #38bdf8; padding: 8px 10px; margin-bottom: 5px; border-radius: 4px;
            font-family: inherit; font-size: 10px; font-weight: bold; cursor: pointer;
            display: flex; justify-content: space-between; align-items: center;
            transition: all 0.2s ease; text-transform: uppercase; letter-spacing: 0.5px;
        }

        .btn-mode:hover {
            background: rgba(56, 189, 248, 0.25); border-color: #38bdf8;
            box-shadow: 0 0 14px rgba(56, 189, 248, 0.4); transform: translateX(2px);
        }

        .btn-mode.active {
            background: #38bdf8; color: #030712; border-color: #38bdf8;
            box-shadow: 0 0 18px rgba(56, 189, 248, 0.6);
        }

        /* BANCO DE DADOS QUÂNTICO SROS (DIREITA) */
        .quantum-panel {
            position: absolute; top: 15px; right: 15px; z-index: 10;
            background: rgba(10, 22, 40, 0.92); border: 1px solid #34d399;
            border-radius: 10px; padding: 15px; width: 340px;
            box-shadow: 0 0 30px rgba(52, 211, 153, 0.2);
            backdrop-filter: blur(12px);
        }

        .quantum-header {
            font-size: 11px; font-weight: bold; letter-spacing: 1px;
            color: #34d399; border-bottom: 1px solid #34d399; padding-bottom: 6px; margin-bottom: 10px;
            text-transform: uppercase; display: flex; justify-content: space-between;
        }

        .metric-row { display: flex; justify-content: space-between; margin: 5px 0; font-size: 10px; color: #cbd5e1; }
        .metric-val { font-family: monospace; font-weight: bold; color: #34d399; }

        .chem-card {
            background: rgba(15, 23, 42, 0.8); border: 1px solid rgba(52, 211, 153, 0.3);
            border-radius: 5px; padding: 8px; margin-top: 8px; font-size: 9.5px; color: #e2e8f0;
        }

        /* BANNER INFERIOR */
        .tactical-banner {
            position: absolute; bottom: 15px; left: 50%; transform: translateX(-50%); z-index: 10;
            background: rgba(10, 22, 40, 0.94); border: 1px solid #c084fc; color: #c084fc;
            padding: 10px 22px; border-radius: 20px; font-size: 11px; font-weight: bold;
            letter-spacing: 1px; box-shadow: 0 0 25px rgba(192, 132, 252, 0.25); text-align: center;
            max-width: 800px; width: 90%;
        }

        .hud-panel::-webkit-scrollbar { width: 4px; }
        .hud-panel::-webkit-scrollbar-track { background: rgba(0,0,0,0.3); }
        .hud-panel::-webkit-scrollbar-thumb { background: #38bdf8; border-radius: 2px; }
    </style>
</head>
<body>

    <div id="canvas-container">
        <canvas id="spaceCanvas"></canvas>
    </div>

    <!-- PAINEL DE CONTROLE DA NAVE E SISTEMAS -->
    <div class="hud-panel">
        <div class="hud-header">
            <span>NAVE ESPACIAL STAR-EXPLORER</span>
            <span class="badge-nasa">NASA // SPACEX</span>
        </div>

        <div class="section-title">INTEGRAÇÃO TRÍADE QUANTICA</div>
        <div class="triad-box">
            <div class="triad-item">
                <span>1. PROPULSÃO / CAMPO MAGNETO (MHD):</span>
                <span class="triad-status status-mhd" id="st-mhd">PLASMA ATIVO</span>
            </div>
            <div class="triad-item">
                <span>2. ESCUDO RESSONANTE MOLECULAR (SAMS):</span>
                <span class="triad-status status-sams" id="st-sams">FREQUÊNCIA SÔNICA</span>
            </div>
            <div class="triad-item">
                <span>3. PROCESSADOR / BIO-DESINTEGRADOR (SROS):</span>
                <span class="triad-status status-sros" id="st-sros">IA QUÂNTICA (0.12ms)</span>
            </div>
        </div>

        <div class="section-title">DETECÇÃO & HIGIENIZAÇÃO DE CONTAGIO</div>
        
        <button class="btn-mode active" id="btn-int_scan" onclick="setSimulation('int_scan')">
            <span>1. CABINE INTERNA: VARREDURA QUÂNTICA</span>
            <div style="width:8px; height:8px; border-radius:50%; background:#38bdf8;"></div>
        </button>

        <button class="btn-mode" id="btn-int_clean" onclick="setSimulation('int_clean')">
            <span>2. CABINE INTERNA: ESTERILIZAÇÃO TRÍADE</span>
            <div style="width:8px; height:8px; border-radius:50%; background:#34d399;"></div>
        </button>

        <button class="btn-mode" id="btn-ext_scan" onclick="setSimulation('ext_scan')">
            <span>3. CASCO EXTERNO: SUJEIRA & POEIRA CÓSMICA</span>
            <div style="width:8px; height:8px; border-radius:50%; background:#fbbf24;"></div>
        </button>

        <button class="btn-mode" id="btn-ext_clean" onclick="setSimulation('ext_clean')">
            <span>4. CASCO EXTERNO: DESINTEGRAÇÃO MHD</span>
            <div style="width:8px; height:8px; border-radius:50%; background:#c084fc;"></div>
        </button>

        <div class="section-title">EXPLORAÇÃO DE PLANETAS & LUAS (GELO / SUBSOLO)</div>

        <button class="btn-mode" id="btn-mars" onclick="setSimulation('mars')">
            <span>5. MARTE: CAPAS DE GELO & REGOLITO</span>
            <div style="width:8px; height:8px; border-radius:50%; background:#ef4444;"></div>
        </button>

        <button class="btn-mode" id="btn-europa" onclick="setSimulation('europa')">
            <span>6. EUROPA (JÚPITER): CÓRTEX DE GELO & OCEANO</span>
            <div style="width:8px; height:8px; border-radius:50%; background:#60a5fa;"></div>
        </button>

        <button class="btn-mode" id="btn-enceladus" onclick="setSimulation('enceladus')">
            <span>7. ENCÉLADO (SATURNO): CRIOVULCÕES & GÊISERES</span>
            <div style="width:8px; height:8px; border-radius:50%; background:#f43f5e;"></div>
        </button>

        <button class="btn-mode" id="btn-titan" onclick="setSimulation('titan')">
            <span>8. TITÃ: MARES DE METANO & MANTO ROCHOSO</span>
            <div style="width:8px; height:8px; border-radius:50%; background:#f97316;"></div>
        </button>

        <button class="btn-mode" id="btn-moon" onclick="setSimulation('moon')">
            <span>9. LUA: CRATERAS SOMBRIAS & GELO POLAR</span>
            <div style="width:8px; height:8px; border-radius:50%; background:#94a3b8;"></div>
        </button>
    </div>

    <!-- PAINEL DE BANCO DE DADOS QUÂNTICO SROS -->
    <div class="quantum-panel">
        <div class="quantum-header">
            <span>SROS COMPUTAÇÃO QUÂNTICA</span>
            <span>TEMPO: 0.12 ms</span>
        </div>

        <div class="metric-row">
            <span>BANCO DE DADOS ELEMENTAR:</span>
            <span class="metric-val">TABELA PERIÓDICA COMPLETA</span>
        </div>

        <div class="metric-row">
            <span>ESPECTRO BIOLÓGICO:</span>
            <span class="metric-val">TODOS VÍRUS/BACTÉRIAS</span>
        </div>

        <div class="metric-row">
            <span>TAXA DE CONTAGEM / CONTAMINAÇÃO:</span>
            <span class="metric-val" id="val-microbe-count">1,482 PARTICULAS</span>
        </div>

        <div class="chem-card" id="chem-details">
            <strong style="color:#34d399;">ANÁLISE MOLECULAR EM TEMPO REAL:</strong><br>
            • Patógeno: <i>Staphylococcus & Influenza A</i><br>
            • Composição: Proteínas, Capsídeo Lipídico (C, H, O, N, P)<br>
            • Sujeira Ext.: Regolito, Óxido de Ferro (Fe₂O₃), Silicatos<br>
            • Ação SROS: Vibração Ressonante UV-C Quântica
        </div>
    </div>

    <!-- BANNER DE STATUS TÁTICO -->
    <div class="tactical-banner" id="info-banner">
        INICIALIZANDO SIMULAÇÃO: VARREDURA DA CABINE INTERNA DA NAVE ESPACIAL.
    </div>

    <script>
        const canvas = document.getElementById('spaceCanvas');
        const ctx = canvas.getContext('2d');

        // AUDIO SYNTHESIZER
        const AudioCtx = window.AudioContext || window.webkitAudioContext;
        let audioCtx = null;

        function playSound(freq = 440, type = 'sine', duration = 0.1, vol = 0.03) {
            try {
                if (!audioCtx) audioCtx = new AudioCtx();
                if (audioCtx.state === 'suspended') audioCtx.resume();
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = type;
                osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
                gain.gain.setValueAtTime(vol, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + duration);
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.start();
                osc.stop(audioCtx.currentTime + duration);
            } catch(e) {}
        }

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        let currentSim = 'int_scan';
        let frameCount = 0;

        // ENTIDADES DE CONTAMINAÇÃO (BACTÉRIAS, VÍRUS, FUNGOS, SUJEIRA)
        const contaminants = [];
        const stars = [];

        for (let i = 0; i < 150; i++) {
            stars.push({
                x: Math.random() * window.innerWidth,
                y: Math.random() * window.innerHeight,
                size: Math.random() * 2,
                alpha: Math.random()
            });
        }

        function generateContaminants(count, isInternal = true) {
            contaminants.length = 0;
            const types = ['bacteria', 'virus', 'fungus', 'dirt'];
            for (let i = 0; i < count; i++) {
                contaminants.push({
                    x: isInternal ? (canvas.width * 0.25 + Math.random() * canvas.width * 0.5) : (canvas.width * 0.2 + Math.random() * canvas.width * 0.6),
                    y: isInternal ? (canvas.height * 0.3 + Math.random() * canvas.height * 0.4) : (canvas.height * 0.2 + Math.random() * canvas.height * 0.6),
                    vx: (Math.random() - 0.5) * 1.5,
                    vy: (Math.random() - 0.5) * 1.5,
                    type: types[Math.floor(Math.random() * types.length)],
                    size: 4 + Math.random() * 6,
                    alive: true
                });
            }
        }
        generateContaminants(80, true);

        function setSimulation(mode) {
            currentSim = mode;
            document.querySelectorAll('.btn-mode').forEach(b => b.classList.remove('active'));
            document.getElementById(`btn-${mode}`).classList.add('active');

            const banner = document.getElementById('info-banner');
            const chem = document.getElementById('chem-details');

            if (mode === 'int_scan') {
                generateContaminants(90, true);
                banner.innerText = "CABINE INTERNA: VARREDURA QUÂNTICA SROS DETECTA VÍRUS, BACTÉRIAS E ESPOROS EM 0.12ms.";
                chem.innerHTML = `<strong style="color:#38bdf8;">DETECÇÃO CABINE:</strong><br>• Micro-organismos: <i>Bacteria/Virus</i><br>• Tabela Periódica: C, H, O, N, P, S<br>• Estado: Ativo nas superfícies e ar`;
                playSound(600, 'sine');
            } else if (mode === 'int_clean') {
                banner.innerText = "HIGIENIZAÇÃO CABINE: TRÍADE (MHD + SAMS + SROS) DESINTEGRA E ESTERILIZA 100% DOS PATÓGENOS.";
                chem.innerHTML = `<strong style="color:#34d399;">ESTERILIZAÇÃO ATIVA:</strong><br>• SAMS: Frequência Ressonante Sônica<br>• SROS: Pulso Quântico desintegra RNA/DNA<br>• Resultado: Ar e superfícies 100% estéreis`;
                playSound(880, 'triangle');
            } else if (mode === 'ext_scan') {
                generateContaminants(120, false);
                banner.innerText = "CASCO EXTERNO: DETECÇÃO DE POEIRA CÓSMICA, SILICATOS E REGOLITO ADERIDO AO ESCUDO.";
                chem.innerHTML = `<strong style="color:#fbbf24;">VARREDURA EXTERNA:</strong><br>• Detritos: Regolito, Fe₂O₃, SiO₂<br>• Partículas micro-meteoríticas<br>• Identificação Instantânea via SROS AI`;
                playSound(400, 'square');
            } else if (mode === 'ext_clean') {
                banner.innerText = "CASCO EXTERNO: PROPULSÃO MHD E CAMPO SAMS EXPULSAM SUJEIRA E DESTRUIÇÃO TÉRMICA.";
                chem.innerHTML = `<strong style="color:#c084fc;">LIMPEZA EXTERNA MHD:</strong><br>• Plasma Magnetohidrodinâmico repele silicatos<br>• Campo SAMS expele poeira do casco<br>• Escudo exterior com frictão zero`;
                playSound(1200, 'sawtooth');
            } else if (mode === 'mars') {
                banner.innerText = "EXPLORAÇÃO MARTE: VARREDURA DE SUB-SOLO E CAPAS DE GELO POLAR (CO₂ E H₂O) VIA RADAR SROS.";
                chem.innerHTML = `<strong style="color:#ef4444;">ANÁLISE DE MARTE:</strong><br>• Superfície: Óxido de Ferro, Basalto<br>• Sub-solo: Gelo de Água (H₂O) e Gelo Seco (CO₂)<br>• Profundidade de Varredura: 45 metros`;
            } else if (mode === 'europa') {
                banner.innerText = "EXPLORAÇÃO EUROPA (JÚPITER): PERFURAÇÃO DE CÓRTEX DE GELO CRISTALINO E DETECÇÃO DE OCEANO SUB-SUPERFICIAL.";
                chem.innerHTML = `<strong style="color:#60a5fa;">ANÁLISE DE EUROPA:</strong><br>• Crosta: Gelo puro de H₂O (-160°C)<br>• Sub-solo: Oceano Líquido Salino com Fontes Hidrotermais<br>• Sinais Biológicos: Em análise SROS`;
            } else if (mode === 'enceladus') {
                banner.innerText = "EXPLORAÇÃO ENCÉLADO (SATURNO): AMOSTRAGEM DE GÊISERES DE CRIOVULCÕES E GELO DE SUBSOLO.";
                chem.innerHTML = `<strong style="color:#f43f5e;">ANÁLISE DE ENCÉLADO:</strong><br>• Plumas: Vapor de Água, Metano, Sais, Macromoléculas Orgânicas<br>• Mantos de Gelo ativo em expansão`;
            } else if (mode === 'titan') {
                banner.innerText = "EXPLORAÇÃO TITÃ (SATURNO): MAPPING DE MARES DE METANO/ETANO LÍQUIDO E MANTO ROCHOSO CONGELADO.";
                chem.innerHTML = `<strong style="color:#f97316;">ANÁLISE DE TITÃ:</strong><br>• Atmosfera: Nitrogênio denso & Metano<br>• Mares: Metano/Etano Líquido (-179°C)<br>• Crosta: Gelo de Hidrocarbonetos`;
            } else if (mode === 'moon') {
                banner.innerText = "EXPLORAÇÃO LUA POLO SUL: SONDA DE PERFURAÇÃO DE GELO EM CRATERAS DE SOMBRA PERPÉTUA.";
                chem.innerHTML = `<strong style="color:#94a3b8;">ANÁLISE LUNAR:</strong><br>• Cratera Shackleton: Pockets de Gelo de H₂O<br>• Regolito Anortosítico rico em Titânio e Hélio-3`;
            }
        }

        // DESENHAR NAVE ESPACIAL REALISTA (NASA / SPACEX STYLE)
        function drawSpaceship(cx, cy, isInterior = false) {
            ctx.save();
            ctx.translate(cx, cy);

            if (!isInterior) {
                // VISÃO EXTERNA REALISTA (TIPO STARSHIP / ORION HYBRID)
                // Propulsão MHD Plasma na Traseira
                ctx.shadowColor = '#38bdf8'; ctx.shadowBlur = 25;
                ctx.fillStyle = 'rgba(56, 189, 248, 0.8)';
                ctx.beginPath();
                ctx.moveTo(-160, -20); ctx.lineTo(-240 + Math.sin(frameCount * 0.4) * 20, 0); ctx.lineTo(-160, 20);
                ctx.closePath(); ctx.fill();

                // Corpo de Liga de Aço Inoxidável Polido
                ctx.shadowBlur = 0;
                const gradHull = ctx.createLinearGradient(0, -60, 0, 60);
                gradHull.addColorStop(0, '#e2e8f0');
                gradHull.addColorStop(0.5, '#64748b');
                gradHull.addColorStop(1, '#1e293b');

                ctx.fillStyle = gradHull;
                ctx.strokeStyle = '#38bdf8';
                ctx.lineWidth = 2;

                ctx.beginPath();
                ctx.moveTo(180, 0); // Nariz
                ctx.quadraticCurveTo(120, -55, -120, -55); // Topo
                ctx.lineTo(-150, -35);
                ctx.lineTo(-150, 35);
                ctx.lineTo(-120, 55);
                ctx.quadraticCurveTo(120, 55, 180, 0); // Base
                ctx.closePath();
                ctx.fill(); ctx.stroke();

                // Escudo de Calor / Placas Cerâmicas Negras (Estilo SpaceX)
                ctx.fillStyle = '#0f172a';
                ctx.beginPath();
                ctx.moveTo(180, 0);
                ctx.quadraticCurveTo(120, 55, -120, 55);
                ctx.lineTo(-150, 35);
                ctx.lineTo(120, 20);
                ctx.closePath(); ctx.fill();

                // Aletas de Manobra Aeroespaciais (Flaps)
                ctx.fillStyle = '#334155';
                ctx.fillRect(100, -70, 30, 20);
                ctx.fillRect(100, 50, 30, 20);
                ctx.fillRect(-140, -80, 45, 30);
                ctx.fillRect(-140, 50, 45, 30);

                // Logotipo NASA / SROS
                ctx.fillStyle = '#ef4444';
                ctx.font = 'bold 12px Arial';
                ctx.fillText("NASA", -20, -15);
                ctx.fillStyle = '#38bdf8';
                ctx.font = 'bold 10px monospace';
                ctx.fillText("SROS QUANTUM", -35, 15);

            } else {
                // VISÃO INTERNA DA CABINE (COCKPIT & LABORATÓRIO)
                ctx.fillStyle = '#0b1329';
                ctx.strokeStyle = '#38bdf8';
                ctx.lineWidth = 3;
                ctx.roundRect(-250, -140, 500, 280, 20);
                ctx.fill(); ctx.stroke();

                // Divisões da Cabine (Cockpit, Lab, Suporte de Vida)
                ctx.strokeStyle = 'rgba(56, 189, 248, 0.3)';
                ctx.lineWidth = 1.5;
                ctx.strokeRect(-230, -120, 150, 240); // Cockpit
                ctx.strokeRect(-70, -120, 160, 240);  // Laboratório Bio
                ctx.strokeRect(100, -120, 130, 240);  // Suporte de Vida

                // Rótulos Internos
                ctx.fillStyle = '#94a3b8'; ctx.font = '10px monospace';
                ctx.fillText("COCKPIT // IA", -210, -100);
                ctx.fillText("LABORATÓRIO SROS", -50, -100);
                ctx.fillText("SUPORTE VIDA", 115, -100);

                // Telas de Controle MFD (Multi-Function Displays)
                ctx.fillStyle = 'rgba(56, 189, 248, 0.2)';
                ctx.fillRect(-220, -80, 130, 70);
                ctx.fillRect(-60, -80, 140, 90);
            }

            ctx.restore();
        }

        // RENDERIZAÇÃO DE AMBIENTES PLANETÁRIOS COMPLETOS (MARTE, EUROPA, ENCÉLADO, TITÃ, LUA)
        function drawPlanetEnvironment(planet) {
            const h = canvas.height;
            const w = canvas.width;

            if (planet === 'mars') {
                // Marte: Céu Vermelho/Alaranjado, Superfície de Regolito, Capa de Gelo de Sub-solo
                const sky = ctx.createLinearGradient(0, 0, 0, h * 0.5);
                sky.addColorStop(0, '#2b0907'); sky.addColorStop(1, '#7f1d1d');
                ctx.fillStyle = sky; ctx.fillRect(0, 0, w, h * 0.5);

                // Solo de Marte
                ctx.fillStyle = '#991b1b'; ctx.fillRect(0, h * 0.5, w, h * 0.5);

                // Sub-solo Radar SROS (Mostrando Camada de Gelo de Água)
                ctx.fillStyle = '#1e293b'; ctx.fillRect(0, h * 0.75, w, h * 0.25);
                ctx.fillStyle = '#38bdf8'; ctx.globalAlpha = 0.6;
                ctx.fillRect(0, h * 0.8, w, 40); // Camada de Gelo
                ctx.globalAlpha = 1.0;

                ctx.fillStyle = '#38bdf8'; ctx.font = 'bold 11px monospace';
                ctx.fillText("RADAR SROS: CAMADA DE GELO DE ÁGUA (H₂O) DETECTADA A 12m DE PROFUNDIDADE", 50, h * 0.83);

            } else if (planet === 'europa') {
                // Europa: Júpiter Gigante no Céu, Crosta de Gelo Cristalino Racheda, Oceano Líquido Profundo
                // Júpiter ao Fundo
                ctx.fillStyle = '#ca8a04'; ctx.beginPath(); ctx.arc(w * 0.8, h * 0.2, 120, 0, Math.PI * 2); ctx.fill();

                // Crosta de Gelo
                ctx.fillStyle = '#93c5fd'; ctx.fillRect(0, h * 0.45, w, h * 0.2);
                // Oceano Sub-superficial Líquido
                ctx.fillStyle = '#1e3a8a'; ctx.fillRect(0, h * 0.65, w, h * 0.35);

                ctx.fillStyle = '#34d399'; ctx.font = 'bold 11px monospace';
                ctx.fillText("OCEANO SUB-SUPERFICIAL SALINO (LIQUIDO) // TEMPERATURA: 4°C (FONTES HIDROTERMAIS)", 50, h * 0.75);

            } else if (planet === 'enceladus') {
                // Encélado: Anéis de Saturno no Céu, Gêiseres / Criovulcões Ativos
                ctx.strokeStyle = '#e2e8f0'; ctx.lineWidth = 10;
                ctx.beginPath(); ctx.arc(w * 0.2, h * 0.1, 180, 0, Math.PI); ctx.stroke();

                // Gelo Brilhante
                ctx.fillStyle = '#f1f5f9'; ctx.fillRect(0, h * 0.5, w, h * 0.5);

                // Pluma do Criovulcão
                ctx.fillStyle = 'rgba(244, 63, 94, 0.4)';
                ctx.beginPath();
                ctx.moveTo(w * 0.5, h * 0.5);
                ctx.lineTo(w * 0.4, 0); ctx.lineTo(w * 0.6, 0);
                ctx.closePath(); ctx.fill();

            } else if (planet === 'titan') {
                // Titã: Atmosfera Laranja Densa, Mar de Metano Líquido
                ctx.fillStyle = '#c2410c'; ctx.fillRect(0, 0, w, h * 0.5);
                // Mar de Metano
                ctx.fillStyle = '#f97316'; ctx.fillRect(0, h * 0.5, w, h * 0.5);
                ctx.fillStyle = '#7c2d12'; ctx.fillRect(0, h * 0.8, w, h * 0.2); // Manto Congelado

            } else if (planet === 'moon') {
                // Lua: Céu Preto, Cratera Escura do Polo Sul, Bolsão de Gelo Cristalino
                ctx.fillStyle = '#0f172a'; ctx.fillRect(0, h * 0.5, w, h * 0.5);
                ctx.fillStyle = '#38bdf8'; ctx.fillRect(w * 0.3, h * 0.7, w * 0.4, h * 0.2); // Gelo Lunar
                ctx.fillStyle = '#f8fafc'; ctx.font = 'bold 11px monospace';
                ctx.fillText("CRATERA SHACKLETON: BOLSÃO DE GELO POLAR LUNAR PRESERVADO", w * 0.3, h * 0.68);
            }
        }

        // SIMULAÇÃO PRINCIPAL E LOOP DE RENDERIZAÇÃO
        function render() {
            frameCount++;
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            const cx = canvas.width / 2;
            const cy = canvas.height / 2;

            // RENDERIZAR ESPAÇO E ESTRELAS
            if (!['mars', 'europa', 'enceladus', 'titan', 'moon'].includes(currentSim)) {
                ctx.fillStyle = '#030712'; ctx.fillRect(0, 0, canvas.width, canvas.height);
                stars.forEach(s => {
                    ctx.fillStyle = `rgba(255, 255, 255, ${s.alpha})`;
                    ctx.fillRect(s.x, s.y, s.size, s.size);
                });
            } else {
                drawPlanetEnvironment(currentSim);
            }

            // MÓDULOS DE SCAN E ESTERILIZAÇÃO TRÍADE
            if (currentSim === 'int_scan' || currentSim === 'int_clean') {
                drawSpaceship(cx, cy, true);

                // Varredura Laser SROS Quântica
                const scanY = cy - 130 + (frameCount * 4) % 260;
                ctx.strokeStyle = currentSim === 'int_clean' ? '#34d399' : '#38bdf8';
                ctx.lineWidth = 2;
                ctx.beginPath(); ctx.moveTo(cx - 240, scanY); ctx.lineTo(cx + 240, scanY); ctx.stroke();

                // Desenhar Contaminantes na Cabine
                let aliveCount = 0;
                contaminants.forEach(c => {
                    if (currentSim === 'int_clean') c.alive = false; // Desintegração ativa

                    if (c.alive) {
                        aliveCount++;
                        ctx.fillStyle = c.type === 'virus' ? '#ef4444' : '#fbbf24';
                        ctx.beginPath(); ctx.arc(c.x, c.y, c.size, 0, Math.PI * 2); ctx.fill();
                        // Alvo SROS
                        ctx.strokeStyle = 'rgba(239, 68, 68, 0.5)';
                        ctx.strokeRect(c.x - 8, c.y - 8, 16, 16);
                    }
                });
                document.getElementById('val-microbe-count').innerText = `${aliveCount} PATÓGENOS`;

            } else if (currentSim === 'ext_scan' || currentSim === 'ext_clean') {
                drawSpaceship(cx, cy, false);

                if (currentSim === 'ext_clean') {
                    // Pulso MHD + SAMS desintegrando sujeira externa
                    ctx.strokeStyle = '#c084fc'; ctx.lineWidth = 4;
                    ctx.shadowColor = '#c084fc'; ctx.shadowBlur = 20;
                    ctx.beginPath(); ctx.ellipse(cx, cy, 220 + Math.sin(frameCount * 0.2) * 20, 90, 0, 0, Math.PI * 2); ctx.stroke();
                    ctx.shadowBlur = 0;
                }

                let aliveCount = 0;
                contaminants.forEach(c => {
                    if (currentSim === 'ext_clean') c.alive = false;
                    if (c.alive) {
                        aliveCount++;
                        ctx.fillStyle = '#fbbf24';
                        ctx.fillRect(c.x, c.y, c.size, c.size);
                    }
                });
                document.getElementById('val-microbe-count').innerText = `${aliveCount} PARTICULAS SUJEIRA`;

            } else {
                // MODO DE EXPLORAÇÃO PLANETÁRIA COM NAVE EM ÓRBITA / POUSADA
                drawSpaceship(cx, cy - 80, false);

                // Feixe de Sondagem e Perfuração Quântica SROS
                ctx.strokeStyle = '#34d399'; ctx.lineWidth = 3;
                ctx.setLineDash([6, 6]);
                ctx.beginPath(); ctx.moveTo(cx, cy - 80); ctx.lineTo(cx, canvas.height); ctx.stroke();
                ctx.setLineDash([]);
                document.getElementById('val-microbe-count').innerText = "0 (ESPECTRO ESTÉRIL)";
            }

            requestAnimationFrame(render);
        }

        // INICIALIZAR RENDERIZAÇÃO
        render();
    </script>
</body>
</html>
