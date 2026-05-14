<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EcoTrack Pro - Global Grid Data</title>
    <link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Work+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --earth-clay: #C1502E; --earth-rust: #8B3A1E; --earth-sand: #E8C39E;
            --earth-moss: #5A7247; --earth-forest: #2D4A2B; --earth-soil: #3A2C28;
            --earth-stone: #8B8680; --alert-red: #D62828; --warning-amber: #F77F00;
            --bg-light: #F5F1E8; --text-dark: #1A1A1A;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Work Sans', sans-serif; background: var(--bg-light); color: var(--text-dark); }

        .header { background: var(--earth-soil); color: var(--bg-light); padding: 2rem; border-bottom: 8px solid var(--earth-clay); }
        .header-content { max-width: 1200px; margin: 0 auto; }
        
        .nav-tabs { display: flex; gap: 10px; margin-top: 1.5rem; }
        .tab-btn { 
            background: transparent; border: 2px solid var(--earth-sand); color: var(--earth-sand); 
            padding: 0.5rem 1rem; cursor: pointer; font-family: 'Space Mono'; font-weight: 700;
        }
        .tab-btn.active { background: var(--earth-sand); color: var(--earth-soil); }

        .container { max-width: 1200px; margin: 0 auto; padding: 2rem; }
        .tab-content { display: none; }
        .tab-content.active { display: block; animation: fadeIn 0.4s ease; }

        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 1.5rem; }
        .card { background: white; border: 4px solid var(--text-dark); padding: 1.5rem; box-shadow: 6px 6px 0 rgba(0,0,0,0.1); }
        .card-header { font-family: 'Space Mono'; font-weight: 700; text-transform: uppercase; color: var(--earth-clay); border-bottom: 2px solid #eee; margin-bottom: 1rem; }

        label { display: block; font-family: 'Space Mono'; font-size: 0.75rem; font-weight: 700; margin: 10px 0 5px; }
        input, select { width: 100%; padding: 0.8rem; border: 3px solid var(--text-dark); font-family: inherit; font-size: 1rem; }

        .btn { 
            background: var(--earth-clay); color: white; border: none; padding: 1rem; 
            font-family: 'Space Mono'; font-weight: 700; cursor: pointer; width: 100%; margin-top: 1rem;
            box-shadow: 4px 4px 0 var(--earth-rust);
        }
        .btn:hover { transform: translate(-2px, -2px); box-shadow: 6px 6px 0 var(--earth-rust); }

        .result-box { 
            margin-top: 2rem; padding: 2rem; border: 4px solid var(--text-dark); background: white; text-align: center; 
            display: none;
        }
        .big-number { font-size: 4rem; font-family: 'Space Mono'; font-weight: 700; color: var(--earth-rust); line-height: 1; }
        
        .chart-flex { display: flex; align-items: flex-end; height: 200px; gap: 15px; margin-top: 2rem; padding-bottom: 20px; border-bottom: 2px solid #eee; }
        .bar { flex: 1; background: var(--earth-clay); border: 2px solid var(--text-dark); position: relative; min-width: 60px; }
        .bar-label { position: absolute; bottom: -25px; width: 100%; font-size: 0.7rem; font-weight: 700; text-align: center; }
    </style>
</head>
<body>

<header class="header">
    <div class="header-content">
        <h1 style="font-family: 'Space Mono';">ECOTRACK <span style="color: var(--earth-sand)">PRO</span></h1>
        <nav class="nav-tabs">
            <button class="tab-btn active" data-tab="calc">Calculadora Global</button>
            <button class="tab-btn" data-tab="info">Dados de Grade</button>
        </nav>
    </div>
</header>

<div class="container">
    <div id="calc" class="tab-content active">
        <div class="grid">
            <!-- COLUNA ENERGIA -->
            <div class="card">
                <div class="card-header">⚡ Energia Elétrica</div>
                <label>País / Região</label>
                <select id="country-grid">
                    <option value="0.089">Brasil (Hidro/Eólica/Solar)</option>
                    <option value="0.055">França (Nuclear)</option>
                    <option value="0.370">EUA (Mix Gás/Renovável)</option>
                    <option value="0.530">China (Mix Carvão/Renovável)</option>
                    <option value="0.710">Índia (Majoritário Carvão)</option>
                    <option value="0.410">Alemanha (Mix Gás/Eólica)</option>
                    <option value="0.025">Islândia (Geotérmica)</option>
                    <option value="0.436">Média Global</option>
                </select>
                <label>Consumo Mensal (kWh)</label>
                <input type="number" id="kwh-val" value="150">
                <p style="font-size: 0.7rem; margin-top: 10px; opacity: 0.7;">*Fatores expressos em kg CO₂e por kWh (Fonte: Ember/IEA 2024).</p>
            </div>

            <!-- COLUNA TRANSPORTE -->
            <div class="card">
                <div class="card-header">🚗 Transporte</div>
                <label>Veículo</label>
                <select id="trans-val">
                    <option value="0.18">Carro Médio (Gasolina)</option>
                    <option value="0.12">Carro Médio (Flex/Etanol)</option>
                    <option value="0.05">Carro Elétrico (Média)</option>
                    <option value="0.08">Ônibus</option>
                    <option value="0">Bicicleta / Caminhada</option>
                </select>
                <label>Km rodados por semana</label>
                <input type="number" id="km-val" value="100">
            </div>

            <!-- COLUNA DIETA -->
            <div class="card">
                <div class="card-header">🍽️ Alimentação</div>
                <label>Padrão Alimentar</label>
                <select id="diet-val">
                    <option value="2.5">Onívoro (Muita carne vermelha)</option>
                    <option value="1.7">Onívoro (Equilibrado)</option>
                    <option value="0.9">Vegetariano</option>
                    <option value="0.5">Vegano</option>
                </select>
            </div>
        </div>

        <button class="btn" onclick="runGlobalCalc()">GERAR RELATÓRIO DE IMPACTO</button>

        <div id="results-area" class="result-box">
            <p style="text-transform: uppercase; font-weight: 800; font-size: 0.8rem;">Sua pegada anual estimada é:</p>
            <div class="big-number"><span id="final-co2">0.0</span> <small style="font-size: 1.5rem;">tCO₂e</small></div>
            <p id="context-msg" style="margin-top: 10px; font-weight: 600;"></p>

            <div class="chart-flex" id="chart-bars">
                <!-- Barras via JS -->
            </div>
        </div>
    </div>

    <div id="info" class="tab-content">
        <div class="card">
            <div class="card-header">Como as matrizes são calculadas?</div>
            <p>O impacto da sua eletricidade depende da <strong>intensidade de carbono</strong> da grade do seu país.</p>
            <br>
            <ul style="margin-left: 20px; line-height: 1.6;">
                <li><strong>Baixo Carbono (< 0.1 kg/kWh):</strong> Países como Brasil, França e Suécia (Hidro, Nuclear, Eólica).</li>
                <li><strong>Médio Carbono (0.1 - 0.4 kg/kWh):</strong> Países em transição como EUA e Reino Unido.</li>
                <li><strong>Alto Carbono (> 0.5 kg/kWh):</strong> Países dependentes de carvão como Índia, China e Polônia.</li>
            </ul>
        </div>
    </div>
</div>

<script>
    // Tab System
    document.querySelectorAll('.tab-btn').forEach(b => {
        b.addEventListener('click', () => {
            document.querySelectorAll('.tab-btn, .tab-content').forEach(el => el.classList.remove('active'));
            b.classList.add('active');
            document.getElementById(b.dataset.tab).classList.add('active');
        });
    });

    function runGlobalCalc() {
        // Obter valores
        const gridFactor = parseFloat(document.getElementById('country-grid').value);
        const kwhMonthly = parseFloat(document.getElementById('kwh-val').value);
        const transFactor = parseFloat(document.getElementById('trans-val').value);
        const kmWeekly = parseFloat(document.getElementById('km-val').value);
        const dietFactor = parseFloat(document.getElementById('diet-val').value);

        // Cálculos anuais (em toneladas)
        const energyYear = (kwhMonthly * 12 * gridFactor) / 1000;
        const transYear = (kmWeekly * 52 * transFactor) / 1000;
        const dietYear = dietFactor; // Já em toneladas

        const total = energyYear + transYear + dietYear;

        // Mostrar resultados
        document.getElementById('results-area').style.display = 'block';
        document.getElementById('final-co2').innerText = total.toFixed(2);

        // Mensagem de Contexto
        const msg = document.getElementById('context-msg');
        if(total < 2.5) {
            msg.innerText = "Excelente! Você está abaixo da meta do Acordo de Paris (2.1t por pessoa).";
            msg.style.color = "var(--earth-forest)";
        } else if (total < 4.7) {
            msg.innerText = "Você está na média mundial, mas precisamos reduzir para frear o aquecimento.";
            msg.style.color = "var(--warning-amber)";
        } else {
            msg.innerText = "Alerta: Seu impacto está alto. Considere reduzir voos e carne vermelha.";
            msg.style.color = "var(--alert-red)";
        }

        // Gráfico
        renderBars({
            "Energia": energyYear,
            "Transporte": transYear,
            "Dieta": dietYear
        });

        window.scrollTo({ top: document.body.scrollHeight, behavior: 'smooth' });
    }

    function renderBars(data) {
        const chart = document.getElementById('chart-bars');
        const max = Math.max(...Object.values(data));
        
        chart.innerHTML = Object.entries(data).map(([label, val]) => `
            <div class="bar" style="height: ${(val/max)*100}%">
                <div style="position: absolute; top: -20px; width: 100%; text-align: center; font-size: 0.7rem; font-family: 'Space Mono'">${val.toFixed(2)}t</div>
                <div class="bar-label">${label}</div>
            </div>
        `).join('');
    }
</script>

</body>
</html>
